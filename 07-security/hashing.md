# Hashing

## What Is Hashing?

Hashing is the process of converting any input (a password, a file, a message) into a fixed-length string of characters using a mathematical function. The output is called a **hash** or **digest**.

```
Input: "hello"
Hash (SHA-256): 2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824

Input: "hello!"  (just added a character)
Hash (SHA-256): 83fee5...
```

Key properties:
- Same input always produces the same hash
- Different inputs produce different hashes (ideally)
- The hash is one-way — you can't reverse it to get the input
- A tiny change in input produces a completely different hash (avalanche effect)

---

## Hashing vs Encryption

These are often confused but are fundamentally different:

| | Hashing | Encryption |
|---|---|---|
| Direction | One-way (cannot reverse) | Two-way (can decrypt) |
| Purpose | Verify integrity / store passwords | Protect confidentiality |
| Example | `hash("password") → abc123` | `encrypt("secret") → xyz, decrypt(xyz) → "secret"` |
| Can recover original? | No | Yes (with the key) |

See `encryption.md` for the other side.

---

## Why Hash Passwords?

If you store passwords in plain text and your database is stolen, every password is exposed immediately. Instead, you store hashes:

```
Database (bad):
  alice | password123
  bob   | hunter2

Database (good):
  alice | $2b$10$N9qo8u...  (hash of password123)
  bob   | $2b$10$X8qr1v...  (hash of hunter2)
```

When a user logs in:
```
User enters "password123"
Server hashes it: hash("password123") = $2b$10$N9qo8u...
Server compares with stored hash: they match → login OK
```

Even if the database is stolen, the attacker only has hashes. They can't reverse them to get the passwords.

---

## But Hashes Can Be Cracked

Attackers can't reverse a hash, but they can try to **brute force** it:

1. Take a list of common passwords (or every possible string)
2. Hash each one
3. Compare the hash to the stolen hash
4. If they match, you've found the password

This is why **fast hashes are bad for passwords**. If a hash function computes millions of hashes per second, an attacker can try millions of guesses per second.

---

## Password Hashing: Slow Is Good

For passwords, you want a hash function that is **deliberately slow** — so brute force takes too long.

| Algorithm | Type | Speed | Use Case |
|---|---|---|---|
| MD5 | General purpose | Very fast | Don't use for passwords |
| SHA-256 | General purpose | Fast | File integrity, not passwords |
| bcrypt | Password-specific | Slow (adjustable) | Passwords |
| Argon2 | Password-specific | Slow (memory-hard) | Passwords (recommended) |
| scrypt | Password-specific | Slow (memory-hard) | Passwords |

```python
import bcrypt

# Hash a password
hashed = bcrypt.hashpw(b"password123", bcrypt.gensalt())
# $2b$12$... (includes the salt and cost factor)

# Verify a password
bcrypt.checkpw(b"password123", hashed)  # True
bcrypt.checkpw(b"wrong", hashed)         # False
```

---

## Salting

If two users have the same password, they'd have the same hash. An attacker could precompute a **rainbow table** (a lookup table of common passwords → hashes) and match instantly.

A **salt** is a random value added to each password before hashing, making identical passwords produce different hashes:

```
Alice: hash("password123" + "random_salt_A") = 7f3a...
Bob:   hash("password123" + "random_salt_B") = 2b9c...
```

Same password, different hashes. Rainbow tables become useless because each salt requires its own table.

Bcrypt and Argon2 handle salting automatically — the salt is embedded in the output hash string.

---

## Other Uses of Hashing

### File Integrity
Download a file, hash it, compare to the published hash. If they match, the file wasn't corrupted or tampered with.

```bash
sha256sum ubuntu-24.04.iso
# Compare with the hash on the official website
```

### Data Structures
- **Hash tables** (hash maps, dictionaries) — use hashing for O(1) lookups
- **Hash sets** — check membership quickly

### Blockchain
Each block contains the hash of the previous block. Tampering with any block invalidates all subsequent hashes.

### Content Addressing
Git stores files by their hash — if the content changes, the hash changes. This is how Git detects changes and deduplicates data.

---

## Properties of a Good Hash Function

1. **Deterministic**: same input → same output, always
2. **Avalanche**: one-bit change → completely different hash
3. **Collision resistant**: two different inputs shouldn't produce the same hash (extremely unlikely for SHA-256)
4. **Fixed output**: regardless of input size, the hash is always the same length
5. **One-way**: infeasible to reverse (for cryptographic hashes)

The most important distinction to remember: **hashing is irreversible, encryption is reversible**.
