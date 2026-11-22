---
tags:
  - "#TRUNK"
  - SystemDesign
  - DistributedSystems
---

# System Design 101

Distributed systems are what separate software that runs on a laptop from software that serves a billion users. This note covers the foundational components — scaling, traffic control, data management, caching, and decoupling — that underpin every major platform from Netflix to Uber. Each concept builds on the last; by the end, you will be able to trace a single user request through a production-grade architecture and justify every design decision along the way.

---

# 1. The Scaling Decision: Vertical vs. Horizontal

When your application starts getting more traffic than a single machine can handle, you face a fork in the road. The wrong choice early can lock you into expensive architecture. The right choice depends entirely on your constraints at the time.

## Vertical Scaling (Scaling Up)

Vertical scaling means upgrading the machine itself — more RAM, a faster CPU, larger storage — without changing how the software is deployed.

**The Truck Analogy:** You own one delivery truck that handles 100 packages a day. Demand spikes to 5,000. You sell the truck and buy a massive super-truck with a jet engine.

| | Detail |
|---|---|
| **In computing** | Add RAM, faster CPU, or more storage to your *existing* server |
| **Pros** | Simple. No code changes. No distributed coordination needed. |
| **Cons** | **Hardware ceiling** — there is a physical limit to how powerful one machine can get. Creates a **Single Point of Failure (SPOF)**: if it goes down, everything goes down. |

> ⚠️ **The SPOF Problem:** Vertical scaling concentrates all risk in one machine. When that machine fails — and it will — your entire system fails with it.

## Horizontal Scaling (Scaling Out)

Horizontal scaling means adding *more* machines to your pool rather than upgrading any single one.

**The Truck Analogy continued:** Instead of one super-truck, you buy 50 regular trucks and hire 50 drivers.

| | Detail |
|---|---|
| **In computing** | Add more servers (nodes) to your cluster |
| **Pros** | Theoretically infinite scaling. If demand doubles, add more cheap servers. If one fails, the others keep running. |
| **Cons** | **Complexity.** You now manage a fleet. You need rules for who handles which request (**Load Balancing**) and how all servers stay in sync (**Data Consistency**). |

## The Golden Rule

> **In modern System Design, horizontal scaling is almost always preferred for large applications.** Vertical scaling hits a wall in both cost and physics.

**The Crucial Detail:** This does not mean you should always start horizontally. In the early stages of a startup, *engineering velocity* matters more than theoretical scale. Vertical scaling buys you time — pay for better hardware, skip the distributed complexity, and ship features. Migrate horizontally when the traffic actually demands it.

---

# 2. Traffic Control: Load Balancing

Once you have horizontally scaled to, say, 50 servers, a new problem appears: users connect to a single domain (`www.example.com`). How does the system decide which of the 50 servers handles any given request?

The answer is a **Load Balancer (LB)** — a dedicated component that sits in front of your server cluster and routes incoming traffic according to a set of rules.

**The Receptionist Analogy:** Visitors (user requests) arrive at a busy office building. The receptionist (the LB) looks at the available meeting rooms (servers) and directs each visitor to a specific room based on availability and capacity.

## Health Checks

The Load Balancer must know which servers are usable. It periodically **pings** every server in the cluster. If a server fails to respond — perhaps it has crashed — the LB stops sending traffic there automatically.

This is the mechanism behind **High Availability**: the system routes around failed nodes without human intervention.

## Routing Algorithms

The algorithm controls *how* the LB picks a server for each incoming request.

| Algorithm | Logic | Best For |
|---|---|---|
| **Round Robin** | Server A → B → C → A → ... | Requests of roughly equal cost |
| **Least Connections** | Route to the server currently handling the *fewest* active requests | Requests of highly variable duration |
| **IP Hash** | Hash the user's IP address to always map them to the same server | Sessions that require sticky state |

> ⚠️ **IP Hash Trade-off:** IP Hash guarantees session affinity (User A always hits Server B), but it can create uneven load — if a large corporate network has thousands of users behind one IP, they all land on the same server.

**The Crucial Detail — Choosing the Right Algorithm:** Round Robin assumes all requests and all servers are equal. When they are not, it fails badly. Consider a video upload service where one request is a 4KB text file (0.1 seconds to process) and another is a 4GB 4K video (10 minutes). Round Robin might send five massive video uploads to Server 1 in a row while Server 2 idles on tiny files. Server 1 crashes; Server 2 is wasted. **Least Connections** solves this: it tracks actual current load, not just turn order, so heavy uploads naturally spread across all available servers.

---

# 3. The Gatekeeper: API Gateways

With horizontal scaling and load balancing in place, the system can handle volume. But it is currently "naked" — every request that reaches the Load Balancer gets forwarded to a server regardless of whether it should.

Imagine you need to:
- Block a user who is flooding you with requests
- Verify a user is logged in *before* wasting server resources on their request
- Route `/billing` requests to the Billing Service and `/video` requests to the Video Service

The naive solution is to put this logic inside every server. With 50 servers, you are duplicating code 50 times. Change the authentication logic once and you must redeploy 50 services. This violates the **DRY (Don't Repeat Yourself) principle** at the infrastructure level.

The solution is an **API Gateway**: a dedicated server that acts as the single entry point — a "Facade" — between clients and all backend services.

```
User → API Gateway → Load Balancer → Server Cluster
         (checks ID,
         limits rate,
         routes path)
```

## Key Responsibilities

**1. Authentication & Authorization**
The Gateway validates the request's identity (JWT token, API key) before the request travels any further. Unauthorized requests are rejected at the edge — your backend servers never see them.

**2. Rate Limiting**
Controls how many requests a single user or IP can make in a given time window (e.g., max 60 requests/minute). This protects against both malicious DDoS attacks and accidental runaway clients.

**3. Request Routing**
The Gateway inspects the request path (`/billing`, `/video`, `/search`) and forwards it to the correct microservice. This is the backbone of microservice architecture.

**4. Protocol Translation**
External clients speak HTTP/REST. Internal services may use faster binary protocols like **gRPC** or **Thrift**. The Gateway translates between them, shielding the outside world from internal implementation details.

## The Gateway Aggregation Pattern

A mobile app's home screen might require data from three separate services: Account Info, Orders, and Recommendations. Without a Gateway, the mobile phone fires three separate network requests across the internet — slow, battery-draining, and fragile.

With a Gateway, the mobile app sends **one request**. The Gateway fans out to all three services *internally* (over fast, local network), aggregates the responses, and returns a single payload to the client. Three slow round-trips become one.

> **The Crucial Detail:** An API Gateway is not magic. It is simply a server (or cluster of servers) running specialized software — Netflix Zuul, Spring Cloud Gateway, Kong (built on NGINX), or a managed service like AWS API Gateway. It requires CPU, RAM, and network bandwidth just like any other server. If traffic spikes, you must scale your Gateway horizontally and place a Load Balancer in front of *it*, or the Gateway itself becomes the bottleneck.

---

# 4. Theoretical Constraints: The CAP Theorem

Before designing a distributed database, you must understand what is fundamentally impossible — not just what is hard. The CAP Theorem, proposed by Eric Brewer, defines the ceiling of what any distributed data store can guarantee.

**The theorem states:** In a distributed data store, you can only provide *two* of the following three guarantees simultaneously.

| Guarantee | Definition | Plain English |
|---|---|---|
| **Consistency (C)** | Every read receives the most recent write or an error | If I update a value to "5", everyone sees "5" immediately |
| **Availability (A)** | Every request receives a non-error response, without the guarantee it contains the most recent write | The system is always "up", but may show stale data |
| **Partition Tolerance (P)** | The system continues operating despite messages being dropped or delayed between nodes | The network between Server A and Server B breaks, but the system survives |

## Why P Is Non-Negotiable

In the real world, networks fail. Cables get cut. Routers crash. Cloud availability zones lose connectivity. A system that shuts down every time the network blinks is not a distributed system — it is a liability.

**Partition Tolerance is therefore mandatory.** You cannot opt out of it. This reduces the CAP choice to a single binary decision:

> **CP or AP?**

## CP — Consistency + Partition Tolerance

When a network partition occurs between two database nodes, a CP system **locks down**. It refuses reads and writes until connectivity is restored, to prevent nodes from diverging.

- **Result:** Users see an error. The system is "down." But every data record is perfectly accurate.
- **Use case:** Banking systems, financial ledgers, inventory management — anywhere where showing wrong data is worse than showing no data.

## AP — Availability + Partition Tolerance

When a partition occurs, an AP system **keeps accepting reads and writes on both sides**. Node A might record a value of "5" while Node B records "6". They will reconcile later.

- **Result:** The system stays up and fast, but users may temporarily see stale or inconsistent data. This is called **Eventual Consistency** — the nodes will sync up, usually within milliseconds or seconds.
- **Use case:** Social media like counts, YouTube view counters, product recommendation feeds.

> **The Crucial Detail:** Eventual Consistency is not a failure mode — it is a deliberate design choice. For a social media "likes" counter, it does not matter if one user sees 1,005 likes and another sees 1,006. What matters is that both users can use the app. Prioritising availability here is the correct business decision.

---

# 5. Data Management: Replication

Web servers are stateless — if one crashes, you spin up a new one and nothing is lost. Databases are different. If your database crashes and you have no copy of the data, it is gone. Replication is the first line of defence.

**Replication** means maintaining copies of your data on multiple servers simultaneously. The most common pattern is **Master-Slave Architecture**.

## How Master-Slave Works

```
Write Request → Master Node → (async copy) → Slave Node A
                                           → Slave Node B
Read Request  →                              Slave Node A (or B)
```

1. **The Master Node** is the only server permitted to accept writes (INSERT, UPDATE, DELETE).
2. **Slave Nodes** continuously copy data from the Master. They are read-only.
3. All read requests are routed to Slaves, freeing the Master to focus on writes.

**Why this matters:**
- **Failover (Safety):** If the Master crashes, a Slave is promoted to Master. The system survives.
- **Read Scaling:** Most applications read far more than they write (you view a tweet 100 times for every one you post). More Slaves = more read capacity, with zero impact on write performance.

## Asynchronous vs. Synchronous Replication

The replication *mode* determines whether Master-Slave leans CP or AP.

| | Asynchronous (default) | Synchronous |
|---|---|---|
| **How it works** | Master saves, immediately confirms "Success" to user, then copies to Slave in the background | Master saves, sends to Slave, *waits* for Slave to confirm, then tells user "Success" |
| **Speed** | Fast — user doesn't wait for the Slave | Slower — write latency includes the Slave round-trip |
| **Data Safety** | If Master crashes before replicating, that write is lost forever | Zero data loss — Slave always has the latest write |
| **CAP lean** | AP (High Availability, lower Consistency) | CP (High Consistency, lower Availability) |
| **Risk** | Data loss on crash | If Slave goes offline, Master cannot complete *any* write — system halts |

## Replication Lag and the "Missing Order" Problem

With asynchronous replication, there is always a brief window — called **Replication Lag** — during which a Slave has not yet received the latest write from the Master.

**Scenario:** A user completes a purchase. The Master records it. Before the Slave replicates it, the user refreshes their Order History page. That page is a read — routed to a Slave. The Slave doesn't have the order yet. The user panics.

This is not a bug. It is the expected behaviour of an AP-leaning system. But for high-stakes reads immediately following a user's own write, it is unacceptable UX.

**The Fix: Read Your Own Writes**

> The system detects that *this specific user* just performed a write. For the next short window, their read requests are routed directly to the Master (or verified against it), while all other users continue reading from Slaves.

This gives you the performance benefits of asynchronous replication for the general case, while guaranteeing consistency for the one user who cares most.

---

# 6. Data Management: Sharding & Partitioning

Replication solves redundancy and read scaling. It does not solve *storage scale*. If your Master database grows to 5 Terabytes and your server cannot hold more disk, adding more Slave replicas does not help — they all hold the same 5TB. You need to *split* the data itself.

**Sharding** is the practice of partitioning a database's rows horizontally across multiple servers. Each server holds a subset of the data and is called a **Shard**. The rule that determines which data goes to which shard is the **Sharding Key**.

## Range-Based Sharding

Split data based on a continuous range of the key.

- **Example:** User IDs 1–1,000,000 → Shard A. User IDs 1,000,001–2,000,000 → Shard B.
- **Pros:** Simple to implement. Excellent for range queries ("give me all orders placed in January").
- **Cons:** The **Hotspot Problem.** If all new signups happen today, they all land on the latest shard. That shard melts under load while earlier shards sit idle.

## Hash-Based Sharding

Run the key through a hash function, then use modulo arithmetic to assign a shard.

```
server_number = hash(user_id) % number_of_servers
```

- **Example:** `hash("Alice")` = 400. With 3 servers: `400 % 3 = 1`. Alice goes to Server 1.
- **Pros:** Even, pseudo-random distribution. No single shard gets disproportionately hot.
- **Cons:** The **Resharding Storm.**

> ⚠️ **The Resharding Storm:** You have 10 servers using `hash(id) % 10`. You add one server — now `hash(id) % 11`. Almost every user now maps to a *different* server than before. You must immediately migrate ~90% of your data. Your database grinds to a halt during the move.

## Consistent Hashing

Consistent Hashing solves the Resharding Storm by replacing the modulo formula with a **Ring** model.

```
         Server A
        /         \
   Server C      Server B
        \         /
         (ring)
```

- Servers and data keys are both placed on the ring using a hash function.
- A key is owned by the first server it encounters travelling clockwise around the ring.
- **When a server is added or removed, only the keys belonging to its immediate neighbour on the ring need to move.** Everything else stays put.

This reduces data movement from "migrate everything" to "migrate a small slice."

## Virtual Nodes — Solving the Uneven Ring

With only 3 physical servers on the ring, random placement may give Server A 50% of the ring and Server B only 10%. The load is uneven again — just a different kind of uneven.

**The Pizza Analogy:** Cutting a pizza into 3 slices gives one friend a huge half-moon slice and another a tiny sliver. But if you cut it into 300 tiny slices and deal them out like cards — one for A, one for B, one for C — each friend gets 100 scattered slices. Even if one region of the pizza is "hot", everyone shares the load.

**Virtual Nodes** apply this principle: instead of placing Server A once on the ring, it is placed as `A1, A2, A3, ... A100`. These are virtual identities that all map to the same physical machine. The result is statistically even data distribution across all physical servers, regardless of how many there are.

> This is how production systems like **DynamoDB** and **Cassandra** operate under the hood.

---

# 7. Speed & Latency: Caching

Even with sharded databases and replication, every read still hits a disk — a hard drive or SSD. This is the speed bottleneck that caching exists to eliminate.

| Storage | Typical Read Latency |
|---|---|
| Hard Disk (HDD) | ~10 milliseconds |
| SSD | ~0.1 milliseconds |
| RAM (in-memory cache) | ~0.0001 milliseconds (100 nanoseconds) |

The difference between disk and RAM is roughly **1,000x**. A **cache** (e.g., Redis or Memcached) is an in-memory store that sits in front of your database. Frequently accessed data lives there, and reads skip the database entirely.

## The Cache-Aside Pattern (Lazy Loading)

This is the most common caching strategy. The application manages the cache explicitly.

```
1. Request  → App asks Cache: "Do you have User A's profile?"
2. Cache Miss → Cache says "No."
3. Fetch    → App queries the Database (slow).
4. Store    → App saves the result in the Cache for next time.
5. Response → App returns data to the user.
```

On the *second* request for the same data, steps 3 and 4 are skipped entirely. The response is instantaneous.

**The trade-off:** RAM is expensive and finite. You cannot cache everything. This raises a critical question: when the cache fills up, what do you delete?

## Eviction Policies

| Policy | Logic | Risk |
|---|---|---|
| **FIFO** (First In, First Out) | Delete the item that was *added* to the cache earliest | Evicts popular items simply because they were cached first |
| **LRU** (Least Recently Used) | Delete the item that hasn't been *accessed* for the longest time | Small risk of evicting items that are rarely-but-importantly accessed |
| **LFU** (Least Frequently Used) | Delete the item accessed the *fewest* times overall | Slow to adapt to sudden popularity spikes on new content |

**Choosing the right policy matters.** Consider a Breaking News website:

- The "Election Results" article is read millions of times today, but will be ignored by tomorrow.
- An archive article from 1990 is read once a year.

**FIFO** would delete "Election Results" simply because it was cached first — potentially replacing it with a trivial article added 5 seconds ago. A performance disaster.

**LRU** is correct here. Every time a user reads "Election Results", it moves to the front of the eviction queue. The 1990 archive article, untouched for hours, sinks to the back and gets evicted. The next day, when "Election Results" itself goes cold, it too sinks to the back and naturally makes room for the next breaking story.

---

# 8. Global Delivery: Content Delivery Networks (CDNs)

Redis caches database query results in memory, close to your application servers. But your application servers are in one place — say, New York. A user in London still has to wait for static assets (images, videos, CSS, JavaScript) to travel across the Atlantic on every page load. The speed of light is fast, but not instant. At scale, this latency is measurable and painful.

A **CDN** (e.g., Cloudflare, AWS CloudFront, Akamai) is a globally distributed network of servers — called **Edge Servers** or **Points of Presence (PoPs)** — that cache your static assets close to users worldwide.

```
Origin Server (New York)
    ↓ (first request only)
Edge Server — London  ←── London users (subsequent requests, instant)
Edge Server — Tokyo   ←── Tokyo users
Edge Server — Sydney  ←── Sydney users
```

## How It Works

1. **First Request:** A London user requests `logo.png`. The London Edge Server has no copy. It fetches it from the New York Origin Server (slow), caches it locally, and serves it.
2. **All Subsequent Requests:** The next 10,000 London users request the same file. The Edge Server serves it instantly from local storage. The Origin Server is never contacted.

**Benefits:**
- **Latency:** Global users experience near-local response times for static content.
- **Load:** Your Origin Server is freed from serving heavy assets, conserving CPU and bandwidth for dynamic requests.

## Cache Invalidation: The Hardest Problem

CDN caches hold files for a configured duration — the **TTL (Time To Live)**. A 24-hour TTL means the CDN holds a file for 24 hours before checking for a newer version. This is efficient, but creates a problem: what if you need to update a file *right now*?

**Scenario:** Your marketing team replaces `logo.png` on the Origin Server. Users are complaining they still see the old logo. You cannot wait 24 hours for the TTL to expire. The CDN's "Purge All" API exists, but it is slow to propagate globally and can cost money.

**The Engineering Solution: Cache Busting via Versioning**

Instead of telling the CDN to forget the old file, trick it into treating the new file as a *brand new resource* by changing the filename or adding a version query parameter in your HTML:

```html
<!-- Old HTML — CDN serves the stale cached version -->
<img src="logo.png">

<!-- New HTML — CDN has never seen this URL; fetches fresh from Origin -->
<img src="logo.png?v=2">
<!-- or -->
<img src="logo_v2.png">
```

The CDN sees `logo.png?v=2` as a completely different request. It has no cached copy, so it immediately fetches the new image from the Origin Server. The update is instant and free.

> **The Crucial Detail:** Modern build tools (Webpack, Vite, etc.) automate this entirely. They hash the file contents into the filename — `logo.a3f9c1.png` — so the filename changes automatically whenever the file contents change. You never manually bump version numbers.

---

# 9. Decoupling Components: Message Queues

Every component in our system so far communicates **synchronously**: Service A calls Service B and waits for a response before doing anything else. This is intuitive, but fragile.

```
Service A ──calls──► Service B (slow or crashed)
    └── waits...
    └── waits...
    └── times out / crashes too
```

If Service B is slow, Service A is blocked. If Service B crashes, Service A crashes with it. This is called **cascading failure** — one slow component can bring down the entire call chain.

**Message Queues** (e.g., RabbitMQ, Apache Kafka, Amazon SQS) break this coupling by introducing asynchronous communication.

## The Restaurant Analogy

Think of a Message Queue as the order rail in a restaurant kitchen:

1. **Producer (Waiter / Web Server):** Takes the customer's order, clips the ticket to the rail, and immediately returns to serve the next table. They do not wait for the food to be cooked.
2. **Queue (The Rail):** Holds the tickets in order, durably.
3. **Consumer (Chef / Worker Server):** Picks up a ticket when ready, processes it, and signals completion.

The waiter and chef are now fully decoupled. If the kitchen slows down, tickets pile up on the rail — but the waiter keeps serving customers. The system degrades gracefully instead of collapsing.

## Smoothing Traffic Spikes

This decoupling is particularly powerful under sudden load.

**Scenario:** 1,000 users simultaneously upload photos that need to be resized.

| Without MQ | With MQ |
|---|---|
| Web server tries to resize 1,000 photos instantly | Web server accepts uploads and places 1,000 "Resize" jobs on the Queue |
| Server runs out of RAM and crashes | Worker server processes jobs steadily at its own pace |
| All 1,000 users get errors | Users see "Processing..." — system stays up |

## The ACK Mechanism: Guaranteeing Delivery

A naive queue might delete a message the moment it hands it to a Worker. If the Worker crashes mid-process, the task is gone forever. For a financial transaction queue, this means lost money.

Robust Message Queues solve this with **Acknowledgements (ACKs)**:

```
1. Queue delivers message to Worker.
   └── Does NOT delete it. Marks it "Invisible" (e.g., for 30 seconds).

2a. Worker finishes → sends ACK → Queue permanently deletes the message. ✓

2b. Worker crashes → no ACK received → timeout expires →
    Queue marks message "Visible" again → re-delivers to a different Worker. ✓
```

This pattern guarantees **At-Least-Once Delivery**: every message will be processed successfully, even if a Worker fails partway through.

> ⚠️ **At-Least-Once means exactly that.** In rare cases (Worker finishes but ACK is lost in transit), a message may be processed *twice*. Systems that cannot tolerate duplicate processing (e.g., charging a credit card) must be designed to be **idempotent** — processing the same message twice produces the same result as processing it once.

