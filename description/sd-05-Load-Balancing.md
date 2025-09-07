Here’s a **clear, structured overview of Load Balancing**, covering L4 vs L7, algorithms like Round Robin, Least Connections, and Consistent Hashing:

---

## **1. What is Load Balancing?**

Load balancing is the process of distributing incoming network traffic across multiple servers to:

* Prevent overloading a single server
* Improve performance & reliability
* Scale applications horizontally

Load balancers act as a **traffic manager**, deciding which backend server should handle each request.

---

## **2. Layer 4 (L4) vs. Layer 7 (L7) Load Balancing**

| Aspect                        | **Layer 4 (Transport)**                                      | **Layer 7 (Application)**                                               |
| ----------------------------- | ------------------------------------------------------------ | ----------------------------------------------------------------------- |
| **OSI Layer**                 | Layer 4 (TCP/UDP)                                            | Layer 7 (HTTP/HTTPS, gRPC)                                              |
| **Routing Decision Based On** | IP address + TCP/UDP port                                    | HTTP headers, cookies, path, hostnames, content                         |
| **Protocol Awareness**        | No awareness of application data                             | Full understanding of application-level data                            |
| **Performance**               | Faster (no deep packet inspection)                           | Slightly slower (parses request content)                                |
| **Use Case**                  | Simple TCP/UDP load balancing (e.g., database, game servers) | Web apps needing intelligent routing (A/B testing, routing by URL path) |

---

## **3. Common Load Balancing Algorithms**

### **a) Round Robin (RR)**

* **How it works:** Assigns requests to servers in a fixed order (Server 1 → Server 2 → … → Server N → repeat).
* **Pros:** Simple, fair when servers are identical.
* **Cons:** Doesn’t account for server load differences.

**Example:**
If you have 3 servers and 6 incoming requests → they are distributed as `S1, S2, S3, S1, S2, S3`.

---

### **b) Least Connections**

* **How it works:** Sends traffic to the server with the fewest active connections.
* **Pros:** Dynamically balances load based on current utilization.
* **Cons:** Slightly more overhead (tracks connection counts).

**Best For:** Long-lived connections (e.g., WebSockets, video streaming).

---

### **c) Weighted Round Robin / Weighted Least Connections**

* **Weighted RR:** Some servers get a higher share of requests (useful if servers have different capacities).
* **Weighted LC:** Takes both active connections and server weights into account.

---

### **d) Consistent Hashing**

* **How it works:** Uses a hash of a client identifier (e.g., IP, user ID) to consistently route the same client to the same server.
* **Pros:** Good for session persistence (stickiness).
* **Cons:** If servers are added/removed, many keys may remap unless a proper consistent hashing ring is used.

**Use Case:** Caching systems, session-based apps where state must stay on the same node.

---

## **4. Example Architecture**

**Typical Flow:**
`Client → DNS → Load Balancer (L4 or L7) → Algorithm (RR/LC/Hash) → Target Server`

---

<div align="center">
  <img src="https://github.com/SomnathRangrej/security/blob/main/images/Mermaid-Chart-workflow.png" alt="Description of the image" width="800" />
</div>

---

Absolutely — a **real-life example** makes load balancing much easier to understand.

Let’s break it down step by step with a **real scenario**:

---

## 🌐 **Real-Life Example: E-Commerce Website**

Imagine you run an online store like **Amazon**.
You have **millions of users**, and you can’t rely on a single server — it would crash under the load.

### **1️⃣ Layer 4 (L4) Load Balancing Example**

* **Scenario:**
  Your database cluster has multiple replicas.
  You use an **L4 load balancer** to distribute **TCP connections** for read queries.

* **Algorithm Used:**
  **Least Connections** – users streaming product reviews or keeping shopping carts open are spread evenly so no single database replica gets overloaded.

* **Why L4?**
  Because you don’t need to look inside the data — just route at the network level.
  It’s fast and efficient.

---

### **2️⃣ Layer 7 (L7) Load Balancing Example**

* **Scenario:**
  Your web frontend is made of several microservices:

  * `/products` → Product Service
  * `/cart` → Cart Service
  * `/checkout` → Payment Service

* **Algorithm Used:**
  **Round Robin** for `/products` (stateless requests)
  **Sticky Sessions** for `/cart` (to keep user’s session on the same server)
  **A/B Testing** routes 10% of `/checkout` traffic to a new payment gateway.

* **Why L7?**
  Because you need to **inspect the URL path** and make smart routing decisions.

---

### **3️⃣ Consistent Hashing Example**

* **Scenario:**
  You have a **distributed cache** (e.g., Redis cluster) for product data.
  You use **consistent hashing** so that:

  * Product ID `12345` always maps to the same cache node.
  * When you add or remove nodes, most of the keys **stay in place**, minimizing cache misses.

---

### 🖼 Putting It Together

**Flow for a Single User Request:**

1. User opens `https://store.com/products/shoes`.
2. **DNS** points them to your global load balancer.
3. **L7 load balancer** inspects the path `/products` and sends the request to one of the **product microservice servers** (using RR).
4. That microservice queries the **database cluster**, where an **L4 load balancer** distributes read requests to replicas (Least Connections).
5. The response is cached via **consistent hashing** so future users get faster results.

---

<div align="center">
  <img src="https://github.com/SomnathRangrej/security/blob/main/images/Mermaid-Chart-example.png" alt="Description of the image" width="800" />
</div>

---
