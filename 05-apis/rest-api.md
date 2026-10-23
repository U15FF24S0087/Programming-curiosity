# REST API

## What Is REST?

REST (Representational State Transfer) is a set of conventions for designing web APIs. It's not a protocol — it's a style. Most web APIs you'll encounter are "RESTful" or claim to be.

REST treats everything as a **resource** and uses standard HTTP methods to act on those resources.

---

## Resources and URLs

In REST, a resource is anything your API exposes — users, products, orders, articles. Each resource gets a URL:

```
/api/users          → collection of users
/api/users/42       → a specific user
/api/users/42/posts → posts belonging to user 42
```

URLs are nouns, not verbs. The HTTP method expresses the action.

---

## The HTTP Methods (REST Verbs)

| Method | URL | Action | Example |
|---|---|---|---|
| **GET** | `/api/users` | List all users | Returns `[...]` |
| **GET** | `/api/users/42` | Get one user | Returns `{id: 42, ...}` |
| **POST** | `/api/users` | Create a new user | Returns `201` + new user |
| **PUT** | `/api/users/42` | Replace user 42 | Returns updated user |
| **PATCH** | `/api/users/42` | Partially update user 42 | Returns updated user |
| **DELETE** | `/api/users/42` | Delete user 42 | Returns `204 No Content` |

The combination of method + URL tells the server exactly what to do.

---

## REST Principles

### 1. Statelessness
Each request contains everything the server needs to process it. The server doesn't store session state between requests. This makes REST APIs easy to scale — any server can handle any request.

### 2. Uniform Interface
All resources are accessed the same way — via URLs and HTTP methods. You don't need a special protocol or library to use a REST API; any HTTP client works.

### 3. Client-Server Separation
The client (frontend) and server (backend) are independent. The server provides data; the client decides how to display it. You can swap frontends or backends without affecting the other.

### 4. Layered System
The client doesn't know if it's talking directly to the server or through a proxy, load balancer, or gateway. This lets you add caching, load balancing, and security layers transparently.

---

## A Complete REST Example

### Create a user
```
POST /api/users
Content-Type: application/json

{
  "name": "Alice",
  "email": "alice@example.com"
}
```
**Response:**
```
201 Created
{
  "id": 42,
  "name": "Alice",
  "email": "alice@example.com",
  "created_at": "2026-10-23T10:30:00Z"
}
```

### Get the user
```
GET /api/users/42
```
**Response:**
```
200 OK
{
  "id": 42,
  "name": "Alice",
  "email": "alice@example.com"
}
```

### Update the user
```
PATCH /api/users/42
Content-Type: application/json

{ "email": "new.alice@example.com" }
```
**Response:**
```
200 OK
{
  "id": 42,
  "name": "Alice",
  "email": "new.alice@example.com"
}
```

### Delete the user
```
DELETE /api/users/42
```
**Response:**
```
204 No Content
(no body)
```

---

## Status Codes in REST

| Code | When to Use |
|---|---|
| 200 OK | Successful GET, PUT, PATCH |
| 201 Created | Successful POST (resource created) |
| 204 No Content | Successful DELETE |
| 400 Bad Request | Invalid input / malformed request |
| 401 Unauthorized | Missing or invalid authentication |
| 403 Forbidden | Authenticated but not allowed |
| 404 Not Found | Resource doesn't exist |
| 409 Conflict | Duplicate resource / version conflict |
| 500 Internal Server Error | Something broke on the server |

---

## REST vs Other API Styles

| | REST | GraphQL | gRPC |
|---|---|---|---|
| Transport | HTTP | HTTP | HTTP/2 |
| Data format | JSON (usually) | JSON | Protocol Buffers (binary) |
| Fetching | Multiple endpoints | Single endpoint, query what you need | Single endpoint, predefined schema |
| Over-fetching | Common (returns more than needed) | Solved (client specifies fields) | Not applicable |
| Learning curve | Low | Medium | High |
| Popularity | Most common | Growing | Microservices |

REST is the default for public APIs and most web applications. It's simple, widely understood, and works with any HTTP client.

---

## What "RESTful" Really Means

Many APIs claim to be RESTful but aren't fully. A truly RESTful API:
- Uses nouns for URLs (`/users` not `/getUsers`)
- Uses HTTP methods correctly (not `POST /deleteUser`)
- Returns appropriate status codes
- Is stateless
- Includes hyperlinks to related resources (HATEOAS — a strict REST principle most APIs skip)

In practice, "REST API" usually means "HTTP API that returns JSON and uses standard methods." That's fine — REST is a set of guidelines, not a strict spec.
