# Transactions

## What Is a Transaction?

A transaction is a group of database operations that must all succeed together, or all fail together. It's an all-or-nothing unit of work.

Classic example: transferring money between accounts.

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;  -- deduct from Alice
UPDATE accounts SET balance = balance + 100 WHERE id = 2;  -- add to Bob
COMMIT;
```

If the first statement succeeds but the second fails (server crash, constraint violation), the database **rolls back** both changes. Money doesn't disappear.

---

## ACID: The Four Guarantees

Transactions in relational databases follow the **ACID** principles:

### A — Atomicity
All operations in a transaction happen, or none of them do. No partial updates.

```
Transfer $100 from Alice to Bob:
  ✓ Deduct $100 from Alice
  ✗ Add $100 to Bob (error!)
  → Rollback: both changes undone
  → Alice's balance is restored
```

### C — Consistency
A transaction brings the database from one valid state to another. All constraints (foreign keys, uniqueness, data types) must hold before and after.

You can't violate a constraint even mid-transaction. If a transaction would leave invalid data, it fails.

### I — Isolation
Concurrent transactions shouldn't interfere with each other. If Alice and Bob are both transferring money at the same time, their transactions should behave as if they ran one at a time.

In practice, perfect isolation is expensive, so databases offer **isolation levels**:

| Level | Description |
|---|---|
| Read Uncommitted | Can see uncommitted changes from other transactions (dirty reads) |
| Read Committed | Only sees committed data (PostgreSQL default) |
| Repeatable Read | Same query returns same results within a transaction |
| Serializable | Full isolation — transactions behave as if sequential |

Higher isolation = more correctness but less concurrency.

### D — Durability
Once a transaction is committed, it survives permanently — even if the power goes out. The database writes to disk (or a write-ahead log) before confirming the commit.

---

## The Dirty Read Problem

Without isolation, transactions can see each other's uncommitted data:

```
Transaction A: BEGIN
Transaction A: UPDATE balance SET balance = 50 WHERE id = 1  (not committed)
Transaction B: SELECT balance FROM accounts WHERE id = 1     → reads 50
Transaction A: ROLLBACK  (undo — balance goes back to 100)
Transaction B: used 50, which never existed
```

Transaction B read **dirty data** — a value that was later rolled back. The "Read Committed" isolation level prevents this.

---

## Race Conditions in Databases

### The Lost Update

```
Transaction A: READ balance = 100 for user 1
Transaction B: READ balance = 100 for user 1
Transaction A: WRITE balance = 100 - 50 = 50
Transaction B: WRITE balance = 100 - 30 = 70  ← overwrites A's update!
```

$50 disappeared. Solutions:
- **Pessimistic locking**: lock the row when reading, release after commit
- **Optimistic locking**: check a version number hasn't changed before writing
- **Serializable isolation**: database prevents the conflict

---

## BEGIN, COMMIT, ROLLBACK

```sql
BEGIN;          -- start transaction

INSERT INTO orders (user_id, total) VALUES (1, 49.99);
UPDATE inventory SET stock = stock - 1 WHERE product_id = 5;

-- Everything OK?
COMMIT;         -- save all changes

-- Or if something went wrong:
ROLLBACK;       -- undo all changes
```

Everything between `BEGIN` and `COMMIT`/`ROLLBACK` is one atomic unit.

---

## Transactions in Code

### Python (with a database connection)
```python
import psycopg2

conn = psycopg2.connect("dbname=mydb")
cursor = conn.cursor()

try:
    cursor.execute("UPDATE accounts SET balance = balance - 100 WHERE id = 1")
    cursor.execute("UPDATE accounts SET balance = balance + 100 WHERE id = 2")
    conn.commit()           # both succeed
except Exception as e:
    conn.rollback()          # both undone
    raise e
```

### Why This Matters
Without transactions, a crash between the two `UPDATE` statements would leave money in an inconsistent state — deducted from Alice but never added to Bob.

---

## ACID vs BASE

Relational databases choose **ACID** (strong consistency). Many NoSQL databases choose **BASE**:

| ACID | BASE |
|---|---|
| Atomic | Basically Available |
| Consistent | Soft state |
| Isolated | Eventually consistent |
| Durable | |

BASE trades immediate consistency for availability and scale. The right choice depends on your use case:
- **Banking, orders, inventory** → ACID (correctness is non-negotiable)
- **Social media feeds, search indexes, product recommendations** → BASE (eventual is fine)

See `nosql.md` for more on eventual consistency.
