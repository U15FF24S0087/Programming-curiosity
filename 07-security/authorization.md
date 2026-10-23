# Authorization

## What Is Authorization?

Authorization is the process of deciding **what you're allowed to do** after authentication confirms who you are.

```
Authentication: "You are Alice" ✓
Authorization: "Alice can view this page, but not edit it" ✓/✗
```

You can be authenticated but not authorized. A regular user is authenticated when they log in, but they can't delete another user's account — they lack authorization.

---

## Authentication vs Authorization

| | Authentication | Authorization |
|---|---|---|
| Question | Who are you? | What can you do? |
| Order | First | Second |
| Example | Password, fingerprint, token | Role check, permission check |
| Analogy | Showing your ID at the door | Being allowed into the VIP room or not |

You can't authorize without authenticating first (usually). Otherwise, who are you authorizing?

---

## Common Authorization Models

### Role-Based Access Control (RBAC)

Users are assigned **roles**, and roles have **permissions**. The system checks roles, not individual users.

```
Roles:
  admin    → can read, write, delete
  editor   → can read, write
  viewer   → can read

Users:
  Alice → admin
  Bob   → editor
  Charlie → viewer
```

```python
if user.role == "admin":
    delete_post(post_id)
elif user.role == "editor":
    edit_post(post_id)
else:
    view_post(post_id)
```

**Advantage**: simple, widely used, easy to manage.  
**Disadvantage**: gets complex when permissions don't fit neatly into roles.

### Attribute-Based Access Control (ABAC)

Authorization is based on **attributes** of the user, resource, and context.

```
Rule: A doctor can view a patient's medical record IF:
  - user.role == "doctor"
  - user.department == patient.department
  - action == "read"
  - time is within business hours
```

More flexible than RBAC but more complex to define and evaluate.

### Access Control Lists (ACLs)

Each resource has a list of who can do what with it.

```
document_42:
  Alice: [read, write]
  Bob: [read]
  everyone_else: []
```

Simple for small systems but hard to manage at scale (millions of resources × millions of users).

---

## Permission Checks in Practice

Every request that touches a resource should be checked:

```python
@app.route("/posts/<post_id>", methods=["DELETE"])
def delete_post(post_id):
    # 1. Authentication (handled by middleware)
    user = get_current_user()  # from token/session

    # 2. Authorization
    post = get_post(post_id)
    if user.id != post.author_id and user.role != "admin":
        return "Forbidden", 403

    # 3. Action
    delete(post_id)
    return "Deleted", 200
```

The `403 Forbidden` status code means "I know who you are, but you can't do that." Compare with `401 Unauthorized` which means "I don't know who you are."

---

## Principle of Least Privilege

Give each user or service the **minimum permissions** needed to do their job — nothing more.

- A content editor doesn't need database admin access
- A read-only analytics dashboard doesn't need write permissions
- A microservice that sends emails doesn't need to delete users

This limits the damage if an account is compromised. An attacker who takes over an editor account can't delete the entire database.

---

## API Authorization

For APIs, authorization is often done via tokens:

### Bearer Token
```
GET /api/users/me
Authorization: Bearer eyJhbGciOi...
```
The server decodes the token, extracts the user's ID and roles, and checks permissions.

### API Keys
```
GET /api/data
X-API-Key: abc123xyz
```
Simpler than JWT but less flexible. Often used for service-to-service auth.

### Scopes (OAuth)
OAuth tokens can include **scopes** — fine-grained permissions:

```
Token with scopes: ["read:profile", "read:email"]
→ Can read profile and email
→ Cannot write posts or delete data
```

The user consents to specific scopes when authorizing the app, so they know exactly what the app can do.

---

## Common Authorization Failures

| Failure | Description |
|---|---|
| **Missing checks** | A route forgets to verify permissions |
| **IDOR** (Insecure Direct Object Reference) | `/api/users/42` works for any ID — no check that the user owns resource 42 |
| **Privilege escalation** | A regular user finds a way to become admin |
| **Stale permissions** | User changes role but old permissions aren't revoked |
| **Client-side only checks** | Frontend hides a button but the API endpoint is unprotected |

The most important rule: **never trust the client**. Authorization must be enforced on the server. Hiding a button in the UI is not security — the API must reject the request regardless.
