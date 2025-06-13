# Man-in-the-Middle (MITM) Attacks and PKI Weaknesses

This document explains **man-in-the-middle (MITM)** attacks and how attackers may exploit **public key infrastructure (PKI)**. These attacks intercept or manipulate data between two communicating parties without their knowledge.

---

## What Is a MITM Attack?

In a **man-in-the-middle attack**, the attacker silently sits between two parties (e.g., a client and server), intercepting and possibly altering communication.

### Common MITM Vectors

| Type         | Description                                                |
|--------------|------------------------------------------------------------|
| **ARP Spoofing** | Tricks the local network into sending traffic to attacker |
| **DNS Spoofing** | Redirects domain name lookups to malicious IPs           |
| **SSL Stripping** | Downgrades secure HTTPS traffic to HTTP                |
| **Wi-Fi MITM**    | Exploits unsecured public Wi-Fi networks                |

---

## Example: ARP Spoofing

Attacker sends fake ARP replies to a victim and gateway:

```bash
arpspoof -t 192.168.1.100 192.168.1.1
```

Now, traffic intended for the gateway is sent to the attacker first.

---

## Consequences of MITM

- Password interception
- Session hijacking
- Data tampering
- Certificate spoofing (if TLS is not properly validated)

---

## Public Key Infrastructure (PKI) Weaknesses

PKI secures TLS/SSL connections using **certificates** issued by **Certificate Authorities (CAs)**. However, this system has vulnerabilities.

### Key Threats

| Vulnerability            | Description                                       |
|--------------------------|---------------------------------------------------|
| **Rogue CA**             | A trusted CA issues fraudulent certificates       |
| **Compromised Root CA**  | If root CA is hacked, all trust is broken         |
| **Self-signed Certificate Abuse** | Used by attackers for phishing or spoofed HTTPS |

---

## Certificate Forgery and MITM

An attacker can impersonate a secure site using a fake or misissued certificate.

### Example Scenario:

1. Attacker intercepts request to `https://bank.com`
2. Sends back a **forged certificate** (maybe self-signed)
3. If the user ignores browser warning, the attacker decrypts and reads traffic

---

## Certificate Pinning and Defense

### How to Defend Against MITM:

- **Use Certificate Pinning**: Apps verify certificates match a known fingerprint
- **Validate Certificate Chains**: Ensure they trace back to a trusted root
- **Use Strong Encryption**: Only allow TLS 1.2 or 1.3
- **Educate Users**: Warn not to click through certificate warnings

---

## Summary

- MITM attacks exploit trust in network infrastructure or certificate validation.
- PKI is strong but depends on trusting CAs — which may be misused or compromised.
- Tools like ARP spoofers, DNS poisons, and SSL stripping proxies are common in lab simulations.

