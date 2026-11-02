<h1 align="center">🛡️ Missing Input Length Validation</h1>
<p align="center"><b>Name Change Request → Client-Side Resource Exhaustion</b></p>

<p align="center">
  <img src="https://img.shields.io/badge/Severity-Low-yellow?style=for-the-badge" alt="Severity">
  <img src="https://img.shields.io/badge/Status-Triaged-blue?style=for-the-badge" alt="Status">
  <img src="https://img.shields.io/badge/Type-Input%20Validation-orange?style=for-the-badge" alt="Type">
  <img src="https://img.shields.io/badge/Platform-HackerOne-black?style=for-the-badge" alt="Platform">
</p>

---

## 📋 Report Information

| | |
|---|---|
| 👤 **Reporter** | Abishek Khatri |
| 🏢 **Program** | Private Program |
| 📅 **Date** | August 2026 |
| 🎯 **Affected Feature** | Account Settings → Name Verification → Request Name Change |
| 🔐 **Authentication** | Required (logged-in user) |
| 🧩 **Weakness** | Improper Input Validation (no maximum length) |

---

## 📝 Summary

The Name Change request feature accepts **extremely large values** in the name fields.
No effective maximum length is enforced at the application level. Very large inputs
caused **longer response times** and made the affected page **unresponsive**.

> 💡 This is an input validation issue. The problem comes from the **size of a single
> request**, not from request frequency, so it is not a rate-limiting issue.

---

## 📊 Observed Results

| Input Size | Result | Status |
|:----------:|--------|:------:|
| 3 characters | About **377 ms** response, normal loading | ✅ Normal |
| 50,000 characters | About **747 ms** response, reload sometimes took **10–15 s** | ⚠️ Slow |
| 100,000+ characters (101,021 used) | Page stuck loading, **"Page Unresponsive"** error | ❌ Degraded |
| 200,000 characters | Server returned **HTTP 413** | 🛑 Rejected |

Inputs below the 413 limit are still accepted and processed instead of being
rejected by application-level validation.

---

## 🔁 Steps to Reproduce

<details>
<summary><b>Click to expand</b></summary>

1. Log in with a valid account and open **Account Settings**.
2. Open the **Name Verification** section and click **Request Name Change**.
3. Submit a normal value (3 characters) in the First Name field and note the response time.
4. Repeat with about **50,000** characters.
5. Repeat with **100,000+** characters (101,021 used).
6. Go back to Account Settings and click **Request Name Change** again.
7. Observe the long loading state and the **"Page Unresponsive"** error.
8. For comparison, submit about **200,000** characters and observe **HTTP 413**.

</details>

---

## ✅ Expected vs ❌ Actual

| ✅ Expected | ❌ Actual |
|------------|----------|
| Server enforces a reasonable maximum length per name field | No effective limit below the request-size cap |
| Oversized input rejected immediately with a clear error | Oversized input accepted and processed |
| Large values never enter the name-change workflow | Large values slow down the page and make it unresponsive |

---

## 💥 Impact

An authenticated user can submit oversized values that take much longer to process
than normal input, which **degrades the Name Change feature for their own account**.
If backend resources are shared, repeated requests could cause wider resource consumption.

> ⚠️ **Limitations:** Other users and shared infrastructure were **not tested**, and
> there was no access to server logs or metrics. Cross-user or platform-wide impact
> was **not confirmed**. For this reason the severity is rated **Low**.

---

## 🧪 Testing Scope

- ✔️ Tested only on my **own authorized account**
- ✔️ No other users or accounts were targeted
- ✔️ No shared infrastructure was affected
- ✔️ Testing **stopped** once the page became unresponsive

---

## 🔧 Remediation

1. Enforce a **server-side maximum length** on all name fields (for example 50–100 characters).
2. **Reject oversized input immediately** with a validation error.
3. Apply the same limit on the **client side, API, and database** layers.
4. Lower the **request body size limit** for this endpoint.

---

## 📌 Timeline

| Step | Result |
|------|--------|
| 📨 Report submitted | Submitted via Report Assistant |
| 🔎 Triage review | Asked for clarification of impact |
| 💬 Researcher reply | Explained that impact was limited to own account |
| ✅ Final status | **Triaged** |

---

<p align="center">
  <sub>🔒 Company name, URLs, report ID and usernames have been removed for privacy.<br>
  Testing was done legally and within the program scope.</sub>
</p>
