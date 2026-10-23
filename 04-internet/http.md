# HTTP (HyperText Transfer Protocol)

## What Is HTTP?

HTTP is the protocol that defines how web browsers and servers communicate. When you open a website, your browser sends an HTTP request to a server, and the server sends back an HTTP response.

```
Browser                                  Server
   │  ─── GET /index.html HTTP/1.1 ──►     │
   │                                      │
   │  ◄── HTTP/1.1 200 OK + HTML ────     │
```

It's a **request-response** protocol: the client asks, the server answers. HTTP itself is just text — a set of rules for structuring that conversation.

---

## HTTP Requests

An HTTP request has three parts:

### 1. Request Line
```
GET /users/42 HTTP/1.1
```
- **Method**: `GET` (what you want to do)
- **Path**: `/users/42` (what you want)
- **Version**: `HTTP/1.1`

### 2. Headers
Key-value pairs with metadata about the request:
```
Host: api.example.com
User-Agent: Mozilla/5.0
Accept: application/json
Authorization: Bearer eyJhbGciOi...
```

### 3. Body (optional)
Data sent with the request — used for `POST` and `PUT`:
```
{"name": "Alice", "email": "alice@example.com"}
```

---

## HTTP Methods

| Method | Purpose | Has Body? | Idempotent? |
|---|---|---|---|
| **GET** | Retrieve data | No | Yes |
| **POST** | Create something | Yes | No |
| **PUT** | Replace/create at a specific URL | Yes | Yes |
| **PATCH** | Partially update | Yes | No |
| **DELETE** | Remove something | No | Yes |

**Idempotent** means calling it multiple times has the same effect as calling it once. `GET` is always idempotent. `POST` is not (sending the same POST twice creates two records).

---

## HTTP Responses

An HTTP response has three parts:

### 1. Status Line
```
HTTP/1.1 200 OK
```
- **Version**: `HTTP/1.1`
- **Status code**: `200`
- **Reason phrase**: `OK`

### 2. Headers
```
Content-Type: text/html
Content-Length: 1024
Cache-Control: max-age=3600
Set-Cookie: session=abc123; HttpOnly
```

### 3. Body (optional)
The actual content:
```html
<html>
  <body>Hello, World!</body>
</html>
```

---

## HTTP Status Codes

| Range | Meaning | Common Codes |
|---|---|---|
| **1xx** | Informational | 100 Continue |
| **2xx** | Success | 200 OK, 201 Created, 204 No Content |
| **3xx** | Redirection | 301 Moved Permanently, 304 Not Modified |
| **4xx** | Client error | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found |
| **5xx** | Server error | 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable |

The most famous is **404 Not Found** — the server couldn't find what you asked for.

---

## Stateless Protocol

HTTP is **stateless** — each request is independent. The server doesn't remember you between requests. This is simple and scalable, but means the web has no built-in concept of a "session."

To remember users (login state, shopping cart), we use:
- **Cookies** — small pieces of data the browser sends with every request
- **Tokens** (like JWT) — included in headers for authentication

The server reads these on each request to figure out who you are.

---

## HTTP/1.1 vs HTTP/2 vs HTTP/3

| Version | Key Improvement |
|---|---|
| HTTP/1.1 | Persistent connections, pipelining (1997) |
| HTTP/2 | Multiplexing (multiple requests over one connection), header compression |
| HTTP/3 | Built on QUIC (UDP-based), faster connection setup, no head-of-line blocking |

Most modern sites use HTTP/2 or HTTP/3, but the request-response model stays the same.

---

## What HTTP Is Not

HTTP is **not encrypted** by default. Data sent over HTTP can be read by anyone between you and the server. That's why we have HTTPS — see `https.md`.
