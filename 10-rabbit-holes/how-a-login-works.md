# How a Login Works

## The Full Journey

You enter your email and password on a login page and click "Sign In." What happens between that click and seeing your dashboard? Let's trace the entire flow — frontend, backend, database, security, and session management.

---

## Step 1: You Enter Your Credentials

The login form on the page captures your input:

```html
<form action="/login" method="POST">
    <input type="email" name="email" />
    <input type="password" name="password" />
    <button type="submit">Sign In</button>
</form>
```

When you click submit, the browser sends a POST request. Before it goes out:

1. **Client-side validation** — the form checks the email format, password length
2. **HTTPS** — the entire request is encrypted in transit (see `../04-internet/https.md`)

---

## Step 2: The Request Reaches the Server

```
POST /login HTTP/1.1
Host: example.com
Content-Type: application/json
Cookie: session=...  (if already logged in)

{
    "email": "alice@example.com",
    "password": "hunter2"
}
```

The server receives this request and routes it to the login handler.

---

## Step 3: The Server Looks Up the User

The server queries the database for a user with this email:

```sql
SELECT id, email, password_hash FROM users WHERE email = 'alice@example.com';
```

This query uses an index on the `email` column (see `../06-databases/indexes.md`) to find the user quickly without scanning every row.

**Important**: The database stores a **password hash**, not the actual password. See `../07-security/hashing.md`.

```
Database returns:
  id: 42
  email: alice@example.com
  password_hash: $2b$12$N9qo8u...  (bcrypt hash of the real password)
```

If no user exists with this email, the server returns an error. But for security, it shouldn't say "email not found" — that lets attackers enumerate accounts. It says "invalid email or password" regardless of which part is wrong.

---

## Step 4: Password Verification

The server has the stored hash but not the real password. It hashes the submitted password and compares:

```python
import bcrypt

# The password the user typed
submitted_password = "hunter2"

# The hash from the database
stored_hash = "$2b$12$N9qo8u..."

# bcrypt extracts the salt from the stored hash, re-hashes the input, and compares
if bcrypt.checkpw(submitted_password.encode(), stored_hash.encode()):
    # Passwords match!
    user_authenticated = True
else:
    # Wrong password
    user_authenticated = False
```

The key point: the server **never knows the actual password**. It only compares hashes. This is why if you forget your password, sites send a reset link — they can't tell you your password because they don't have it.

---

## Step 5: Rate Limiting (Brute Force Protection)

Before or after the password check, the server checks:

```python
# How many failed attempts from this IP in the last 15 minutes?
attempts = redis.incr(f"login_attempts:{ip_address}")
redis.expire(f"login_attempts:{ip_address}", 900)  # 15 min window

if attempts > 5:
    return "Too many attempts. Try again later.", 429
```

This prevents brute force attacks — someone trying every possible password. (See `../07-security/authentication.md`)

---

## Step 6: Create a Session

If the password matches, the server needs to "remember" that this user is now logged in. Two common approaches:

### Session-Based
```python
# Create a session record server-side
session_id = secrets.token_urlsafe(32)  # random, unguessable string
redis.setex(f"session:{session_id}", 3600, json.dumps({
    "user_id": 42,
    "email": "alice@example.com",
    "created_at": time.now()
}))

# Send the session ID to the browser as a cookie
response.set_cookie("session_id", session_id, httponly=True, secure=True, samesite="Lax")
```

The session data lives on the server (in Redis or a database). The browser only gets the session ID — a random, meaningless string.

### Token-Based (JWT)
```python
# Create a signed token containing the user's info
import jwt

token = jwt.encode({
    "user_id": 42,
    "email": "alice@example.com",
    "exp": datetime.utcnow() + timedelta(hours=1)
}, SECRET_KEY, algorithm="HS256")

# Send the token to the browser
return {"token": token}
```

The token itself contains the user data, signed with the server's secret key. The server doesn't need to store anything — it just verifies the signature.

---

## Step 7: The Browser Stores the Credential

The browser receives the session cookie or token and stores it:

**Cookie (session-based):**
- Automatically sent with every future request to the same domain
- `HttpOnly` flag → JavaScript can't read it (prevents XSS theft)
- `Secure` flag → only sent over HTTPS
- `SameSite` flag → prevents CSRF attacks

**Token in localStorage (JWT):**
- Stored by JavaScript in the browser
- Must be manually attached to each request in the Authorization header
- More vulnerable to XSS (JavaScript can read it)

---

## Step 8: Subsequent Requests Are Authenticated

Now when Alice visits `/dashboard`:

```
GET /dashboard HTTP/1.1
Cookie: session_id=abc123xyz...
```

The server:
1. Extracts the session ID from the cookie
2. Looks up the session in Redis/database
3. Finds: `session_id=abc123xyz → user_id=42`
4. Loads user 42's data
5. Renders the dashboard for Alice

```
Request → [Auth middleware: extract session/token → identify user] → [Route handler: serve dashboard]
```

---

## Step 9: Authorization Checks

Being logged in (authenticated) doesn't mean you can do everything. The server also checks authorization (see `../07-security/authorization.md`):

```python
@app.route("/admin/users", methods=["DELETE"])
@require_auth  # checks authentication
@require_role("admin")  # checks authorization
def delete_user(user_id):
    ...
```

If Alice is a regular user trying to access an admin endpoint, she gets `403 Forbidden` — authenticated, but not authorized.

---

## Step 10: Logout

When Alice clicks "Log Out":
1. The server **destroys the session** (deletes from Redis/database)
   ```python
   redis.delete(f"session:{session_id}")
   ```
2. The browser **clears the cookie**
3. Future requests no longer include the session ID
4. The session can't be used even if the cookie was stolen earlier

With JWT, logout is trickier — the token is stateless, so the server can't "delete" it. Solutions:
- Short expiration times
- A blocklist of revoked tokens
- Refresh token rotation

---

## The Full Flow

```
User enters email + password
    ↓
Browser sends POST /login (over HTTPS)
    ↓
Server queries database for user by email
    ↓
Server hashes submitted password, compares with stored hash
    ↓
Rate limiting check (prevent brute force)
    ↓
If match: create session/token
    ↓
Send session cookie / JWT to browser
    ↓
Browser stores credential (cookie or localStorage)
    ↓
Future requests include the credential
    ↓
Server validates credential on each request (auth middleware)
    ↓
Server checks authorization (role/permission checks)
    ↓
Request is served or rejected (403)
    ↓
Logout: destroy session / invalidate token
```

---

## What Can Go Wrong

| Attack | What Happens | Prevention |
|---|---|---|
| **Password theft in transit** | Attacker intercepts login request | HTTPS (TLS encryption) |
| **Database breach** | Attacker steals stored hashes | bcrypt/Argon2 (slow hashing) |
| **Brute force** | Attacker tries many passwords | Rate limiting, account lockout |
| **Session hijacking** | Attacker steals session cookie | HttpOnly, Secure, SameSite flags |
| **XSS** | Malicious script steals token | Input sanitization, CSP headers |
| **CSRF** | Attacker tricks browser into making requests | CSRF tokens, SameSite cookies |
| **Credential stuffing** | Attacker uses leaked passwords from other sites | MFA, password monitoring |

Every layer matters. Security is a chain — it's only as strong as the weakest link.
