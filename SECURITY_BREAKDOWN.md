# 🛡️ Template Security and DSRP Breakdown

This template is designed to provide a secure foundation for every project, incorporating both fundamental web security best practices (aligned with the **CIA Triad**) and structured thinking principles (**DSRP**).

---

## 1. Governance and Risk Management

All security decisions are documented and tracked. Security is a shared responsibility, not just a backend task.

* **Risk Register:** We used the **CIA Triad** (Confidentiality, Integrity, Availability) to analyze every element in the `index.html`.
    * **Mitigated:** Risks handled directly by the template (e.g., security headers).
    * **Deferred:** Risks requiring action in the backend code or infrastructure. **If a risk is Deferred, it is YOUR team's responsibility to implement the control!**
* **Documentation:** [View the complete RISK_REGISTER.md here.](./RISK_REGISTER.md)

---

## 2. Key Security Controls in the `<head>`

The following controls enforce **Distinctions (D)** between trusted and untrusted content, protecting the **Integrity (I)** of the page.

| Header/Meta Tag | CIA Focus | Security Goal | DSRP Connection |
| :--- | :--- | :--- | :--- |
| **Content Security Policy (CSP)** | I / C | Primary defense against **Cross-Site Scripting (XSS)**. | Defines the secure **Relationship (R)** with all external resources. |
| **X-Frame-Options: DENY** | C | Prevents **Clickjacking**. | Maintains the **Distinction (D)** (Identity) of the page by preventing external framing. |
| **Referrer-Policy** | C | Prevents sensitive data leakage to third parties. | Limits the amount of information shared during a **Relationship (R)** with an external system. |
| **X-Content-Type-Options: nosniff** | I | Prevents content confusion attacks. | Enforces the file's correct **Distinction (D)** (MIME type). |

---

## 3. Form Security and Integrity

The `<form>` is a critical **System Part (S)** that manages the user-to-server **Relationship (R)**. Controls here protect data **Integrity (I)** and **Confidentiality (C)**.

* **CSRF Token:** Placeholder for a token that validates the **Integrity** of the user-to-server **Relationship**. **(Deferred to Backend)**
* **Input Attributes (`pattern`, `minlength`):** Enforces **Distinctions** on valid input data, serving as the first line of defense for data **Integrity**.
* **`autocomplete="new-password"`:** This is a crucial **Confidentiality** control that prevents browsers from auto-filling credentials from other sites, securing the **Distinction** of your specific application's login.

---

## 4. Availability and Contingency

* **Subresource Integrity (SRI):** This cryptographic hash checks the **Integrity** of external scripts. If a CDN file is maliciously altered, the hash will fail, and the browser will not load it.
* **Local Fallback:** This demonstrates the **Perspective (P)** mindset: **"What if the primary system fails?"** If the SRI check fails, the local fallback ensures application **Availability (A)** is maintained.

---
