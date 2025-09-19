On a Linux host, Docker relies on **kernel features like cgroups (control groups) and namespaces** to isolate and control resources for containers. The host can enforce limits on **CPU, memory, block I/O, and PIDs** so that no single container can monopolize system resources.

Here’s how the host can control CPU and memory for Docker containers:

---

## 1. **Memory Control**

Docker uses **cgroups memory controller** to enforce memory limits.

* **Set hard memory limit** (container cannot use more than this):

  ```bash
  docker run -m 512m nginx
  ```

  → Limits memory usage to **512 MB**. If the container tries to use more, it may be **killed (OOM)**.

* **Set swap + memory limit**:

  ```bash
  docker run -m 512m --memory-swap=1g nginx
  ```

  → Container can use **512 MB RAM + 512 MB swap**.

* **Disable swap usage**:

  ```bash
  docker run -m 512m --memory-swap=512m nginx
  ```

  → Forces container to only use RAM (no swap).

* **Soft limit (reservation)**:

  ```bash
  docker run --memory-reservation=256m nginx
  ```

  → Hints the kernel to prioritize reclaim when memory goes below 256 MB, but not enforced strictly.

---

## 2. **CPU Control**

Docker uses **cgroups CPU controller** to share or restrict CPU usage.

* **CPU shares (relative weight)**:

  ```bash
  docker run --cpu-shares=512 nginx
  ```

  → Default is 1024. If two containers (512 and 1024) run on same CPU, the second gets \~2x more CPU.

* **Limit CPU cores**:

  ```bash
  docker run --cpuset-cpus="0,2" nginx
  ```

  → Restricts container to run only on CPU 0 and 2.

* **Absolute CPU quota**:

  ```bash
  docker run --cpus="1.5" nginx
  ```

  → Container can use up to **1.5 CPU cores**.

* **Advanced (quota + period)**:

  ```bash
  docker run --cpu-quota=50000 --cpu-period=100000 nginx
  ```

  → Restricts container to **50% of one CPU**.

---

## 3. **Other Resource Controls**

* **PIDs limit**:

  ```bash
  docker run --pids-limit=100 nginx
  ```

  → Prevents fork bombs inside containers.

* **Block I/O (disk IOPS/throughput)**:

  ```bash
  docker run --device-read-bps /dev/sda:1mb nginx
  ```

  → Limit read speed to **1 MB/s** on `/dev/sda`.

---

✅ **Summary**:

* **Memory**: `-m`, `--memory-reservation`, `--memory-swap`
* **CPU**: `--cpus`, `--cpu-shares`, `--cpuset-cpus`
* **Other**: `--pids-limit`, `--device-read-bps`

All of these use Linux **cgroups** underneath. That’s how the host kernel enforces control over container resource usage.

---

Do you want me to also explain how these **limits are reflected in `docker stats`** so you can monitor them in real time?

Perfect 👍 let’s connect the **limits you set** with what you see in `docker stats`.

---

## 🔎 Monitoring with `docker stats`

Run:

```bash
docker stats
```

You’ll see live usage for running containers, like:

```
CONTAINER ID   NAME       CPU %     MEM USAGE / LIMIT   MEM %     NET I/O        BLOCK I/O      PIDS
3f1e12ab34cd   nginx1     25.37%    128MiB / 512MiB     25.00%    1.2kB / 900B   5.2MB / 1.1MB  23
```

### Breakdown:

* **CPU %**

  * Shows percentage of host CPU the container is consuming.
  * If you used `--cpus="1.5"` → stats will cap relative to 1.5 cores.
  * Example: On a 4-core system, 100% in `docker stats` = 1 core. So if a container is capped to 1.5 CPUs, it can max out at **150%**.

* **MEM USAGE / LIMIT**

  * Shows **current memory usage** and **configured max (`-m` or `--memory`)**.
  * If you set `-m 512m`, it will display something like `128MiB / 512MiB`.
  * **MEM %** is usage relative to that limit.

* **PIDS**

  * Number of processes running inside the container.
  * If you used `--pids-limit`, you’ll see this count climb but never exceed the cap.

* **BLOCK I/O**

  * Bytes read/written to block devices.
  * If you set I/O throttling (`--device-read-bps`, `--device-write-bps`), this will show activity but the speed won’t exceed limits.

---

## 🛠 Example

Let’s say you start a container with limits:

```bash
docker run -d --name myapp \
  -m 256m --memory-swap=512m \
  --cpus="0.5" \
  --pids-limit=50 \
  nginx
```

Then run:

```bash
docker stats myapp
```

You might see:

```
CONTAINER ID   NAME    CPU %   MEM USAGE / LIMIT   MEM %   NET I/O   BLOCK I/O   PIDS
a1b2c3d4e5f6   myapp   48.20%  120MiB / 256MiB     46.8%   500B/300B 2.1MB/1.0MB  12
```

Here:

* CPU never goes above **50% of 1 CPU**.
* Memory cannot exceed **256 MB RAM + 256 MB swap**.
* PIDs won’t exceed **50**.

---

👉 So, `docker stats` is your real-time dashboard to check if the **cgroup limits** you set are actually being enforced.

