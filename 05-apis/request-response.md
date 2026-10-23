# Request and Response

## The Request-Response Cycle

Every web API interaction follows the same pattern: a client sends a **request**, the server processes it, and sends back a **response**. This happens over HTTP (see `../04-internet/http.md`).

```
Client                                    Server
  │                                        │
  │  ──── Request ───────────────────►     │
  │  (method, URL, headers, body)          │
  │                                        │  (process)
  │  ◄──── Response ──────────────────     │
  │  (status, headers, body)               │
```

---

## Anatomy of a Request

### The Method and URL
```
POST https://api.example.com/v1/users
```
- **Method**: `POST` (creating something)
- **URL**: `https://api.example.com/v1/users` (the endpoint)

### Headers
Metadata about the request:
```
Content-Type: application/json
Authorization: Bearer eyJhbGciOi...
Accept: application/json
```

### Body
The data you're sending (for POST/PUT/PATCH):
```json
{
  "name": "Alice",
  "email": "alice@example.com"
}
```

---

## Anatomy of a Response

### Status Code
```
201 Created
```
Tells you what happened: success, error, redirect, etc. See the HTTP status code table in `../04-internet/http.md`.

### Headers
```
Content-Type: application/json
Content-Length: 87
Cache-Control: no-store
```

### Body
The data the server returns:
```json
{
  "id": 42,
  "name": "Alice",
  "email": "alice@example.com",
  "created_at": "2026-10-23T10:30:00Z"
}
```

---

## Making a Request in Code

### Python (requests library)
```python
import requests

response = requests.post(
    "https://api.example.com/v1/users",
    headers={
        "Authorization": "Bearer token123",
        "Content-Type": "application/json"
    },
    json={"name": "Alice", "email": "alice@example.com"}
)

print(response.status_code)  # 201
print(response.json())       # the response body as a dict
```

### JavaScript (fetch)
```javascript
const response = await fetch("https://api.example.com/v1/users", {
  method: "POST",
  headers: {
    "Authorization": "Bearer token123",
    "Content-Type": "application/json"
  },
  body: JSON.stringify({ name: "Alice", email: "alice@example.com" })
});

const data = await response.json();
console.log(response.status);  // 201
console.log(data);
```

### Command Line (curl)
```bash
curl -X POST https://api.example.com/v1/users \
  -H "Authorization: Bearer token123" \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice", "email": "alice@example.com"}'
```

---

## JSON: The Lingua Franca of APIs

Most modern APIs use **JSON** (JavaScript Object Notation) for request and response bodies. It's lightweight, human-readable, and works across every language.

```json
{
  "name": "Alice",
  "age": 30,
  "skills": ["Python", "JavaScript"],
  "active": true,
  "address": null
}
```

JSON supports: strings, numbers, booleans, null, arrays, and objects (key-value pairs).

---

## Common API Patterns

### Pagination
Large datasets are returned in pages:
```
GET /api/users?page=2&limit=50
```
Response includes metadata:
```json
{
  "data": [...],
  "page": 2,
  "total_pages": 42,
  "total_items": 2100
}
```

### Rate Limiting
APIs limit how many requests you can make per time window:
```
Headers:
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1698070800
```

### Error Responses
```json
{
  "error": {
    "code": "INVALID_EMAIL",
    "message": "The email field must be a valid email address."
  }
}
```

---

## The Full Cycle

1. Build a request (method + URL + headers + body)
2. Send it over HTTP/HTTPS
3. Server receives, authenticates, processes
4. Server sends back a response (status + headers + body)
5. Client parses the response and acts on it

This cycle repeats for every API call. Understanding it is the foundation for working with any web API.
