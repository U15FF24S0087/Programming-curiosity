# HTTPS (HTTP Secure)

## What Is HTTPS?

HTTPS is HTTP wrapped in an encrypted connection. Same protocol, same request-response cycle — but the data in transit is encrypted so no one between the browser and server can read or tamper with it.

```
HTTP:    Browser ──── plain text ────► Server
                            ↑
                   anyone can read this

HTTPS:   Browser ──── encrypted ────► Server
                             ↑
                   only the server can decrypt
```

The "S" stands for Secure. It's what makes online banking, shopping, and login pages safe to use.

---

## Why HTTPS Exists

Without encryption:
- Your ISP can see every website you visit
- Someone on the same Wi-Fi (coffee shop, airport) can steal your passwords
- Attackers can modify responses — inject ads, malware, or fake content

HTTPS provides three guarantees:
1. **Confidentiality** — data is encrypted, no one can read it in transit
2. **Integrity** — data can't be modified without detection
3. **Authentication** — you're actually connected to the real server, not an impostor

---

## TLS (Transport Layer Security)

HTTPS is HTTP over **TLS** (formerly SSL). TLS is the protocol that handles the encryption. Here's what happens when you visit an HTTPS site:

### 1. TCP Connection
Browser establishes a standard TCP connection to the server on port 443.

### 2. TLS Handshake
```
Client                                    Server
  │  ──── ClientHello ──────────────►       │
  │  ◄──── ServerHello + Certificate ──     │
  │  ──── Key Exchange ─────────────►       │
  │  ──── Finished ─────────────────►       │
  │  ◄──── Finished ─────────────────       │
  │                                        │
  │  ════ encrypted channel established ════ │
```

During the handshake:
- Client and server agree on encryption algorithms
- Server presents its **certificate** (proof of identity)
- Both sides exchange keys to derive a shared **session key**
- From this point, all HTTP traffic is encrypted with the session key

### 3. Encrypted HTTP
Once TLS is established, normal HTTP requests and responses flow — but encrypted.

---

## Certificates and CAs

How does your browser know the server is who it claims to be?

- The server presents a **digital certificate** — a file containing the server's public key and identity, signed by a trusted **Certificate Authority (CA)**
- Your browser comes pre-loaded with a list of trusted CAs (Let's Encrypt, DigiCert, GlobalSign, etc.)
- If the certificate is signed by a trusted CA, your browser trusts the server

This is the **authentication** part of HTTPS — without certificates, you could be connected to an attacker pretending to be your bank.

---

## Symmetric vs Asymmetric Encryption

TLS uses both:

| | Symmetric | Asymmetric |
|---|---|---|
| Keys | Same key for encrypt and decrypt | Public key encrypts, private key decrypts |
| Speed | Very fast | Slow |
| Used for | The actual data transfer | Key exchange and certificates |

The **asymmetric** part happens during the handshake (securely exchange a shared key). The **symmetric** part handles the bulk data (fast encryption of actual web traffic).

See `../07-security/encryption.md` for a deeper dive.

---

## HTTPS in Practice

Today, HTTPS is the default, not the exception:
- Browsers flag HTTP sites as "Not Secure"
- **HSTS** (HTTP Strict Transport Security) tells browsers to always use HTTPS
- Let's Encrypt provides free certificates, removing the cost barrier
- Modern web features (geolocation, camera, service workers) require HTTPS

The padlock icon in your address bar means the connection is encrypted and the certificate is valid. It does **not** mean the site itself is trustworthy — a scam site can have HTTPS too.
