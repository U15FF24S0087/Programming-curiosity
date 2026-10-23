# Cloud

## What Is the Cloud?

The cloud is someone else's servers. Instead of buying and maintaining your own hardware, you rent computing resources from a cloud provider over the internet.

```
Traditional: You buy a server, put it in a room, maintain it
Cloud:       You rent a server for $0.05/hour, turn it off when done
```

The provider handles the hardware, networking, power, cooling, and physical security. You focus on your application.

---

## Why the Cloud Exists

Before cloud computing, getting a server meant:
1. Buy hardware (weeks of procurement)
2. Rack it in a data center
3. Configure the network
4. Install the OS
5. Maintain it (power, cooling, hardware failures)

This was slow, expensive, and hard to scale. If you needed 10 more servers for a traffic spike, you couldn't get them quickly.

The cloud lets you provision servers in **seconds** with a few clicks or an API call, and pay only for what you use.

---

## Major Cloud Providers

| Provider | Key Products | Notes |
|---|---|---|
| **AWS** (Amazon) | EC2, S3, Lambda, RDS | Market leader, widest selection |
| **Azure** (Microsoft) | VMs, Blob Storage, Functions | Strong in enterprise, integrates with Microsoft tools |
| **Google Cloud** | Compute Engine, Cloud Storage | Strong in data/AI, Kubernetes origin |
| **DigitalOcean** | Droplets | Simpler, developer-friendly, cheaper |
| **Vercel / Netlify** | Static hosting, serverless | Focused on frontend deployment |

---

## Cloud Service Models

### IaaS (Infrastructure as a Service)
You rent raw computing resources — VMs, storage, networking. You manage the OS, runtime, and app.

```
You manage: OS, runtime, app, data
Provider manages: hardware, networking
Example: AWS EC2, Azure VMs
```

### PaaS (Platform as a Service)
You deploy your app; the provider manages the OS, runtime, and infrastructure.

```
You manage: app, data
Provider manages: OS, runtime, hardware, networking
Example: Heroku, Google App Engine
```

### SaaS (Software as a Service)
You just use the software. The provider manages everything.

```
You manage: nothing (just use it)
Provider manages: app, data, OS, runtime, hardware
Example: Gmail, Slack, Salesforce
```

### FaaS (Function as a Service / Serverless)
You write individual functions. The provider runs them on demand, scaling automatically. You pay per execution.

```
You manage: function code
Provider manages: everything else, including scaling
Example: AWS Lambda, Google Cloud Functions
```

---

## The Shared Responsibility Model

Security and maintenance responsibilities are split between you and the provider:

| | On-Premises | IaaS | PaaS | SaaS |
|---|---|---|---|---|
| Hardware | You | Provider | Provider | Provider |
| OS | You | You | Provider | Provider |
| Runtime/App | You | You | You | Provider |
| Data | You | You | You | You (mostly) |

The more the provider manages, the less control you have. The tradeoff: convenience vs flexibility.

---

## Key Cloud Concepts

### Elasticity
Scale up when demand increases, scale down when it decreases. You're not stuck paying for peak capacity 24/7.

```
Traffic spike → add 5 more servers automatically
Traffic drops → remove them, stop paying
```

### Pay-as-You-Go
You pay for what you use, by the hour or second. No upfront hardware costs.

### Regions and Availability Zones
Cloud providers have data centers around the world. A **region** is a geographic area (e.g., us-east-1 in Virginia). Each region has multiple **availability zones** (isolated data centers). Deploying across zones provides redundancy.

### Managed Services
The provider runs and maintains a service for you:
- **RDS**: Managed databases (automatic backups, patching, failover)
- **S3**: Managed object storage (infinite scale, pay per GB)
- **CDN**: Content delivery network (cache content close to users)

---

## Serverless

Serverless doesn't mean "no servers" — it means **you don't manage them**. The provider handles provisioning, scaling, and maintenance. You only write code.

```
Traditional: Server running 24/7, you pay for idle time
Serverless:  Function runs only when called, you pay per execution
```

**Advantages**: no server management, automatic scaling, pay per use.  
**Disadvantages**: cold starts (delay on first invocation), vendor lock-in, harder to debug, harder for long-running tasks.

---

## Cloud vs On-Premises: The Tradeoff

| | Cloud | On-Premises |
|---|---|---|
| Upfront cost | Low (pay as you go) | High (buy hardware) |
| Scaling | Easy, elastic | Hard (buy more hardware) |
| Maintenance | Provider handles infrastructure | You handle everything |
| Control | Limited by provider's offerings | Full control |
| Vendor lock-in | Risk (provider-specific APIs) | None |
| Predictable cost? | Can spiral if not monitored | Fixed hardware cost |

Most companies use cloud for flexibility. Some (banks, government, healthcare) keep sensitive data on-premises for compliance. Many use a **hybrid** approach.

See `server.md` for what a server is, and `scaling.md` for handling growth.
