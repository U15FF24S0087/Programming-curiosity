# Authentication

## What Is Authentication?

Authentication is the process of verifying **who you are**. It answers the question: "Are you really the person you claim to be?"

```
You: "I'm Alice"
System: "Prove it"
You: [enters password]
System: "Welcome, Alice" ✓
```

This is different from **authorization** (what you're allowed to do), which comes after authentication. See `authorization.md`.

---

## The Three Factors of Authentication

Authentication methods are grouped into three categories ("something you..."):

| Factor | Examples |
|---|---|
| **Something you know** | Password, PIN, security questions |
| **Something you have** | Phone (SMS code), hardware key, authenticator app |
| **Something you are** | Fingerprint, face scan, iris, voice |

Using one factor (e.g., just a password) is **single-factor authentication**. Using two is **two-factor authentication (2FA)**. Using multiple is **multi-factor authentication (MFA)**.

The idea: if one factor is compromised, the attacker still can't get in.

---

## Password-Based Authentication

The most common method, despite its flaws.

### How It Should Work (Hashing)
```
Registration:
  User enters password "hunter2"
  Server hashes it: hash("hunter2") = $2b$10$N9qo8u...
  Server stores only the hash in the database

Login:
  User enters password "hunter2"
  Server hashes the input: hash("hunter2") = $2b$10$N9qo8u...
  Server compares hashes — they match → login OK
```

The server never stores the plain password. If the database is stolen, the attacker gets hashes, not passwords. See `hashing.md` for why this matters.

### What Goes Wrong
- **Weak passwords** — "password123" can be guessed in seconds
- **Reuse** — same password across sites; one breach compromises all
- **Plain text storage** — some services actually store passwords unhashed (bad)

---

## Session-Based Authentication

After you log in, the server needs to remember you for subsequent requests. Since HTTP is stateless (see `../04-internet/http.md`), we need a mechanism:

1. User logs in with username/password
2. Server validates, creates a **session** (stored server-side)
3. Server sends a **session ID** to the browser (in a cookie)
4. Browser sends the cookie with every subsequent request
5. Server reads the session ID, looks up the session, identifies the user

```
Login → Server creates session 42, sets cookie: session_id=42
Next request → Cookie: session_id=42 → Server: "Ah, that's Alice"
```

**Downside**: The server must store and manage all active sessions. For millions of users, this is memory and infrastructure overhead.

---

## Token-Based Authentication (JWT)

An alternative to sessions — the server doesn't store anything. Instead, it gives the client a signed **token**.

### JSON Web Tokens (JWT)
A JWT has three parts:
```
eyJhbGciOi...  .  eyJzdWIiOi...  .  SflKxwRJ...
  (header)         (payload)         (signature)
```

1. **Header**: algorithm used for signing
2. **Payload**: user data (user ID, roles, expiration time)
3. **Signature**: server signs the header + payload with a secret key

The client sends the token with each request (in the `Authorization` header). The server verifies the signature — if it's valid, the payload is trusted.

**Advantage**: serverless, stateless, scalable. No session storage needed.
**Disadvantage**: can't revoke a token before it expires (unless you maintain a blocklist).

---

## OAuth (Third-Party Login)

"Log in with Google/Facebook/GitHub" uses OAuth. Instead of creating a new account, you authenticate with a provider you already trust.

```
1. You click "Login with Google"
2. You're redirected to Google's login page
3. You log in to Google (Google authenticates you)
4. Google sends an authorization code back to the app
5. The app exchanges the code for an access token
6. The app uses the token to get your profile from Google
```

The app never sees your Google password. It only gets a token that proves Google verified you.

---

## Common Authentication Attacks

| Attack | Description |
|---|---|
| **Brute force** | Try every possible password |
| **Credential stuffing** | Use leaked username/password pairs from other breaches |
| **Phishing** | Fake login page tricks users into entering credentials |
| **Session hijacking** | Steal someone's session cookie/token |
| **Man-in-the-middle** | Intercept traffic between user and server (prevented by HTTPS) |

---

## Best Practices

- Use **HTTPS** — never send credentials over plain HTTP
- **Hash passwords** with a slow algorithm (bcrypt, Argon2)
- Enforce **strong passwords** or use **passkeys**
- Offer **MFA** for sensitive accounts
- Use **rate limiting** to slow down brute force
- Never store plain-text passwords
- Set reasonable **token expiration** times
