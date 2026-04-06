## What is fsck and when should it be used

### What is `fsck`?

`fsck` stands for **File System Consistency Check**. It’s a Linux/Unix utility used to **scan and repair filesystem errors**.

Think of it like a “disk doctor” 🩺:

* It checks the integrity of a filesystem (ext4, xfs*, etc.)
* Detects issues like:

  * Corrupted metadata
  * Lost inodes
  * Broken links
  * Orphaned files
* Attempts to **fix inconsistencies automatically or interactively**

> Common command:

```bash
fsck /dev/sda1
```

---

### How `fsck` Works (Simple View)

![Image](https://longhorn.io/img/kb/analysis-filesystem-corrupted-issues-due-to-error-on-cow-while-rebuilding-replicas/data-corruption-illustration.png)

![Image](https://web.cs.ucla.edu/classes/winter15/cs111/scribe/12a/inode-block.png)

![Image](https://oneuptime.com/blog/post/2026-01-24-filesystem-checks-fsck/social-media.png)

![Image](https://i.sstatic.net/WPZ6b.png)

* Scans filesystem structures (superblock, inodes, blocks)
* Verifies relationships between them
* Repairs mismatches (e.g., reconnects orphan files to `lost+found`)

---

### When Should You Use `fsck`?

#### ✅ 1. After Improper Shutdown

* System crash / power failure
* Filesystem may be left in inconsistent state

#### ✅ 2. Boot Errors

* Messages like:

  * `UNEXPECTED INCONSISTENCY`
  * `fsck failed`
* System may drop into **emergency mode**

#### ✅ 3. Disk/File Corruption Symptoms

* Missing files/directories
* I/O errors
* Strange behavior while accessing files

#### ✅ 4. Manual Maintenance (rare)

* When you suspect silent corruption
* During troubleshooting storage issues

---

### ⚠️ Important Rules (Very Critical)

#### 🚫 Never run `fsck` on a mounted filesystem

* Can cause **severe data corruption**
* Always unmount first:

```bash
umount /dev/sda1
fsck /dev/sda1
```

#### ✅ For root filesystem

* Run from:

  * Rescue mode
  * Live CD/ISO
  * Or let system run fsck at boot

---

### Useful Variants

* Auto-fix without prompts:

```bash
fsck -y /dev/sda1
```

* Check without fixing:

```bash
fsck -n /dev/sda1
```

* Force check:

```bash
fsck -f /dev/sda1
```

---

### Bonus: Auto fsck at Boot

Linux may automatically run fsck:

* After a certain number of mounts
* Or after a time interval
* Controlled via:

```bash
tune2fs -l /dev/sda1
```

---

### Summary

* `fsck` = filesystem repair tool
* Used when corruption is suspected or detected
* **Never run on mounted filesystem**
* Essential for system recovery scenarios

---

I can also walk you through a **real fsck failure scenario and how to recover step-by-step** (very useful for interviews or production debugging).

Great—let’s walk through a **real-world `fsck` failure + recovery scenario** step by step. This is exactly the kind of situation you’ll hit in production or interviews.

---

# 🚨 Scenario: System Won’t Boot (Filesystem Corruption)

### What you see during boot:

```
UNEXPECTED INCONSISTENCY; RUN fsck MANUALLY.
(i.e., without -a or -p options)
```

System drops into:

```
Give root password for maintenance
(or press Control-D to continue)
```

---

## 🔍 What Happened?

![Image](https://i.sstatic.net/N5Ylv.jpg)

![Image](https://wehco.media.clients.ellingtoncms.com/img/photos/2026/03/23/260324FPbensonjpg_t600.jpg?4326734cdb8e39baa3579048ef63ad7b451e7676=)

![Image](https://i.sstatic.net/wQrcK.jpg)

![Image](https://i.snipboard.io/z6BZTp.jpg)

* Likely causes:

  * Sudden power failure ⚡
  * Disk issue 💽
  * Improper shutdown
* Filesystem metadata is inconsistent → system refuses to mount it safely

---

# 🛠️ Step-by-Step Recovery

## 1️⃣ Enter Maintenance Mode

* Provide **root password**
* You’ll get a shell like:

```bash id="k1"
sh-4.2#
```

---

## 2️⃣ Identify the Affected Filesystem

Check error message:

```
/dev/sda1: UNEXPECTED INCONSISTENCY
```

Or verify:

```bash id="k2"
lsblk
blkid
```

---

## 3️⃣ Ensure Filesystem is NOT Mounted

Very important ⚠️

Check:

```bash id="k3"
mount | grep sda1
```

If mounted → remount as read-only or unmount:

```bash id="k4"
umount /dev/sda1
```

---

## 4️⃣ Run fsck Manually

```bash id="k5"
fsck /dev/sda1
```

You’ll see prompts like:

```
Inode 12345 has illegal block(s). Fix? yes
```

👉 Option:

* Type `y` manually (safe, controlled)
* OR auto-fix:

```bash id="k6"
fsck -y /dev/sda1
```

---

## 5️⃣ What fsck Fixes

During repair, it may:

* Reconnect orphan files → placed in:

  ```
  /lost+found
  ```
* Fix:

  * Inodes
  * Block counts
  * Directory structure

---

## 6️⃣ Reboot System

After successful repair:

```bash id="k7"
reboot
```

System should now boot normally ✅

---

# 🧪 Example Output (Simplified)

```
Pass 1: Checking inodes
Pass 2: Checking directory structure
Pass 3: Checking connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary

/dev/sda1: ***** FILE SYSTEM WAS MODIFIED *****
```

👉 This means fsck **found and fixed issues**

---

# ⚠️ Real Production Tips (Very Important)

### ✅ 1. Always Try Read-Only First

```bash id="k8"
fsck -n /dev/sda1
```

* See damage before fixing

---

### ✅ 2. If Root Filesystem is Corrupt

* Use:

  * Rescue mode
  * Live Linux ISO
* Then run fsck on unmounted disk

---

### ✅ 3. Backup Critical Data (If Possible)

* If disk is failing → repair may worsen situation

---

### ✅ 4. Frequent fsck = Bigger Problem

* Could indicate:

  * Failing disk → check with:

```bash id="k9"
smartctl -a /dev/sda
```

---

# 🧠 Interview Insight (Short Answer)

👉 If asked:

**Q: What would you do if system fails to boot due to filesystem error?**

**Answer:**

* Boot into maintenance/emergency mode
* Identify affected partition
* Ensure it's unmounted
* Run `fsck` manually
* Fix errors and reboot
* Investigate root cause (disk, power failure, etc.)

---

If you want next level, I can show you:
👉 Difference between `fsck.ext4`, `xfs_repair` (important in RHEL systems)
👉 Or a **live debugging checklist used by SREs**
