# What Is an API?

## What Is an API?

An API (Application Programming Interface) is a set of rules that lets different programs talk to each other. It defines what you can ask for, how to ask, and what you'll get back.

Think of a restaurant:
- The **menu** is the API — it tells you what's available
- You **order** (make a request) through the waiter
- The **kitchen** (the system) prepares your order
- The waiter **brings your food** (the response)

You don't need to know how the kitchen works. You just need to know what's on the menu and how to order.

---

## APIs Are Everywhere

APIs aren't just web services. The concept applies at many levels:

| Type | Example |
|---|---|
| **Web API** | Your app asks a server for weather data |
| **OS API** | Your program asks the OS to open a file |
| **Library API** | Your code calls a function in a package |
| **Hardware API** | A driver talks to a graphics card |

When you write `print("hello")`, you're using the Python standard library's API. When you fetch data from a URL, you're using a web API.

---

## Why APIs Exist

Without APIs, every program would have to implement everything from scratch. APIs let you:

- **Reuse functionality** — don't build your own payment system, use Stripe's API
- **Access data** — get stock prices, weather, maps from companies that have it
- **Integrate systems** — your CRM talks to your email tool talks to your database
- **Separate concerns** — the frontend doesn't need to know how the backend works

---

## What an API Defines

An API typically specifies:

1. **Endpoints** — the URLs you can call (e.g., `GET /api/users`)
2. **Methods** — what actions are available (GET, POST, etc.)
3. **Parameters** — what input the API expects
4. **Response format** — what the API returns (usually JSON)
5. **Authentication** — how to prove you're allowed to use it

---

## A Simple Example

A weather API might work like this:

**Request:**
```
GET https://api.weather.com/v1/forecast?city=Seattle&days=3
Authorization: Bearer your_api_key
```

**Response:**
```json
{
  "city": "Seattle",
  "forecast": [
    {"date": "2026-10-23", "temp": 15, "condition": "cloudy"},
    {"date": "2026-10-24", "temp": 12, "condition": "rain"},
    {"date": "2026-10-25", "temp": 14, "condition": "partly cloudy"}
  ]
}
```

You send a request with parameters, the API returns structured data. Your app just needs to display it — the API handled the hard part (weather models, data collection, etc.).

---

## Types of Web APIs

| Type | Description | Example |
|---|---|---|
| **REST** | Standard HTTP-based API (most common) | Twitter API, GitHub API |
| **GraphQL** | Query language — client specifies exactly what data it wants | GitHub GraphQL API |
| **gRPC** | High-performance binary protocol | Microservice-to-microservice |
| **WebSocket** | Persistent two-way connection | Chat apps, live notifications |

See `rest-api.md` for the most common pattern, and `request-response.md` for the mechanics of making calls.
