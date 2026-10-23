# Indexes

## What Is an Index?

An index is a data structure that makes finding specific rows much faster — at the cost of extra storage and slower writes.

Think of a book index. Without one, to find every mention of "TCP" you'd have to read every page. With an index, you look up "TCP" in the back, see pages 42, 87, 203, and jump straight there.

A database index works the same way — it creates a lookup structure so the database can find rows without scanning the entire table.

---

## Without an Index (Full Table Scan)

```sql
SELECT * FROM users WHERE email = 'alice@example.com';
```

Without an index on `email`, the database checks **every single row** in the table:

```
Row 1: alice@example.com     ✓ match!
Row 2: bob@example.com       ✗
Row 3: charlie@example.com   ✗
...
Row 1,000,000: zoe@example.com  ✗
```

For a million-row table, this means reading a million rows just to find one. Slow.

---

## With an Index

Add an index on the `email` column:

```sql
CREATE INDEX idx_users_email ON users(email);
```

The database creates a sorted data structure (usually a B-tree) for email values:

```
Index (sorted by email):
alice@example.com  → row 1
bob@example.com    → row 2
charlie@example.com → row 3
...
```

Now the same query does a **binary search** — jumping to the right place in the sorted index:

```
Start at middle... "charlie@example.com"
"alice" comes before "charlie" → go left
Found: alice@example.com → row 1  ✓
```

For 1,000,000 rows, this takes ~20 comparisons instead of 1,000,000 reads. That's the difference between milliseconds and seconds.

---

## Types of Indexes

### B-Tree (Default)
Balanced tree structure. Good for equality (`=`) and range queries (`>`, `<`, `BETWEEN`).

```
           [M]
          /    \
      [F]      [T]
     / \       / \
   [B][J]   [P][W]
```

### Hash Index
Exact match lookups only. Very fast for `=`, but can't do ranges.

### Composite (Multi-Column) Index
Index on multiple columns at once:

```sql
CREATE INDEX idx_name_city ON users(last_name, city);
```

Useful when you frequently query by both columns together:
```sql
SELECT * FROM users WHERE last_name = 'Smith' AND city = 'Seattle';
```

Order matters — the index works left to right. It helps with `last_name` alone, but not `city` alone.

### Unique Index
Enforces that no two rows have the same value:

```sql
CREATE UNIQUE INDEX idx_unique_email ON users(email);
```

This is how primary keys and unique constraints work under the hood.

---

## The Tradeoff: Reads vs Writes

Indexes aren't free:

| | Without Index | With Index |
|---|---|---|
| Read speed | Slow (full scan) | Fast (lookup) |
| Write speed | Fast (just insert) | Slower (must update index) |
| Storage | Just the table data | Table data + index data |

Every `INSERT`, `UPDATE`, or `DELETE` must also update the index. For a table with 5 indexes, each write updates 5 data structures. This is why you don't index every column — only the ones you query by frequently.

---

## When to Index

**Do index:**
- Columns used in `WHERE` clauses frequently
- Columns used in `JOIN` conditions (foreign keys)
- Columns used for `ORDER BY` or `GROUP BY`
- Columns with high selectivity (many unique values)

**Don't index:**
- Small tables (full scan is fast enough)
- Columns that are rarely queried
- Columns with very few unique values (e.g., boolean — the index doesn't help much)
- Tables with very high write rates and low read rates

---

## The Analogy

| Real World | Database |
|---|---|
| Book index in the back | Database index |
| Looking up a topic → page numbers | Index lookup → row locations |
| More indices = thicker book | More indexes = more storage |
| Writing a new edition requires updating all indices | New writes require updating all indexes |

---

## Checking Your Indexes

Most databases let you see how a query uses indexes:

```sql
-- PostgreSQL
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'alice@example.com';

-- Look for "Seq Scan" (bad — full table scan)
-- vs "Index Scan" or "Index Only Scan" (good)
```

`EXPLAIN` shows the query plan — which indexes it uses, whether it scans, and how many rows it expects to process. It's the single most important tool for query performance tuning.
