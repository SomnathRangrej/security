Here’s a structured overview of **Data Privacy & Retention (Encryption, GDPR, CCPA):**

---

## 🔐 Data Privacy

Data privacy is about ensuring personal or sensitive information is collected, stored, processed, and shared in a secure and compliant way.
Key aspects:

* **Encryption:**

  * **At rest** → Data stored in databases, files, or backups must be encrypted (e.g., AES-256).
  * **In transit** → Data traveling over networks must be encrypted (e.g., TLS 1.2+).
  * **Key Management** → Use secure key vaults (AWS KMS, HashiCorp Vault). Rotate keys regularly.
* **Access Control:**

  * Role-based (RBAC) or attribute-based (ABAC) access.
  * Principle of least privilege (PoLP).
* **Data Minimization:**

  * Only collect what’s necessary.
  * Avoid retaining unnecessary PII (Personally Identifiable Information).

---

## 📅 Data Retention

* **Policies:** Define how long data should be kept based on business, legal, and compliance needs.
* **Data Lifecycle Management:**

  1. **Collection** → Capture only necessary data.
  2. **Storage** → Securely store with encryption.
  3. **Access/Usage** → Audit logs for usage and access.
  4. **Retention Period** → Define retention timelines.
  5. **Deletion/Anonymization** → Securely erase or anonymize after retention expires.
* **Techniques:**

  * Automated retention policies in DBs/storage (e.g., TTL in MongoDB, S3 lifecycle rules).
  * Pseudonymization / anonymization for analytics while protecting identities.

---

## ⚖️ GDPR (General Data Protection Regulation – EU)

* Applies to processing personal data of **EU residents**.
* Key rights:

  * Right to **access**, **rectify**, and **erase** ("Right to be forgotten").
  * Right to **data portability** (transfer data to another provider).
  * Right to **restrict processing**.
  * **Consent** must be explicit and easy to withdraw.
* Requirements:

  * Data Breach Notification within **72 hours**.
  * Maintain **Records of Processing Activities (RoPA)**.
  * Implement **Data Protection Impact Assessments (DPIA)** for high-risk processing.

---

## ⚖️ CCPA (California Consumer Privacy Act – US)

* Applies to businesses handling personal data of **California residents**.
* Key rights:

  * Right to **know** what personal data is collected and how it’s used.
  * Right to **opt-out** of sale of personal data.
  * Right to **delete** personal data.
  * Right to **non-discrimination** for exercising rights.
* Enforcement & penalties by the **California Privacy Protection Agency (CPPA)**.

---

## ✅ Best Practices

* Encrypt **all sensitive data** (AES, TLS).
* Apply **data classification** → tag data as confidential, restricted, public.
* Implement **Data Loss Prevention (DLP)** solutions.
* Automate **data retention and deletion policies**.
* Train employees on **privacy awareness**.
* Regularly perform **privacy impact assessments**.

---

