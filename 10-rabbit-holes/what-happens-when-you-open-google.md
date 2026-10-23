# What Happens When You Open Google

## The Full Journey

You type `google.com` and press Enter. In the ~200 milliseconds before the page appears, your request travels through the entire internet infrastructure. Let's trace every step.

---

## Step 1: You Type the URL

As you type `google.com`:
1. Your browser's autocomplete suggests completions (sending requests to Google's suggestion API as you type)
2. When you press Enter, the browser checks: is this a URL or a search query?
3. It determines `google.com` is a domain → treat as a URL

---

## Step 2: DNS Lookup

The browser needs to convert `google.com` to an IP address. (See `../04-internet/dns.md`)

1. **Browser DNS cache** — has google.com been resolved recently? Probably yes (it's the most visited site)
2. **OS DNS cache** — if not found, ask the OS
3. **OS resolver cache** — still nothing? Query the configured DNS server
4. **Recursive DNS resolver** (e.g., your ISP's, or 8.8.8.8):
   - Ask root nameserver → points to .com TLD
   - Ask .com TLD → points to Google's authoritative nameserver
   - Ask Google's nameserver → returns `142.250.190.46` (or similar)
5. The IP is cached at every level for future use

```
google.com → 142.250.190.46
(~5-50ms)
```

---

## Step 3: TCP Connection

Now the browser needs to establish a connection to `142.250.190.46` on port 443 (HTTPS).

The **TCP three-way handshake**:
```
Browser → Server:  SYN (can we talk?)
Server → Browser:  SYN-ACK (yes, I'm ready)
Browser → Server:  ACK (great, let's start)
```

This takes **1 round trip** — the time for a packet to travel from your computer to Google's server and back. Depending on your location, this is 10–100ms.

---

## Step 4: TLS Handshake

Since we're using HTTPS, the browser and server must establish encryption. (See `../04-internet/https.md`)

```
Browser → Server:  ClientHello (supported ciphers, TLS version)
Server → Browser:  ServerHello + Certificate (server's public key, signed by CA)
Browser:           Verifies certificate with trusted CAs ✓
Browser → Server:  Key exchange (encrypted with server's public key)
Server → Browser:  Finished
Browser → Server:  Finished
```

Both sides now have a shared **session key** for symmetric encryption. This takes 1-2 additional round trips (~20-100ms).

**TLS 1.3** (modern) can do this in 1 round trip, or even 0 with session resumption.

---

## Step 5: The HTTP Request

With an encrypted channel established, the browser sends the HTTP request:

```
GET / HTTP/1.1
Host: google.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)...
Accept: text/html,application/xhtml+xml...
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate, br
Cookie: NID=512=abc123...; SID=xyz789...
```

The request includes:
- The path (`/`)
- Cookies (if you've visited before — used for auth, personalization, tracking)
- Accepted formats and languages
- Compression support

---

## Step 6: The Server Processes the Request

Google's server (actually, a load balancer → a fleet of servers) receives the request:

1. **Load balancer** routes the request to an available server (see `../08-cloud-and-deployment/scaling.md`)
2. The server checks:
   - Are you logged in? (reads cookies)
   - What's your location? (from your IP → geolocation)
   - What language? (from Accept-Language header)
   - What device? (from User-Agent)
3. The server renders the Google homepage:
   - Personalized if you're logged in (your account icon, search history)
   - Minimal if not (just the logo and search box)
4. The response is compressed (gzip or brotli) to reduce transfer size

---

## Step 7: The HTTP Response

```
HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Content-Encoding: br
Content-Length: 45000
Cache-Control: private, max-age=0
Set-Cookie: NID=513=def456...; expires=...

<html>...(the Google homepage)...</html>
```

The HTML is compressed and sent back over the encrypted TLS connection.

---

## Step 8: The Browser Renders

Your browser receives the HTML and begins rendering:

1. **Parse HTML** → build the DOM tree
2. **Parse CSS** → build the CSSOM (CSS object model)
3. **Render tree** — combine DOM + CSSOM → what actually gets painted
4. **Layout** — calculate where each element goes on screen
5. **Paint** — draw pixels on screen
6. **Fetch additional resources** — the page references JS, CSS, images, fonts

Each resource triggers its own DNS lookup, TCP handshake, and HTTP request (though the browser reuses connections and parallelizes aggressively).

For Google's homepage, this might involve:
- Google logo image
- Search box styling (CSS)
- JavaScript for search autocomplete, keyboard handling
- Font files

---

## Step 9: You See the Page

Your screen now shows the Google homepage. From pressing Enter to seeing the page: typically **100-300ms** for a fast connection, which includes:
- DNS: ~5-50ms
- TCP handshake: ~10-100ms
- TLS handshake: ~10-100ms
- HTTP request/response: ~20-100ms
- Rendering: ~10-50ms

---

## What's Happening in Parallel

While the page loads, other things are happening:
- **Preconnect** — the browser may open connections to Google's other domains (analytics, fonts, etc.) before you even need them
- **Prefetch** — the browser may download resources it predicts you'll need
- **Cookies are sent** — Google receives your previous session data
- **Tracking begins** — analytics scripts start collecting data about your visit

---

## The Scale Behind It

When you load `google.com`:
- Your request hits one of Google's **edge nodes** — servers in data centers around the world, probably within 50ms of you
- Google operates in **35+ regions** with hundreds of data centers
- Behind the edge node, the request may fan out to dozens of microservices
- Google serves **~8.5 billion searches per day**
- Each request is processed in milliseconds

You pressed one key. Millions of machines, thousands of miles of fiber, billions of dollars of infrastructure, and decades of engineering made that page appear in a blink.

That's the internet.
