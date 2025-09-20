Idempotency is a **concept in computing and system design** that describes an operation which, when performed multiple times, has the **same effect as performing it once**.

---

### 🔑 Key Idea

* **Idempotent Operation** → Running it once, twice, or a hundred times produces the same end result.
* The first call may cause a change, but subsequent identical calls **do not create side effects**.

---

### ✅ Examples

#### 1. **Everyday Life**

* **Light switch ON button (smart switch)**:
  Pressing the "ON" button once turns the light on.
  Pressing it again (when it’s already ON) has **no additional effect**—the light stays ON.

#### 2. **HTTP Methods**

* `GET /users/123` → Always returns the user info, no matter how many times you call it.
* `DELETE /users/123` → First call deletes the user. Repeating the same request won’t delete anything further—the result remains “user not found” (idempotent).
* `POST /orders` → Typically **not idempotent**, because calling it multiple times creates multiple orders.
* `PUT /users/123` → Idempotent, because replacing user data with the same payload multiple times yields the same state.

#### 3. **Banking**

* ✅ **Idempotent**: “Set balance to \$100” → same final state, no matter how many times executed.
* ❌ **Not Idempotent**: “Add \$100 to balance” → repeating it increases balance every time.

---

### ⚙️ Why It Matters

* **Reliability**: If a request is retried (e.g., due to network failure), the system won’t cause duplicate side effects.
* **APIs & Distributed Systems**: Ensures safe retries without inconsistencies.
* **Fault Tolerance**: Helps in building resilient systems.

---

👉 Do you want me to also show you **how to implement idempotency in APIs** (like using unique request IDs or tokens)?

Perfect 👍 Let’s walk through **how to implement idempotency in APIs** so retries don’t cause duplicate or inconsistent results.

---

## 🔧 Strategies to Implement Idempotency in APIs

### 1. **Idempotency Keys**

* The **client generates a unique identifier** (UUID, hash, or request ID) for each request.
* The server **stores the key and result** of the first successful request.
* If the same key is sent again, the server **returns the stored response instead of reprocessing**.

✅ Example: Stripe, PayPal, and many payment APIs use this.

```http
POST /payments
Idempotency-Key: abc123

{
  "amount": 100,
  "currency": "USD",
  "to": "user_567"
}
```

* First request → processes payment.
* Retry with same `Idempotency-Key` → server returns the same result, no duplicate charge.

---

### 2. **PUT vs POST Semantics**

* `PUT` is **naturally idempotent** (replace the resource).
* `POST` is **not idempotent** (create a new resource each time).
  👉 If your API operation is a **replace/update**, design it as `PUT` instead of `POST`.

Example:

```http
PUT /users/123
{
  "email": "test@example.com"
}
```

Calling multiple times → always results in the same user record.

---

### 3. **Safe Database Operations**

* Use **unique constraints** (e.g., unique transaction ID).
* Example: Inserting into `transactions` table with `transaction_id` as unique key ensures a retry won’t create duplicates.

SQL Example:

```sql
INSERT INTO payments (id, user_id, amount)
VALUES ('txn_123', 'user1', 100)
ON CONFLICT (id) DO NOTHING;
```

---

### 4. **Request Replay Detection**

* Track **recent requests** by (UserID, Operation, Timestamp).
* If a new request matches one recently processed, **ignore or return cached response**.

---

### 5. **Stateless + Deterministic Processing**

* If possible, design the operation itself to be **stateless and deterministic**:

  * “Set status = PAID” (idempotent) ✅
  * “Add \$10 to balance” (non-idempotent) ❌

---

## 🚦 Real-World Example: Payment API

* **Without Idempotency**: Network retry might charge the customer **twice**.
* **With Idempotency**: Retry returns the same **payment receipt**, no duplicate charge.

---

