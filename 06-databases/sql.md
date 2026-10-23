# SQL (Relational Databases)

## What Is SQL?

SQL (Structured Query Language) is the language used to interact with **relational databases** — databases that store data in tables with rows and columns, much like a spreadsheet.

```
users table:
┌────┬───────────┬──────────────────────┬─────────┐
│ id │ name      │ email                │ age     │
├────┼───────────┼──────────────────────┼─────────┤
│  1 │ Alice     │ alice@example.com    │ 30      │
│  2 │ Bob       │ bob@example.com      │ 25      │
│  3 │ Charlie   │ charlie@example.com  │ 35      │
└────┴───────────┴──────────────────────┴─────────┘
```

SQL lets you create, read, update, and delete data in these tables.

---

## Core SQL Operations (CRUD)

### Create
```sql
INSERT INTO users (name, email, age)
VALUES ('Dave', 'dave@example.com', 28);
```

### Read
```sql
SELECT name, email FROM users WHERE age > 25;
```

### Update
```sql
UPDATE users SET age = 31 WHERE name = 'Alice';
```

### Delete
```sql
DELETE FROM users WHERE id = 3;
```

---

## Tables and Relationships

Relational databases get their name from how tables **relate** to each other.

```
users                orders
┌────┬────────┐     ┌─────┬─────────┬──────────┐
│ id │ name   │     │ id  │ user_id  │ total    │
├────┼────────┤     ├─────┼─────────┼──────────┤
│  1 │ Alice  │◄────│ 101 │    1    │  $49.99  │
│  2 │ Bob    │◄────│ 102 │    1    │  $12.50  │
└────┴────────┘     │ 103 │    2    │  $99.00  │
                    └─────┴─────────┴──────────┘
```

The `user_id` column in `orders` is a **foreign key** that references `id` in `users`. This connects each order to its user.

### Joins
To combine data from multiple tables:
```sql
SELECT users.name, orders.total
FROM users
JOIN orders ON users.id = orders.user_id;
```

Result:
```
name   | total
-------|-------
Alice  | 49.99
Alice  | 12.50
Bob    | 99.00
```

---

## Schemas

A relational database has a **schema** — a predefined structure that specifies tables, columns, data types, and relationships. Before you can store data, you must define the schema:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    age INTEGER,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

This is a **schema-on-write** model — the database enforces structure before data goes in. If you try to insert a row with a missing `email`, the database rejects it.

---

## Common SQL Databases

| Database | Notes |
|---|---|
| PostgreSQL | Open source, feature-rich, great for complex queries |
| MySQL | Open source, widely used (WordPress, etc.) |
| SQLite | Embedded (no server), lives in a single file — great for apps |
| SQL Server | Microsoft's enterprise database |
| Oracle | Enterprise, expensive, used by large corporations |

Despite differences in features and performance, they all speak SQL (with slight dialects).

---

## When to Use SQL

Relational databases shine when:
- Data has a clear, consistent structure
- **ACID** guarantees matter (banking, transactions, inventory) — see `transactions.md`
- You need complex queries and joins across tables
- Data integrity is critical

They're less ideal when:
- Data structure is flexible or rapidly changing
- You need horizontal scaling across many servers
- You're storing huge volumes of unstructured data

For those cases, see `nosql.md`.
