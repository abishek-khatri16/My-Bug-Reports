<h1 align="center">📧 Missing Email Ownership Verification at Signup</h1>
<p align="center"><b>Account activated with an internal company email domain, without verification</b></p>

<p align="center">
  <img src="https://img.shields.io/badge/Severity-Low-yellow?style=for-the-badge" alt="Severity">
  <img src="https://img.shields.io/badge/Status-Triaged-blue?style=for-the-badge" alt="Status">
  <img src="https://img.shields.io/badge/Type-Improper%20Email%20Verification-orange?style=for-the-badge" alt="Type">
  <img src="https://img.shields.io/badge/Platform-HackerOne-black?style=for-the-badge" alt="Platform">
</p>

---

## 📋 Report Information

| | |
|---|---|
| 👤 **Reporter** | Abishek Khatri |
| 🏢 **Program** | VDP |
| 📅 **Date** | August 2026 |
| 🎯 **Affected Feature** | Public signup / account registration |
| 🔐 **Authentication** | Not required |
| 🧩 **Weakness** | Improper verification of email ownership |

---

## 📝 Summary

The application lets a user register and activate an account using an **email address
on the company's own internal domain**, without proving that they own that mailbox.
No verification email is sent, and the user lands on the dashboard immediately.

---

## 🔁 Steps to Reproduce

<details open>
<summary><b>Click to collapse / expand</b></summary>

1. Open the public signup page.
2. Fill in the form:
   - Username: any name
   - Email: an address on the company's **internal domain** (for example `employee@company.com`)
   - Password: any password
3. Click the signup button.
4. Observe that you are redirected straight to the dashboard.
5. Observe that no verification email is requested or sent.

</details>

---

## ✅ Expected vs ❌ Actual

| ✅ Expected | ❌ Actual |
|------------|----------|
| A verification email is sent to the address | No verification email is sent |
| The user must click the link before the account is activated | Account is activated immediately |
| Access is granted only after ownership is proven | User reaches the dashboard without proving ownership |

---

## 🧠 Root Cause

The application does not verify **ownership of the mailbox** before activating the account.
An outsider can therefore create an account that appears to belong to an internal employee.

---

## 💥 Potential Impact

An attacker could register accounts with the company's internal domain and appear
to be an employee. Depending on how the platform treats internal domains, this could allow:

- 🎭 Impersonation of employees or internal users
- 🗣️ Social engineering that relies on a trusted-looking email address
- 🔓 Access to employee-only features, if any are granted by email domain
- ⬆️ Privilege escalation, if internal accounts receive extra permissions based on domain

> ⚠️ These are **potential** impacts. I did not demonstrate access to any internal
> feature or data.

---

## 🏢 Program Response (Summary)

The security team reviewed the report and shared this reasoning:

| Point | Program's position |
|-------|--------------------|
| Account ownership | Tied to who controls the account later (password reset or SSO), not to who registered it |
| Real owner | Can always reclaim the account by password reset, and SSO login resets the password and revokes the attacker's session |
| Privileges | Self-registered accounts get **no** elevated permissions, whatever the email domain |
| Internal accounts | Provisioned by administrators, not through public signup |

They asked which elevated privileges or internal features I had actually accessed.
I shared a supporting document, and the report was then marked **Triaged** for internal review.

---

## 🧪 Honest Assessment

- ✔️ The missing email verification at signup was **confirmed in my testing**
- ❌ I did **not** find any elevated access or internal data through this account
- ✔️ Because of this, I rate the severity as **Low**
- ✔️ The program's reasoning above is a valid defense against persistent impersonation

---

## 🔧 Remediation

1. Send a **verification email** and require the user to confirm before activating the account.
2. Do not trust an email domain as proof of identity or internal status.
3. Block or flag signups that use the company's own domain unless they come through SSO.
4. Keep internal privileges tied to administrative provisioning, never to the signup form.

---

## 📌 Timeline

| Step | Result |
|------|--------|
| 📨 Report submitted | With a screen recording and a PDF report |
| 🔎 Triage review | Asked which privileges or data were actually accessed |
| 💬 Researcher reply | Shared a supporting document |
| ✅ Final status | **Triaged** (program said it would check on its end) |

---

<p align="center">
  <sub>🔒 Company name, URLs, report ID, usernames and attachments have been removed for privacy.<br>
  Testing was done legally and within the program scope.</sub>
</p>
