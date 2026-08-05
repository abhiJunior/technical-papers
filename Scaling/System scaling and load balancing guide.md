# Scalability in System Design

## 1. What is Scaling?

As an application grows, more users, requests, and data start hitting the system. If the system isn't built to handle this growth, it becomes slow, crashes often, or stops working entirely.

> **Scaling** = increasing a system's capacity so it can smoothly handle more traffic and workload.

### Client-Server Model (why scaling matters)

```
 [Client: Browser/Mobile App]  --request-->  [Server]
 [Client: Browser/Mobile App]  <--response--  [Server]
```

- **Client** sends a request.
- **Server** processes it and sends back a response.
- As the number of clients grows, the server receives more requests → risk of overload.
- Scaling lets the server keep handling more clients **without downtime or slowdowns**.

---

## 2. Types of Scaling

There are two main ways to scale a system: **Vertical** and **Horizontal**.

```
                 SCALING
                /        \
      VERTICAL              HORIZONTAL
    (Scale-Up)               (Scale-Out)
   "Bigger engine"        "More cars sharing load"
```

### 2.1 Vertical Scaling (Scale-Up)

**Definition:** Increase the capacity of a *single* machine by adding more resources (RAM, CPU, storage) instead of adding new machines.

```
Before:                     After (Vertical Scaling):
+----------------+          +----------------------+
|  Server        |          |  Server (upgraded)    |
|  8 GB RAM      |   --->   |  16 GB RAM             |
|  128 GB Disk   |          |  500 GB Disk           |
+----------------+          +----------------------+
```

- Example database: **MySQL**

**Advantages**
| # | Benefit |
|---|---------|
| 1 | Easy to implement |
| 2 | Lower software cost (no new resources/machines) |
| 3 | Less effort to maintain (still just one system) |

**Disadvantages**
| # | Drawback |
|---|----------|
| 1 | Single point of failure |
| 2 | High downtime if the server fails (only one server exists) |
| 3 | High risk from hardware failure |

**Real-world example:** Traffic increases → server slows down → team upgrades RAM (8 GB → 16 GB) and disk (128 GB → 500 GB). Works for a while, but hardware has a ceiling — eventually it saturates and can't be pushed further.

---

### 2.2 Horizontal Scaling (Scale-Out)

**Definition:** Add *more machines/instances* of the same type to the existing pool, instead of upgrading one machine's power. A **Load Balancer** routes requests across these servers based on availability.

```
                     +----------------+
                     |  Load Balancer |
                     +----------------+
                     /       |        \
              +--------+ +--------+ +--------+
              |Server 1| |Server 2| |Server 3|
              +--------+ +--------+ +--------+
```

- Example databases: **NoSQL, Cassandra, MongoDB**

**Advantages**
| # | Benefit |
|---|---------|
| 1 | Fault tolerance — no single point of failure (many servers back each other up) |
| 2 | Low latency — requests processed faster since load is spread out |
| 3 | Built-in backup (other servers keep running if one fails) |

**Disadvantages**
| # | Drawback |
|---|----------|
| 1 | Harder to implement (more moving parts) |
| 2 | Higher cost |
| 3 | Needs networking components — routers, load balancers |

**Real-world example:** System has 8 GB RAM capacity and needs to handle a load equal to 16 GB. Instead of upgrading to a single bigger machine, you add another 8 GB instance alongside it — two machines sharing the work.

---

### 2.3 Vertical vs Horizontal — Quick Comparison

| Aspect | Vertical Scaling | Horizontal Scaling |
|---|---|---|
| Approach | Make one server bigger | Add more servers |
| Analogy | Bigger engine in one car | More cars sharing the workload |
| Failure risk | Single point of failure | Fault-tolerant (others back it up) |
| Cost | Lower initially | Higher (extra machines + networking) |
| Complexity | Simple | Complex (needs load balancer, coordination) |
| Scaling limit | Hits a hardware ceiling | Can scale almost indefinitely |
| Example tech | MySQL | MongoDB, Cassandra, NoSQL |

---

## 3. What is Scalability?

**Scalability** = a system's ability to handle increasing workload, users, or data **without losing performance** — by adding resources like servers, storage, or processing power when needed.

- When load increases, a scalable system keeps performance, efficiency, and reliability intact.
- It avoids needing a full redesign just because traffic or data grew.

**Example:** A video streaming platform automatically spins up more servers when millions of users start watching at the same time.

### Real-World Examples of Scalable Systems

| Company | How They Scale |
|---|---|
| **Google** | Distributed systems like Bigtable, MapReduce, Spanner to handle billions of searches |
| **AWS** | Scalable cloud services — compute, storage, databases scale on demand |
| **Netflix** | Cloud infrastructure + microservices + caching to stream to millions at once |

---

## 4. Other Approaches to Achieve Scalability

Besides vertical/horizontal scaling, a few related strategies:

```
Divide and Conquer  --->  Microservices (break app into small independent services)
No Servers, No Problems --->  Serverless (auto-scales, no manual server management, e.g. AWS Lambda)
```

- **Microservices:** Break the app into small, independent services — scale only the parts that need it.
- **Serverless:** No manual server management; scales automatically with demand (e.g., AWS Lambda). Cost-efficient for variable workloads.

---

## 5. Factors Affecting Scalability

| Factor | Why it Matters |
|---|---|
| **Performance Bottlenecks** | Slow databases, inefficient code, or limited resources drag down the whole system |
| **Resource Utilization** | Efficient use of CPU/memory/disk avoids bottlenecks |
| **Network Latency** | Delay in data transmission between nodes slows communication |
| **Data Storage & Access** | How data is stored/accessed matters — distributed DBs + caching help |
| **Concurrency & Parallelism** | Handling multiple tasks at once boosts throughput, but needs careful management to avoid sync overhead |
| **System Architecture** | Modular, loosely-coupled systems scale (both vertically & horizontally) more easily |

## 6. Components That Help Increase Scalability

- **Load Balancer** — distributes traffic across servers
- **Caching** — stores frequently accessed data to reduce latency/backend load
- **Database Replication** — multiple copies of data for availability & read performance
- **Database Sharding** — splits data into shards across multiple DB instances
- **Microservices Architecture** — independent services scale separately
- **Data Partitioning** — splits data by criteria (user/region) for better scalability
- **CDNs** — serve cached content from locations closer to users
- **Queueing Systems** — handle requests asynchronously to absorb traffic spikes

## 7. Challenges & Trade-offs

| Trade-off | Explanation |
|---|---|
| Cost vs Scalability | Better performance/availability usually costs more infrastructure/ops budget |
| Complexity | Bigger scaled systems are harder to manage, maintain, debug |
| Latency vs Throughput | Optimizing for one often costs the other |
| Data Partitioning Trade-offs | Needs balance of partition size, data movement, and locality |

---

# Load Balancer

## 8. What is a Load Balancer?

A **load balancer** is a device or software application that distributes incoming traffic across multiple servers — improving availability, performance, and resource efficiency.

> Think of it as a **traffic cop** — directing client requests to the right server so no single server gets overwhelmed.

**Restaurant analogy:** Instead of one chef cooking every order, multiple chefs split the work — customers get served faster.

**Examples:** NGINX, HAProxy, AWS Elastic Load Balancing

```
        Requests from many users
                  |
                  v
          +----------------+
          | Load Balancer  |
          +----------------+
           /      |       \
      Server1  Server2  Server3
```

---

## 9. How a Load Balancer Works

1. **Receives Incoming Requests** — user requests hit the load balancer first, not the servers directly.
2. **Checks Server Health** — continuously monitors which servers are healthy.
3. **Distributes Traffic** — routes each request to the best server (based on load, response time, or proximity).
4. **Handles Server Failures** — stops sending traffic to a down server and reroutes to healthy ones.
5. **Optimizes Performance** — spreads traffic efficiently to reduce delays.

```
User Requests
     |
     v
+----------------+       Health Checks (heartbeat)
| Load Balancer  |<---------------------------------+
+----------------+                                   |
   |     |     |                                      |
   v     v     v                                      |
 [S1]  [S2]  [S3(down)] -----------------------------+
  OK    OK      X  --> traffic rerouted to S1 & S2
```

---

## 10. Key Characteristics

| Characteristic | What it Does |
|---|---|
| Traffic Distribution | Spreads requests evenly so no single server is overburdened |
| High Availability | Reroutes to healthy servers if one fails |
| Scalability | Makes it easy to add servers (enables horizontal scaling) |
| Optimization | Efficient use of server capacity, avoids bottlenecks |
| Health Monitoring | Detects issues/downtime on servers |
| SSL Termination | Handles SSL/TLS encryption/decryption, offloading it from servers |

---

## 11. Types of Load Balancers

### By Deployment

| Type | Description | Example |
|---|---|---|
| **Hardware** | Dedicated physical device, used in large data centers, very high performance | F5 Networks appliances |
| **Software** | Runs as an application, flexible & cost-effective | NGINX, HAProxy |
| **Cloud** | Managed service, auto-distributes traffic, no infra management needed | AWS Elastic Load Balancing |

### By OSI Layer

| Layer | Operates On | Behavior | Example |
|---|---|---|---|
| **Layer 4** (Transport) | IP address, TCP/UDP port | Fast, doesn't inspect request content | Routes TCP requests by port/IP |
| **Layer 7** (Application) | HTTP headers, URLs, cookies, content | Smarter routing based on request type | `/images` → Server A, `/api` → Server B |

---

## 12. Server Health Monitoring

```
     +------------------+
     |   Load Balancer   |
     +------------------+
        |    heartbeat / health check
        v
   +---------+     healthy -> keep sending traffic
   | Server  | --->
   +---------+     unhealthy -> stop sending traffic,
                                reroute to others
```

1. **Active Health Checks / Heartbeat Monitoring** — LB periodically pings servers (HTTP/TCP/ICMP) to verify they're alive. Repeated failed heartbeats → marked unhealthy.
2. **Passive Health Checks** — LB watches real traffic for errors/timeouts; marks a server down without needing separate test pings.
3. **Automatic Failover & Recovery** — unhealthy servers are removed from rotation immediately; once they pass health checks again, they're added back — minimal disruption to users.

**Example:** During a flash sale on an e-commerce site, if one server crashes under load, the load balancer detects it via heartbeat monitoring and reroutes traffic to healthy servers — preventing downtime and lost orders.

---

## 13. Problems Without a Load Balancer

| Problem | Impact |
|---|---|
| **Single Point of Failure** | If the one server goes down, the entire app becomes unavailable |
| **Overloaded Servers** | A server has a request limit; growth beyond that overloads it |
| **Limited Scalability** | Can't easily add servers — all traffic is stuck hitting one server |

---

## 14. Challenges & Risks of Load Balancers

| Risk | Description |
|---|---|
| Single Point of Failure | If the LB itself fails, traffic can't reach servers (unless backup LBs exist) |
| Performance Bottleneck | LB itself can slow down under very high traffic |
| Configuration Complexity | Setting up correctly for large-scale apps is non-trivial |
| Security Risks | Sits between users & servers — a prime target for attacks |
| Cost | Hardware LBs + high-availability setups add infrastructure cost |

---

## 15. Summary — The Big Picture

```
              Growing Traffic / Users
                        |
                        v
      +----------------------------------+
      |     How do we handle this?       |
      +----------------------------------+
           |                       |
   Vertical Scaling         Horizontal Scaling
   (upgrade 1 server)       (add more servers)
           |                       |
      Hits a limit          Needs a Load Balancer
                                    |
                          +-------------------+
                          |   Load Balancer   |
                          | - distributes     |
                          |   traffic         |
                          | - checks health   |
                          | - reroutes on     |
                          |   failure         |
                          +-------------------+
                                    |
                     High Availability + Better Performance
```

**Key takeaway:** Vertical scaling is simple but limited and risky (single point of failure). Horizontal scaling is more resilient and scales further, but needs a load balancer to intelligently distribute traffic and handle server failures.
