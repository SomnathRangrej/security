Overview of **capacity estimation** covering **QPS (queries per second), storage, and bandwidth**. Here’s a structured breakdown:

---

## **1. Capacity Estimation Overview**

Capacity estimation is about predicting **how much infrastructure you need** to handle current and future workloads without service degradation. It typically focuses on:

* **Compute** – CPU & memory for handling requests
* **Storage** – how much data will be stored, and at what growth rate
* **Network** – bandwidth needed to serve data at peak load
* **QPS** – system throughput (how many requests per second it can handle)

---

## **2. Estimating QPS (Queries Per Second)**

### **Step 1: Understand Workload**

* Identify **peak hours** (traffic is usually not evenly distributed).
* Collect historical request logs (if available).
* Define **service level objective (SLO)** — e.g., 99th percentile latency < 200ms.

### **Step 2: Calculate Peak QPS**

Formula:

$$
\text{QPS} = \frac{\text{Total requests in peak period}}{\text{Duration of peak period (in seconds)}}
$$

Example:

* 360,000 requests in 1 hour peak window
* QPS = 360,000 ÷ 3600 = **100 QPS**

### **Step 3: Add Safety Buffer**

Add **headroom** (usually 30–50%) to handle traffic spikes:

$$
\text{Provisioned QPS} = \text{Peak QPS} \times (1 + \text{Buffer %})
$$

---

## **3. Storage Capacity Estimation**

### **Step 1: Estimate Data per Request**

Determine how much data is generated/stored per request:

* Request payload size
* Response size (if logging responses)
* Database writes, logs, metadata

### **Step 2: Calculate Daily Growth**

$$
\text{Daily Storage} = \text{Requests per day} \times \text{Data per request}
$$

Example:

* 10M requests/day × 5 KB/request → **50 GB/day**

### **Step 3: Account for Retention**

$$
\text{Total Storage} = \text{Daily Storage} \times \text{Retention Period (days)}
$$

Add **20–30% buffer** for growth & overhead (indexes, compression inefficiencies).

---

## **4. Bandwidth Estimation**

### **Step 1: Calculate Payload Size**

$$
\text{Avg Payload Size} = \text{Request Size} + \text{Response Size}
$$

### **Step 2: Calculate Throughput**

$$
\text{Network Throughput (bps)} = \text{QPS} \times \text{Avg Payload Size (bytes)} \times 8
$$

Convert to **Mbps or Gbps** for network planning.

Example:

* QPS = 500
* Avg payload = 2 KB/request → 2 × 8 = 16 Kb/request
* Throughput = 500 × 16 Kb = **8,000 Kbps = 8 Mbps**

---

## **5. Best Practices**

* **Use Percentiles (P95, P99)** for QPS and latency, not just averages.
* **Account for future growth** — plan 6–12 months ahead.
* **Monitor & Adjust** — build dashboards for live metrics (QPS, CPU, bandwidth).
* **Perform Load Testing** to validate estimates.
* **Consider Redundancy** — HA setups double resource needs.

---

Would you like me to create a **visual diagram (mermaid)** showing how QPS, storage, and bandwidth estimation connect (input → calculation → provisioning)? That would make it easier to explain to a team.

<div align="center">
  <img src="https://github.com/SomnathRangrej/security/blob/main/images/mermaid-diagram-Capacity-Estimation.png" alt="Description of the image" width="600" />
</div>

Here’s a **clear tabular summary** of the same capacity estimation example — perfect for docs, wikis, or spreadsheets:

| **Category**            | **Step**         | **Formula / Calculation**           | **Example Result**              | **Provisioned (with Buffer)**             |
| ----------------------- | ---------------- | ----------------------------------- | ------------------------------- | ----------------------------------------- |
| **QPS / Compute**       | Peak QPS         | `Peak Requests ÷ Peak Window (sec)` | 3,000,000 ÷ 3600 = **833 QPS**  | **\~1080 QPS** (30% buffer)               |
|                         | Compute Planning | Based on QPS + P95 latency target   | Target < 200 ms per request     | Size CPU & memory for 1080 QPS            |
| **Storage**             | Data per Request | Request + Response + Logs           | **5 KB**                        | —                                         |
|                         | Daily Growth     | `Requests/day × Data/req`           | 10M × 5 KB = **50 GB/day**      | —                                         |
|                         | Total Storage    | `Daily × Retention Period`          | 50 GB × 30 days = **1.5 TB**    | **\~1.8 TB** (+20% growth/index overhead) |
| **Network / Bandwidth** | Avg Payload Size | Request + Response                  | **2 KB**                        | —                                         |
|                         | Throughput       | `QPS × Payload × 8`                 | 1080 × 2 KB × 8 = **17.3 Mbps** | **\~25 Mbps** (rounded up for spikes)     |

---

✅ **Benefits of This Table:**

* Makes capacity planning easy to review at a glance.
* Shows both **raw numbers** and **buffered provisioning** (realistic planning).
* Can be updated quickly as traffic patterns change.



### Notes

the **“× 8”** in point 4 is there because we need to convert **bytes to bits** when calculating **network throughput**.

Here’s why:

* **Payload size** is usually measured in **bytes** (e.g., 2 KB per request).
* **Network throughput** is measured in **bits per second (bps)** — Mbps, Gbps, etc.
* **1 byte = 8 bits**, so we multiply by 8 to get the equivalent size in bits before converting to Mbps or Gbps.

---

### Example (Step-by-Step)

**Given:**

* QPS = 1,080
* Avg payload = 2 KB/request

**Calculation:**

1. **Convert KB to bytes:**

   $$
   2 \text{ KB} = 2 × 1024 = 2048 \text{ bytes}
   $$

2. **Convert to bits:**

   $$
   2048 × 8 = 16{,}384 \text{ bits/request}
   $$

3. **Calculate bits per second:**

   $$
   1080 × 16{,}384 = 17{,}694{,}720 \text{ bps} ≈ 17.7 \text{ Mbps}
   $$

---

### Shortcut Version

So the formula becomes:

$$
\text{Throughput (bps)} = \text{QPS} × \text{Payload Size (bytes)} × 8
$$

And then you just convert bps → Kbps/Mbps/Gbps.

---
