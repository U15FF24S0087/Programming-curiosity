# Server

## What Is a Server?

A server is a computer that provides services to other computers (clients). When you visit a website, your browser (the client) asks a server for the page, and the server responds with the HTML.

```
Client (your browser)          Server (remote computer)
    │  ──── "Give me the page" ────►    │
    │  ◄─── "Here's the HTML" ─────────  │
```

A server is just a computer. It's not fundamentally different from your laptop — it's just optimized for running 24/7, handling many requests, and being reliable.

---

## What Makes a Server Different from Your Laptop

| | Your Laptop | A Server |
|---|---|---|
| Uptime | On when you use it | Always on (24/7) |
| Purpose | Interactive use | Serve requests from clients |
| Hardware | General purpose | More CPU, RAM, faster network |
| Location | Your desk | Data center (cool, redundant power) |
| Connection | Home internet | High-bandwidth, fixed IP |
| Redundancy | None | Backup power, RAID storage, failover |

---

## Types of Servers

| Type | What It Serves | Example |
|---|---|---|
| **Web server** | HTML, CSS, JS files and responses | Nginx, Apache |
| **Application server** | Runs business logic, talks to databases | Node.js app, Django, Spring Boot |
| **Database server** | Stores and queries data | PostgreSQL, MongoDB |
| **File server** | Stores files for network access | NFS, SFTP |
| **Mail server** | Handles email | Postfix, Exchange |
| **DNS server** | Resolves domain names | BIND, Unbound |
| **Proxy server** | Forwards requests, caches, filters | HAProxy, Squid |

In modern setups, these are often **virtualized** — one physical server runs many virtual servers (VMs) or containers.

---

## How a Web Server Works

1. **Listen**: The server listens on a port (e.g., 80 for HTTP, 443 for HTTPS)
2. **Accept**: A client connects to that port
3. **Process**: The server reads the request, does any processing, fetches data if needed
4. **Respond**: The server sends back the response
5. **Close (or reuse)**: The connection closes, or stays open for more requests (keep-alive)

```
Request → [Port 443] → [Web server software] → [App logic] → [Database] → Response
```

---

## Static vs Dynamic Content

**Static content**: The server just returns files as-is (HTML, images, CSS). No processing needed. Very fast.

**Dynamic content**: The server runs code to generate a response (queries a database, renders a template, calls an API). Slower but flexible.

Most modern sites combine both — a web server (Nginx) serves static files directly, and forwards dynamic requests to an application server.

```
Nginx (web server)
  ├── /static/* → serve files directly (fast)
  └── /api/*    → forward to Node.js app (dynamic)
```

---

## Ports

Servers listen on specific **ports** — numbered channels on a network interface:

| Port | Protocol | Common Use |
|---|---|---|
| 80 | HTTP | Web traffic (unencrypted) |
| 443 | HTTPS | Web traffic (encrypted) |
| 22 | SSH | Secure remote access |
| 53 | DNS | DNS queries |
| 3306 | MySQL | Database access |
| 5432 | PostgreSQL | Database access |
| 6379 | Redis | Redis cache |
| 8080 | HTTP (alt) | Dev servers, proxies |

Only one process can listen on a given port at a time.

---

## Physical vs Virtual vs Container

### Physical Server (Bare Metal)
A real machine in a data center. Maximum performance, full control, but you pay for the hardware even when idle.

### Virtual Machine (VM)
A software simulation of a computer running on a physical server. Multiple VMs share one physical machine. You get your own OS, isolated from other VMs.

### Container
A lightweight package containing your app and its dependencies. Containers share the host OS kernel but are otherwise isolated. Start in seconds. Much lighter than VMs.

```
Physical Server
├── VM 1 (full OS + app)
├── VM 2 (full OS + app)
└── ...

Physical Server (container host)
├── Container 1 (app + deps, shares kernel)
├── Container 2 (app + deps, shares kernel)
└── ...
```

See `cloud.md` for how these are rented as services, and `scaling.md` for handling more traffic.
