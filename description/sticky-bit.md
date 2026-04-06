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

I can also show a quick demo of what happens **with vs without sticky bit**—that usually makes it crystal clear.

Great—let’s walk through a simple **hands-on demo** to make the sticky bit crystal clear.

---

# 🧪 Demo: Without Sticky Bit (Problem Scenario)

### Step 1: Create a shared directory

```bash
mkdir /shared_dir
chmod 777 /shared_dir
```

👉 Now everyone can read/write/execute.

---

### Step 2: Two users create files

User1:

```bash
touch /shared_dir/user1_file
```

User2:

```bash
touch /shared_dir/user2_file
```

---

### Step 3: User2 deletes User1’s file ❌

```bash
rm /shared_dir/user1_file
```

👉 This **works**, which is dangerous
Because directory is writable by everyone.

---

# 🔐 Demo: With Sticky Bit (Protected Scenario)

### Step 4: Enable sticky bit

```bash
chmod +t /shared_dir
```

Check:

```bash
ls -ld /shared_dir
```

Output:

```
drwxrwxrwt ...
```

👉 Notice the **`t`**

---

### Step 5: Try deleting again

User2 tries:

```bash
rm /shared_dir/user1_file
```

👉 Now you get:

```
rm: cannot remove 'user1_file': Operation not permitted
```

✅ **Protected!**

---

# 📌 Real-world Example

### 🔹 /tmp directory

Check it:

```bash
ls -ld /tmp
```

Output:

```
drwxrwxrwt ...
```

👉 Why?

* Everyone writes temp files here
* Sticky bit prevents users from deleting others’ files

---

# 🧠 Key Takeaway

| Scenario           | Can delete others' files? |
| ------------------ | ------------------------- |
| Without sticky bit | ✅ Yes (unsafe)            |
| With sticky bit    | ❌ No (safe)               |

---

# ⚡ Quick Tip

Sticky bit is **only meaningful on directories**, not regular files.

---

If you want, I can also show how sticky bit compares with **SUID and SGID**—that’s a common interview question.

