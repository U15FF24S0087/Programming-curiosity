# Scaling

## What Is Scaling?

Scaling is how a system handles more load — more users, more requests, more data. Every system that succeeds eventually needs to scale. The question is how.

```
100 users → works fine
10,000 users → slow
100,000 users → crashes

What do you do?
```

---

## Vertical vs Horizontal Scaling

### Vertical Scaling (Scale Up)
Make your existing server more powerful — add more CPU, RAM, or storage.

```
Small server (2 CPU, 4GB RAM) → Bigger server (16 CPU, 64GB RAM)
```

**Advantages**: simple — no code changes needed. Just move to a bigger machine.  
**Disadvantages**: there's a limit. Eventually you can't buy a bigger server. It's also a single point of failure.

### Horizontal Scaling (Scale Out)
Add more servers and distribute the load across them.

```
1 server → 2 servers → 10 servers → 100 servers
```

**Advantages**: no theoretical limit (keep adding servers), no single point of failure.  
**Disadvantages**: more complex — you need load balancing, shared state management, and your app must be designed to work across multiple instances.

| | Vertical | Horizontal |
|---|---|---|
| Complexity | Low | High |
| Limit | Hardware max | Theoretical unlimited |
| Downtime | Usually needed to upgrade | None (add servers anytime) |
| Redundancy | Single machine | Multiple machines |
| Cost | Exponential (top-tier hardware is expensive) | Linear (add commodity servers) |

---

## Load Balancing

When you have multiple servers, something needs to distribute requests across them. That's the **load balancer**.

```
                    ┌── Server 1 (25% of traffic)
Client → [Load Balancer] ──┼── Server 2 (25% of traffic)
                    ├── Server 3 (25% of traffic)
                    └── Server 4 (25% of traffic)
```

The load balancer receives each request and forwards it to one of the servers. Common strategies:

| Strategy | How It Works |
|---|---|
| Round robin | Send to the next server in rotation |
| Least connections | Send to the server with fewest active requests |
| IP hash | Same client always goes to the same server |
| Weighted | Send more traffic to more powerful servers |

The load balancer also monitors server health — if a server stops responding, it stops sending traffic there.

---

## State and Scaling

The biggest challenge in horizontal scaling is **state**. If a server stores data locally (sessions, cached data), then requests from the same user need to go to the same server.

### Sticky Sessions (Session Affinity)
The load balancer ensures the same user always hits the same server. Simple, but if that server dies, the user's session is lost.

### Stateless Architecture
Servers don't store any local state. All state lives in a shared store (database, Redis, S3). Any server can handle any request.

```
Stateless:
  Request → any server → reads/writes to shared DB → responds
  If server dies, next request goes to another server (no data lost)
```

This is the gold standard for scalable web applications. It's why modern frameworks encourage stateless design.

---

## Caching

Caching is the most effective scaling technique: store results so you don't have to recompute them.

### Levels of Caching

| Level | What's Cached | Example |
|---|---|---|
| Browser | Static assets (JS, CSS, images) | Cache-Control headers |
| CDN | Content near the user geographically | Cloudflare, CloudFront |
| Application | Computed results, database queries | Redis, Memcached |
| Database | Query results | Materialized views, query cache |

```
Without cache: every request → compute → database query → respond
With cache:    first request → compute → store result
               subsequent requests → read from cache → respond (100x faster)
```

Caching trades memory for speed. The challenge is **invalidation** — knowing when cached data is stale and needs to be refreshed.

---

## Database Scaling

### Read Replicas
Most apps read far more than they write. Create copies of the database dedicated to reads:

```
                  ┌── Read Replica 1
Master (writes) ──┼── Read Replica 2
                  └── Read Replica 3
```

All writes go to the master. Reads can go to any replica. This scales read capacity.

### Sharding (Partitioning)
Split data across multiple databases. Each shard holds a portion of the data:

```
Shard 1: users with id 1–10,000
Shard 2: users with id 10,001–20,000
Shard 3: users with id 20,001–30,000
```

This scales both reads and writes, but adds complexity (cross-shard queries, rebalancing).

---

## Asynchronous Processing

For slow operations (sending emails, processing images, generating reports), don't block the user's request. Put the work in a **queue** and process it in the background:

```
User request → "generate report" → put job in queue → respond "we'll email it to you"
                     ↓
              Background worker picks up job → generates report → emails it
```

This keeps web servers responsive even under heavy load. Tools: RabbitMQ, AWS SQS, Celery, Kafka.

---

## Autoscaling

In the cloud, you can configure servers to be added or removed automatically based on metrics:

```
CPU usage > 70% for 5 minutes → add 2 more servers
CPU usage < 30% for 10 minutes → remove 2 servers
```

This handles traffic spikes without human intervention. The challenge is getting the thresholds right — too sensitive and you thrash, too slow and users experience lag.

See `cloud.md` for the infrastructure that enables this, and `server.md` for what's actually running.
