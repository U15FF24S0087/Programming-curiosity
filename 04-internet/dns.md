# DNS (Domain Name System)

## What Is DNS?

DNS is the phonebook of the internet. Computers identify each other using IP addresses (like `142.250.190.46`), but humans remember names (like `google.com`). DNS translates domain names into IP addresses.

```
You type: google.com
    ↓
DNS lookup
    ↓
DNS returns: 142.250.190.46
    ↓
Your browser connects to that IP
```

---

## Why DNS Exists

Without DNS, you'd have to memorize IP addresses for every website. That worked in the early internet (when there were only a few computers), but doesn't scale to billions of devices.

DNS is a distributed, hierarchical system that maps names to addresses across the entire planet.

---

## How DNS Works: Step by Step

When you type `example.com` into your browser:

1. **Browser cache check** — your browser may have recently resolved this domain and remembers the IP
2. **OS cache check** — your operating system also keeps a short-term DNS cache
3. **Recursive resolver query** — if not cached, your request goes to your configured DNS resolver (often your ISP's, or a public one like Google's `8.8.8.8`)
4. **Root nameserver** — the resolver asks the root server (".") where to find `.com` domains
5. **TLD nameserver** — the root points to the `.com` nameserver, which knows about `example.com`
6. **Authoritative nameserver** — the `.com` server points to the specific server responsible for `example.com`, which returns the final IP address
7. **Cache and return** — the IP is returned to your browser, and cached at every level for future use

```
Browser → Resolver → Root → TLD (.com) → Authoritative (example.com)
    ↑                                              │
    └────────────── IP address ────────────────────┘
```

---

## DNS Records

DNS doesn't just map names to IPs. It stores different types of records:

| Record Type | Purpose | Example |
|---|---|---|
| **A** | Maps a domain to an IPv4 address | `example.com → 93.184.216.34` |
| **AAAA** | Maps a domain to an IPv6 address | `example.com → 2606:2800:220:1:...` |
| **CNAME** | Alias from one domain to another | `blog.example.com → example.com` |
| **MX** | Mail exchange — where to send email | `example.com → mail.example.com` |
| **TXT** | Arbitrary text (verification, SPF, etc.) | `"v=spf1 include:_spf.google.com"` |
| **NS** | Nameservers for the domain | `example.com → ns1.example.com` |

---

## Hierarchical Structure

DNS is organized as a tree:

```
                    Root (.)
                   /    \
               com      org
              / \        |
        google  example  wikipedia
         |        |        |
       www     www blog  en es
```

Each level delegates responsibility to the level below. The root servers (13 logical instances, replicated worldwide) know about top-level domains (.com, .org, .net, country codes like .uk, .jp).

---

## Caching and TTL

Every DNS record has a **TTL (Time to Live)** — how long the result can be cached before it needs to be re-fetched. Typical TTLs range from minutes to days.

This is why DNS changes take time to propagate — caches around the world hold the old value until the TTL expires.

---

## Common DNS Tools

```bash
# Look up the IP for a domain
nslookup example.com

# Detailed DNS query
dig example.com

# See the full resolution path
dig +trace example.com

# Flush your local DNS cache (macOS)
sudo dscacheutil -flushcache
```

---

## DNS Over HTTPS (DoH)

Traditionally, DNS queries are sent in plain text, meaning anyone watching your network can see which domains you're looking up. **DNS over HTTPS** encrypts these queries, improving privacy.

See `https.md` for more about encryption on the web.
