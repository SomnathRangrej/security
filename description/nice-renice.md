## What is nice and renice

In Linux, **`nice`** and **`renice`** are used to control the **priority of processes**—i.e., how much CPU time a process gets compared to others.

---

## 🔹 What is `nice`?

`nice` is used to **start a new process with a specific priority**.

### 👉 Key idea:

* It sets the **niceness value** when launching a command.
* Range: **-20 to +19**

  * **-20 → highest priority (least nice)**
  * **+19 → lowest priority (most nice)**

### 📌 Example:

```bash
nice -n 10 tar -czf backup.tar.gz /data
```

➡️ Starts the `tar` process with **lower priority** so it doesn’t heavily impact the system.

### 💡 Default behavior:

* If you just run `nice command`, it uses **niceness = 10** (lower priority than normal).

---

## 🔹 What is `renice`?

`renice` is used to **change the priority of an already running process**.

### 📌 Example:

```bash
renice -n 5 -p 1234
```

➡️ Changes the niceness of process **PID 1234** to **5**.

You can also renice:

* By user:

  ```bash
  renice -n 10 -u username
  ```
* By process group:

  ```bash
  renice -n 0 -g 5678
  ```

---

## 🔑 Key Differences

| Feature   | nice                  | renice                   |
| --------- | --------------------- | ------------------------ |
| When used | Before process starts | After process is running |
| Purpose   | Set initial priority  | Modify existing priority |
| Target    | New command           | PID / user / group       |

---

## ⚠️ Important Notes

* Only **root** can assign **negative niceness values** (higher priority).
* Regular users can:

  * Increase niceness (lower priority)
  * Cannot decrease niceness (increase priority)

---

## 🧠 Simple Analogy

Think of CPU like a queue:

* `nice` → “I’m starting a task, let others go first.”
* `renice` → “Change my position in the queue while I’m already waiting.”

---

If you want, I can show you **real-time examples using `top` or `ps`** to see nice values in action 👍

## what PR value means in top command output?

In the `top` command, **`PR` (Priority)** represents the **actual scheduling priority** the Linux kernel assigns to a process—this is what the CPU scheduler uses to decide which process runs next.

---

Great—let’s see how **`nice` and `renice` look in real time** using common Linux tools like `top` and `ps`.

---

## 🔹 1. View nice values using `top`

Run:

```bash
top
```

### 👉 What to look for:

* Column **`NI`** = *Nice value*
* Column **`PR`** = *Priority*

### 📌 Example output (simplified):

```
PID   USER   PR  NI   VIRT   RES   SHR  S  %CPU  COMMAND
1234  root   20   0  500m   50m   10m  R   40.0  java
5678  user   30  10  200m   20m    5m  S   5.0   tar
```

### 💡 Interpretation:

* `NI = 0` → default priority
* `NI = 10` → lower priority (more “nice”)
* Higher NI ⇒ less CPU preference

---

## 🔹 2. Start a process with `nice`

Run a CPU-heavy task:

```bash
nice -n 10 yes > /dev/null &
```

Now check in `top`:

* You’ll see the `yes` process with **NI = 10**
* It will consume CPU but with **lower priority**

---

## 🔹 3. Change priority using `renice`

Find the PID:

```bash
ps -ef | grep yes
```

Then change priority:

```bash
renice -n 5 -p <PID>
```

### 👉 Verify in `top`:

* NI changes from **10 → 5**
* Process gets **higher priority than before**

---

## 🔹 4. View nice values using `ps`

Run:

```bash
ps -eo pid,ni,pri,cmd | grep yes
```

### 📌 Example output:

```
1234  5  25 yes
```

| Column | Meaning         |
| ------ | --------------- |
| PID    | Process ID      |
| NI     | Nice value      |
| PRI    | Kernel priority |

---

## 🔹 5. Real-world use case

Imagine:

* You’re running a **backup job** (`tar`, `rsync`)
* You don’t want it to slow down production apps

👉 Start it with low priority:

```bash
nice -n 15 rsync -av /data /backup
```

👉 If it’s already running and causing load:

```bash
renice -n 15 -p <PID>
```

---

## ⚡ Pro Tip

In `top`, you can:

* Press **`r`** → enter PID → change nice value interactively

---

## 🧠 Summary

* Use **`nice`** → when starting a process
* Use **`renice`** → to adjust a running process
* Monitor using **`top`** or **`ps`**

---

If you want, I can give you a **quick lab exercise (5 mins hands-on)** to practice this on your system.

## 🔹 What exactly is `PR`?

* **`PR` = Priority value used by the kernel scheduler**
* It is **derived from the nice value (NI)** and other factors (like scheduling policy)
* **Lower PR number ⇒ higher priority ⇒ more CPU time**

---

## 🔹 Relationship between `PR` and `NI`

For normal processes (non–real-time):

```text
PR = 20 + NI
```

### 📌 Examples:

| NI (Nice) | PR (Priority) | Meaning         |
| --------- | ------------- | --------------- |
| -5        | 15            | Higher priority |
| 0         | 20            | Default         |
| 10        | 30            | Lower priority  |

---

## 🔹 What you see in `top`

Example:

```bash
PID   USER   PR  NI  %CPU  COMMAND
1234  root   20   0  40.0  java
5678  user   30  10   5.0  tar
```

### 💡 Interpretation:

* `java` → PR=20 → normal priority
* `tar` → PR=30 → lower priority (less CPU preference)

---

## 🔹 Special case: Real-time processes

Sometimes you’ll see:

* **`PR` < 20** or even very small numbers
* Or **`RT`** in `top`

👉 That means:

* Process is using **real-time scheduling (SCHED_FIFO / SCHED_RR)**
* These processes have **very high priority** and can preempt normal ones

---

## 🔑 Key takeaways

* **PR is what actually matters to the CPU scheduler**
* **NI influences PR**, but PR is the final value used
* **Lower PR = higher priority**
* Real-time processes can override normal PR/NI behavior

---

## 🧠 Simple way to remember

* **NI (Nice)** → “user preference”
* **PR (Priority)** → “kernel decision”

---

If you want, I can also explain **how Linux scheduler (CFS) actually uses PR internally**, which is often asked in interviews.
