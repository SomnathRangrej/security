## What is sticky bit and where is it used

The **sticky bit** is a special permission in Linux/Unix that **controls deletion of files within a directory**, not access to the files themselves.

---

## 🔐 What does the sticky bit do?

When the sticky bit is set on a directory:

👉 **Only these users can delete or rename files inside it:**

* The file owner
* The directory owner
* The root user

Even if others have write permissions on the directory, they **cannot delete files they don’t own**.

---

## 📁 Where is it used?

The most common and important use is on **shared directories**, especially:

### 🔹 /tmp directory

* World-writable (everyone can create files)
* Sticky bit ensures users **cannot delete each other's files**

Example:

```bash
drwxrwxrwt  10 root root 4096 Apr  3  /tmp
```

👉 Notice the **`t` at the end** — that indicates the sticky bit is set.

---

## ⚙️ How to set and remove sticky bit

### Set sticky bit:

```bash
chmod +t directory_name
```

### Remove sticky bit:

```bash
chmod -t directory_name
```

---

## 🔎 How to identify it

### In `ls -l` output:

* `t` → sticky bit + execute permission
* `T` → sticky bit but **no execute permission**

Examples:

```bash
drwxrwxrwt   # correct usage (common)
drwxrwxrwT   # uncommon, no execute
```

---

## 💡 Why is it important?

Without sticky bit:

* Any user could delete anyone else's files in a shared writable directory ❌

With sticky bit:

* Protects user files in shared environments ✅

---

## 🧠 Quick summary

* Sticky bit = **deletion protection in shared directories**
* Most commonly used on **/tmp**
* Represented as **`t`** in permissions
* Set using `chmod +t`

---

If you want, I can also show a quick demo of what happens **with vs without sticky bit**—that usually makes it crystal clear.
