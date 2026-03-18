Here’s a **simple, practical explanation** of **FIPS** and **STIG** compliance — what they are, why they matter, and what you actually have to do.

---

## 🔐 FIPS Compliance (Federal Information Processing Standards)

### 👉 What it is (in plain English)

**FIPS = Approved encryption rules from the U.S. government**

It ensures your system uses **strong, government-approved cryptography** to protect data.

---

### 🧠 Why it exists

To make sure sensitive data (government, healthcare, finance, enterprise) is protected using trusted algorithms — not weak or outdated ones.

---

### 🔑 Key Standard: FIPS 140-2 / 140-3

Focuses on **cryptographic modules** (OpenSSL, Java crypto, TPMs, etc.)

---

### ✅ What FIPS mode enforces

When a system is in FIPS mode:

* Only approved encryption algorithms allowed
* Weak algorithms disabled
* Secure random number generation required
* Crypto libraries must be validated

---

### ❌ Examples of blocked algorithms

* MD5
* SHA-1 (for signatures)
* DES / 3DES (often restricted)
* RC4
* Weak TLS versions (TLS 1.0/1.1)

---

### ✅ Allowed examples

* AES
* SHA-256 / SHA-384
* RSA (strong key sizes)
* ECC (NIST curves)
* TLS 1.2 / 1.3

---

### 🖥️ Typical Linux actions for FIPS

Enable FIPS mode:

```
fips-mode-setup --enable
reboot
```

Verify:

```
cat /proc/sys/crypto/fips_enabled
```

---

### 🏢 Who requires FIPS

* U.S. Federal agencies
* Defense contractors
* Healthcare (sometimes)
* Financial institutions
* Enterprise products sold to government

---

## 🛡️ STIG Compliance (Security Technical Implementation Guide)

### 👉 What it is (in plain English)

**STIG = Detailed security hardening checklist for systems**

Created by DISA (Defense Information Systems Agency).

Think of it as:

👉 “Lock down everything so attackers have almost no way in.”

---

### 🧠 Why it exists

To protect military and government systems from cyber attacks by enforcing strict security configuration.

---

### 🔎 What STIG covers

Almost everything in the OS:

* Password policies
* User accounts
* SSH configuration
* File permissions
* Logging
* Services
* Network settings
* Auditing
* Firewall rules
* Kernel security

---

### 📋 Example STIG requirements

#### 🔑 Password rules

* Minimum length (e.g., 15 characters)
* Complexity required
* Password expiration
* Account lockout after failures

---

#### 👤 User controls

* Disable root SSH login
* Remove unused accounts
* Enforce least privilege

---

#### 🔐 SSH hardening

* Disable weak ciphers
* Disable password auth (use keys)
* Idle session timeout

---

#### 🧱 Service restrictions

* Disable unused services
* Only required ports open
* No anonymous access

---

#### 📊 Logging & auditing

* Audit all privileged actions
* Log login attempts
* Central log collection recommended

---

### 🖥️ Tools often used for STIG compliance

* OpenSCAP
* SCAP Security Guide
* Ansible hardening roles
* DISA STIG Viewer

Example scan:

```
oscap xccdf eval --profile stig ...
```

---

## ⚖️ FIPS vs STIG — Quick Comparison

| Feature        | FIPS                  | STIG                      |
| -------------- | --------------------- | ------------------------- |
| Security focus | Encryption            | Full system hardening     |
| Scope          | Crypto only           | Entire OS & configuration |
| Maintained by  | NIST                  | DISA (DoD)                |
| Goal           | Strong encryption     | Attack surface reduction  |
| Mode/Checklist | Mode-based            | Detailed checklist        |
| Applies to     | Libraries & protocols | OS, apps, network         |

---

## 🧩 How they work together

Many government systems require **both**:

👉 FIPS = Secure encryption
👉 STIG = Secure configuration

Together:

✔ Protect data
✔ Prevent intrusion
✔ Meet compliance requirements

---

## 🏢 Real-world example (Linux server)

To be compliant, a system might need:

**FIPS**

* FIPS-validated OpenSSL
* TLS 1.2+
* AES encryption only

**STIG**

* SSH hardened
* Root login disabled
* Firewall enabled
* Audit logging active
* Strong passwords enforced
* Unnecessary services removed

---

## 🧠 One-sentence summary

👉 **FIPS = Use approved encryption**
👉 **STIG = Lock down the entire system**

---

```
[root@us1aph0081 /]# update-crypto-policies --show
FIPS:VXOS
[root@us1aph0081 /]# 
[root@us1aph0081 /]# cat /proc/sys/crypto/fips_enabled
1
[root@us1aph0081 /]# 

```
