# IP (Internet Protocol)

## What Is an IP Address?

An IP address is a unique number assigned to every device on a network. It's how devices find and communicate with each other — like a postal address for your computer.

```
192.168.1.100  ←  your laptop
192.168.1.1    ←  your router
8.8.8.8        ←  Google's DNS server
```

Every packet of data sent across the internet includes a source IP and a destination IP. Routers use these addresses to forward data toward its destination.

---

## IPv4 vs IPv6

### IPv4

The original IP format: four numbers (0–255) separated by dots.

```
192.168.1.100
```

- 32-bit address → ~4.3 billion possible addresses
- We ran out of new IPv4 addresses (largely) around 2011
- Still the most commonly used format on local networks

### IPv6

The successor: eight groups of hexadecimal digits separated by colons.

```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

- 128-bit address → 340 undecillion possible addresses (enough for the foreseeable future)
- Slowly being adopted, but IPv4 still dominates

---

## Public vs Private IPs

Not every device needs a globally unique address. Most home networks use **private IP ranges** that are reused behind routers:

| Range | Scope |
|---|---|
| `10.0.0.0 – 10.255.255.255` | Private (large networks) |
| `172.16.0.0 – 172.31.255.255` | Private (medium networks) |
| `192.168.0.0 – 192.168.255.255` | Private (home networks) |

Your router has one **public IP** (visible to the internet) and assigns **private IPs** to your devices. This is called **NAT (Network Address Translation)** — the router translates between your private network and the public internet.

---

## How IP Routing Works

When you send data to another IP address:

1. Your device sends the packet to your **router** (your default gateway)
2. Your router forwards it to your **ISP's router**
3. Each router along the way checks the destination IP and forwards the packet to the next hop
4. After several hops, the packet reaches its destination

```
Your laptop → Home router → ISP router → ... → Destination server
```

Routers use routing tables to decide where to forward packets. There's no single "map" — each router just knows its neighbors and makes local decisions.

---

## TCP and UDP

IP handles addressing and delivery, but it doesn't guarantee packets arrive. Two protocols sit on top of IP to provide different delivery guarantees:

| | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented (handshake first) | Connectionless (just send) |
| Reliability | Guaranteed delivery, retransmits lost packets | Best effort, no retransmission |
| Order | Packets arrive in order | Packets may arrive out of order |
| Use cases | Web, email, file transfer | Video streaming, gaming, DNS |

**TCP** (Transmission Control Protocol) ensures every packet arrives, in order. Most web traffic uses TCP.

**UDP** (User Datagram Protocol) fires packets without guarantees — faster but lossy. Used where speed matters more than completeness (live video, gaming).

Together, IP + TCP/UDP form the foundation of internet communication. See `http.md` for what gets built on top.
