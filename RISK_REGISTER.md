## 📋 RISK_REGISTER.md

This document serves as the formal risk register for the **DSRP-Generic-Web-Template**. It details potential security and operational risks associated with various HTML elements and architectural decisions, categorized by the **CIA Triad** (Confidentiality, Integrity, and Availability). It also tracks the control or mitigation strategy applied.

| Step/Element | CIA Pillar | Risk | Mitigation/Control | Status |
| :--- | :--- | :--- | :--- | :--- |
| **Global Scope** | Confidentiality | Information Disclosure (Source): Leakage of sensitive configuration data (keys, tokens). | Control: Implement a .gitignore file. Use Environment Variables/Kubernetes Secrets. | Deferred to CI/CD/K8s |
| **Global Scope** | Integrity | Code Tampering: Unauthorized modification of source code in the repository. | Control: Enforce GitHub Branch Protection Rules. Use cryptographic checksums during CI/CD. | Deferred to CI/CD/GitHub |
| **Global Scope** | Availability | Denial of Service (DoS): Overloading the deployment or distribution channel. | Control: Utilize Cloudflare's DDoS protection. Implement K8s Resource Quotas/LimitRanges. | Deferred to Cloudflare/K8s |
| **Authentication** | Confid./Integrity | Unauthorized Access: User bypasses SSO login. | Control: Implement Cloudflare Access / Zero Trust at the edge network to intercept all requests. | Deferred to Cloudflare |
| **<html>** | Integrity | Use of Deprecated/Vulnerable Features (e.g., manifest). | Control: Strict Omission of deprecated HTML5 attributes/tags. | Mitigated (Template Omission) |
| **<html>** | Availability | Multilingual Availability/SEO: Wrong language served or duplicate content penalty. | Control: Implement Reciprocal hreflang Tags and use x-default. | Mitigated (Template Inclusion) |
| **<meta charset="UTF-8">** | Integrity | Character Encoding Attack: Attacker uses non-standard character representations to bypass WAFs. | Control: Mandate UTF-8 in the meta tag and the server's Content-Type header. | Mitigated (Template & Header) |
| **<meta name="viewport">** | Integrity/Avail. | Inconsistent Rendering/Accessibility Failure: Broken layout or disabled user zooming. | Control: Mandatory Inclusion of width=device-width, initial-scale=1.0. Avoid user-scalable=no. | Mitigated (Template Inclusion) |
| **<meta http-equiv="CSP">** | Integrity/Confid. | Cross-Site Scripting (XSS): Malicious scripts are injected. Data Exfiltration. | Control: Strict default-src 'self' policy and restricted connections (connect-src 'self'). | Mitigated (Template Inclusion) |
| **<meta http-equiv="X-Frame-Options">** | Confidentiality | Clickjacking (Framing Attacks): Attacker frames your page to trick users. | Control: X-Frame-Options: DENY (or SAMEORIGIN) header/meta tag. | Mitigated (Template Inclusion) |
| **<meta name="referrer">** | Confidentiality | Referrer Leakage: Sensitive URL paths leaked to external sites. | Control: Referrer-Policy: strict-origin-when-cross-origin meta tag. | Mitigated (Template Inclusion) |
| **<form> Tag** | Integrity | Cross-Site Request Forgery (CSRF): Attacker forces unintended request submission. | Control: Server-Side CSRF Token generation and validation. | Deferred to Backend |
| **<form> Tag** | Confidentiality | Insecure Data Transmission: Data submitted over HTTP. | Control: Mandate HTTPS/HSTS header enforcement. | Deferred to Cloudflare/K8s |
| **<input> Tag** | Integrity | XSS/SQL Injection: Malicious code submitted via user input. | Control: Strict Input Validation/Sanitization on the server-side. | Deferred to Backend |
| **<input> Tag** | Confidentiality | Autofill Credential Leakage: Browser autofills sensitive data. | Control: Specific autocomplete Attribute usage (e.g., new-password). | Mitigated (Template Attribute) |
| **SRI Strategy** | Integrity | Supply Chain Attack: Attacker compromises third-party CDN code. | Control: Subresource Integrity (SRI) with cryptographic hashing. | Mitigated (Template Attribute) |
| **SRI Strategy** | Availability | Vendor Update Failure: Valid vendor update breaks the SRI hash and application availability. | Control: Automated Build Hashing (Option 1) and Local Fallback (Option 3) for fault tolerance. | Mitigated (Template Logic & CI/CD) |
| **SRI Strategy** | Integrity | Fallback Bypass: Local fallback file is compromised, bypassing the primary SRI check. | Control: Tight Repository Security and strict access control for the local fallback file. | Deferred to GitHub/CI/CD |
| **Overall** | Integrity | Unsanitized Dynamic Content (Highest XSS Risk): Server-side renders un-trusted data into HTML. | Control: Output Encoding: Ensure all dynamic data is context-aware encoded before rendering. | Deferred to Backend (Highest Priority) |
