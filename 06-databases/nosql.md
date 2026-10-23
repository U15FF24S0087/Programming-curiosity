# NoSQL (Non-Relational Databases)

## What Is NoSQL?

NoSQL ("Not Only SQL") refers to databases that don't use the traditional table-based relational model. Instead of rows and columns with a fixed schema, NoSQL databases use flexible structures that can handle varied, unstructured, or rapidly changing data.

---

## Types of NoSQL Databases

### 1. Document Stores
Store data as flexible, self-describing documents (usually JSON-like).

```
users collection:
{
  "_id": 1,
  "name": "Alice",
  "email": "alice@example.com",
  "interests": ["hiking", "coding"],
  "address": { "city": "Seattle", "zip": "98101" }
}
```

Each document can have a different structure. No predefined schema needed.

**Examples**: MongoDB, CouchDB, Amazon DocumentDB

### 2. Key-Value Stores
The simplest model — just a map of keys to values. Extremely fast for lookups.

```
key          → value
"user:1"     → {"name": "Alice", "age": 30}
"user:2"     → {"name": "Bob", "age": 25}
"session:x"  → "active"
```

**Examples**: Redis, DynamoDB, Memcached

### 3. Column-Family Stores
Store data in columns instead of rows. Optimized for queries over large datasets.

**Examples**: Apache Cassandra, HBase

### 4. Graph Databases
Store entities (nodes) and their relationships (edges). Optimized for connected data — social networks, recommendation engines, fraud detection.

```
(Alice) ── FRIENDS_WITH ──► (Bob)
(Alice) ── WORKS_AT ──────► (Acme Corp)
```

**Examples**: Neo4j, Amazon Neptune

---

## NoSQL vs SQL

| | SQL (Relational) | NoSQL |
|---|---|---|
| Structure | Tables, rows, columns | Documents, key-value, graphs |
| Schema | Fixed, predefined | Flexible (schema-on-read) |
| Scaling | Vertical (bigger server) | Horizontal (more servers) |
| Joins | Built-in, efficient | Often limited or nonexistent |
| Consistency | Strong (ACID) | Often eventual (BASE) |
| Best for | Structured data, transactions | Unstructured data, scale |

---

## Why NoSQL Exists

Relational databases were built for structured data with consistent schemas. But the modern internet created new challenges:

- **Massive scale** — millions of users, billions of rows. SQL databases are hard to spread across multiple servers. Many NoSQL databases are designed for **horizontal scaling** from the start.
- **Flexible data** — a social media post, a user profile, and a product listing all have different fields. Forcing them into a fixed table is awkward.
- **Speed over consistency** — for some use cases (caching, real-time analytics), it's acceptable for data to be *eventually* consistent rather than perfectly consistent at every moment.
- **Developer velocity** — without a schema, you can add new fields without migrations.

---

## Schema-On-Read vs Schema-On-Write

**SQL (schema-on-write):** The database validates data structure *before* storing it. Reject anything that doesn't fit the schema.

**NoSQL (schema-on-read):** The database stores anything. The application interprets the structure when reading. One document might have `age`, another might not.

This means:
- NoSQL is more flexible, but the application must handle missing/inconsistent fields
- SQL is stricter, but the database enforces data quality for you

---

## Eventual Consistency

In distributed NoSQL systems, when you write data, it might not immediately be visible to all nodes. The system becomes **eventually consistent** — if no new updates happen, all nodes will eventually converge on the same value.

```
Write to Node A ──► Node A has latest data
                     Node B has old data (will catch up)
                     Node C has old data (will catch up)

Eventually: all nodes agree
```

This is a deliberate tradeoff: faster writes and higher availability at the cost of immediate consistency. For a banking app, this is dangerous. For a social media feed, it's fine.

---

## Common Use Cases

| Database | Use Case |
|---|---|
| MongoDB | Content management, catalogs, user profiles |
| Redis | Caching, session storage, real-time leaderboards |
| Cassandra | Time-series data, IoT, write-heavy workloads |
| Neo4j | Social networks, recommendation engines, fraud detection |
| DynamoDB | Serverless apps, high-throughput key-value lookups |

See `sql.md` for the relational side, and `transactions.md` for consistency models.
