<h1 align="center">🔐 OTP Verification Bypass via HTTP Method Manipulation</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Severity-Medium-orange?style=for-the-badge" alt="Severity">
  <img src="https://img.shields.io/badge/Status-Triggered-brightgreen?style=for-the-badge" alt="Status">
  <img src="https://img.shields.io/badge/Type-Broken%20Access%20Control-blue?style=for-the-badge" alt="Type">
  <img src="https://img.shields.io/badge/Appreciation%20Letter-Received-gold?style=for-the-badge" alt="Appreciation">
</p>

---

## 📋 Report Information

| | |
|---|---|
| 👤 **Reporter** | Abishek Khatri |
| 🏢 **Program** | wscube****.com |
| ⚠️ **Severity** | Medium |
| 🏷️ **Status** | Triggered (Appreciation letter received) |
| 🧩 **Affected Component** | Contact and lead generation forms (OTP verification flow) |

---

## 📝 Summary

A contact form on a web application required OTP verification during its
normal workflow. However, the server-side check was not applied consistently.
By changing the HTTP method of the submission request, the OTP step could be
skipped and the form was still accepted.

---

## 🎯 Affected Component

Contact and lead generation forms (OTP verification flow)

---

## 🔁 Steps to Reproduce

<details open>
<summary><b>Click to collapse / expand</b></summary>

1. Open a contact form and fill in all required fields.
2. Click Submit and observe the normal OTP flow.
3. Intercept the form submission request using Burp Suite.
4. Change the HTTP method to a different method.
5. Forward the request.
6. Observe that the submission is accepted without completing OTP verification.

</details>

---

## ✅ Expected vs ❌ Actual

| ✅ Expected Result | ❌ Actual Result |
|-------------------|-----------------|
| OTP verification should be enforced on the server side, regardless of the HTTP method used. | The request was processed successfully without any OTP validation. |

---

## 💥 Impact

- 🚫 Bypass of the intended verification control
- 📨 Submission of unverified leads
- 📈 Increased spam and abuse risk
- 🤖 Bulk automated form submissions

---

## 🔧 Remediation

1. Enforce OTP verification fully on the server side.
2. Apply the same validation logic to all endpoints and HTTP methods.
3. Reject unsupported HTTP methods with `405 Method Not Allowed`.
4. Log and monitor abnormal submission patterns.

---

## 🏆 Acknowledgement

The company acknowledged this finding and issued an appreciation letter.

📄 [View appreciation letter](appreciation-letter.pdf)

---

<p align="center">
  <sub>Testing was done legally and within the program scope.</sub>
</p>
