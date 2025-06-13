# Browser Threats and DNS Security

This section explains common security threats that occur within web browsers and DNS systems. These are often the target of attacks because they are essential for accessing and navigating the internet.

---

## Browser Threats

Web browsers are commonly exploited by attackers using:

### 1. Password Theft

- Attackers use browser extensions, phishing, or malware to extract stored passwords.
- Browsers often store credentials in plaintext or weakly protected databases.

**Example:**
Malicious JavaScript injected into a webpage can steal passwords from form fields.

---

### 2. Cookie Theft

Cookies store session data (like login state). If stolen, they allow attackers to impersonate users.

**Methods:**
- Intercepting HTTP (non-HTTPS) traffic
- Cross-site scripting (XSS)
- Access via malicious browser extensions

---

### 3. Malicious Extensions

Browser extensions may request access to:
- All visited websites
- Read/modify page content
- Stored credentials

**Risks:**
- Keystroke logging
- Session hijacking
- Redirecting to malicious sites

---

## DNS Security

DNS (Domain Name System) translates domain names (like `uts.edu.au`) into IP addresses.

### 1. DNS Spoofing (DNS Cache Poisoning)

An attacker corrupts the DNS cache to redirect users to malicious websites.

**How it works:**
- Victim queries DNS for `bank.com`
- Attacker intercepts and responds with a fake IP
- Victim visits a fake `bank.com` that looks real

---

### 2. Attack Goals

- **Credential theft** by redirecting users to fake login pages
- **Traffic redirection** for phishing, malware, or ads
- **Surveillance** of user behavior

---

### 3. Defense Mechanisms

| Defense                   | Description                                              |
|---------------------------|----------------------------------------------------------|
| HTTPS Everywhere          | Encrypt traffic and validate certificates                |
| Secure DNS (DNSSEC)       | DNS responses are cryptographically signed               |
| Use of Trusted Resolvers  | ISPs or VPNs with secure DNS                            |
| Disable Untrusted Add-ons | Avoid risky browser extensions and plugins               |
| Avoid HTTP Sites          | Prevents MITM access to cookies and login forms          |

---

## Summary

- Browsers and DNS are major entry points for attackers.
- Passwords and cookies must be protected with encryption and good browsing hygiene.
- DNS spoofing is dangerous because it’s invisible to users.
- Defensive strategies rely on HTTPS, secure DNS, and cautious browser extension use.

Next: We’ll explore how cryptography and certificates work to enable secure communications.
