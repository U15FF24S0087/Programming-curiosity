# Encryption

## What Is Encryption?

Encryption is the process of converting readable data (**plaintext**) into an unreadable form (**ciphertext**) using a mathematical algorithm and a **key**. Only someone with the correct key can decrypt it back to the original.

```
Plaintext: "Hello, World!"
      ↓ [encrypt with key]
Ciphertext: 7d2e9f1a3b8c...
      ↓ [decrypt with key]
Plaintext: "Hello, World!"
```

Unlike hashing (one-way), encryption is **reversible** — if you have the key, you can get the original back.

---

## Two Types of Encryption

### Symmetric Encryption

The **same key** encrypts and decrypts.

```
Alice                          Bob
  │                              │
  │  key = "secret123"           │  key = "secret123"
  │                              │
  │  encrypt("Hello", key)       │
  │  → ciphertext                │
  │  ──── send ciphertext ──►   │
  │                              │  decrypt(ciphertext, key)
  │                              │  → "Hello"
```

**Examples**: AES, ChaCha20, DES (outdated)

**Advantage**: very fast.  
**Disadvantage**: how do you share the key securely? If you send the key over the internet, anyone intercepting it can decrypt everything.

### Asymmetric Encryption (Public-Key)

Uses a **pair of keys**: a public key and a private key.

- **Public key**: can be shared with anyone. Used to encrypt.
- **Private key**: kept secret. Used to decrypt.

```
Bob's public key: (shared with everyone)
Bob's private key: (only Bob knows)

Alice wants to send a secret to Bob:
  Alice encrypts with Bob's public key  →  ciphertext
  Only Bob's private key can decrypt it  →  plaintext
```

**Examples**: RSA, ECC (Elliptic Curve Cryptography)

**Advantage**: no need to share a secret key.  
**Disadvantage**: much slower than symmetric.

---

## How They're Used Together (Hybrid Encryption)

In practice (like HTTPS/TLS), both types are combined:

1. **Asymmetric** encryption to securely exchange a symmetric key
2. **Symmetric** encryption for the actual data (because it's much faster)

```
1. Browser generates a random symmetric key
2. Encrypts it with the server's public key (asymmetric)
3. Sends it to the server
4. Server decrypts with its private key → now both have the symmetric key
5. All further communication uses the fast symmetric key
```

This is how HTTPS works. See `../04-internet/https.md` for the full handshake.

---

## Common Algorithms

| Algorithm | Type | Use Case |
|---|---|---|
| AES-256 | Symmetric | General data encryption (files, disks, traffic) |
| ChaCha20 | Symmetric | Mobile / high-speed encryption |
| RSA | Asymmetric | Key exchange, digital signatures |
| ECC | Asymmetric | Key exchange (smaller keys, same security) |
| Diffie-Hellman | Asymmetric | Key exchange protocol |

AES-256 is the current standard for symmetric encryption. RSA is the classic for asymmetric, but ECC is replacing it (smaller keys, faster operations).

---

## Digital Signatures

Asymmetric encryption also works in reverse for **signing**:

```
Bob signs a message:
  1. Hash the message: hash("Transfer $100") = abc123
  2. Encrypt the hash with Bob's PRIVATE key: signature
  3. Send message + signature

Alice verifies:
  1. Decrypt signature with Bob's PUBLIC key → get hash
  2. Hash the message herself → get hash
  3. If both hashes match → message is authentic and untampered
```

This proves two things:
- **Authenticity**: only Bob has the private key, so only Bob could have signed it
- **Integrity**: if the message was modified, the hashes wouldn't match

Digital signatures are used in:
- HTTPS certificates (proving server identity)
- Software updates (proving the update is from the real publisher)
- Git commits (signed commits verify authorship)
- Cryptocurrency transactions

---

## Encryption at Rest vs in Transit

| | In Transit | At Rest |
|---|---|---|
| What it protects | Data moving over the network | Data stored on disk |
| Technology | TLS / HTTPS | Disk encryption, database encryption |
| Threat it stops | Eavesdroppers on the network | Physical theft of disk / database breach |
| Example | HTTPS connection to your bank | FileVault on macOS, BitLocker on Windows |

Both are needed. Encrypting in transit doesn't help if the database stores data in plain text and gets stolen.

---

## Key Management: The Hard Part

Encryption is only as secure as its keys. If the key is stolen, the encryption is useless.

Key management challenges:
- **Where do you store keys?** Not next to the encrypted data. Use a key management service (AWS KMS, HashiCorp Vault).
- **How do you rotate keys?** Change them periodically to limit exposure.
- **How do you share keys?** Distribute them without exposing them.
- **What if a key is lost?** Without the key, encrypted data is gone forever. There's no "forgot my password" for raw encryption.

---

## Hashing vs Encryption vs Encoding

These three are often confused:

| | Purpose | Reversible? |
|---|---|---|
| **Hashing** | Verify integrity, store passwords | No |
| **Encryption** | Protect confidentiality | Yes (with key) |
| **Encoding** | Format data for transport | Yes (no key needed) |

Encoding (Base64, URL encoding) is NOT security. It's just formatting. `Base64("password")` can be decoded instantly by anyone — it provides zero protection.

See `hashing.md` for the one-way side of the coin.
