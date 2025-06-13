# TLS and Transport Layer Security

This section explains how **Transport Layer Security (TLS)** ensures secure communication over networks. TLS is the cryptographic protocol used in HTTPS, VPNs, and email security.

---

## What is TLS?

TLS (formerly SSL) is a cryptographic protocol that provides secure data transmission between clients and servers.

### Goals of TLS:

- **Encryption** – ensures data is unreadable to outsiders
- **Authentication** – verifies server and optionally client identities
- **Integrity** – ensures data has not been tampered with

---

## How TLS Works

TLS uses a combination of symmetric and asymmetric encryption, digital certificates, and hashing.

### TLS Handshake Process (Simplified):

1. **Client Hello**:
   - Client sends supported cipher suites, TLS version, and a random number.

2. **Server Hello**:
   - Server responds with chosen cipher suite, another random number, and its digital certificate (with public key).

3. **Certificate Verification**:
   - Client validates the server certificate against trusted root CAs.

4. **Key Exchange**:
   - Client generates a **pre-master secret**, encrypts it with the server’s public key, and sends it.

5. **Session Key Derivation**:
   - Both parties compute the same symmetric session key using the random values and pre-master secret.

6. **Encrypted Communication**:
   - All subsequent messages are encrypted using the session key.

---

## Important TLS Concepts

| Concept              | Description                                  |
|----------------------|----------------------------------------------|
| **Symmetric Key**    | Used for fast encrypted communication         |
| **Public Key**       | Used to encrypt the pre-master secret         |
| **Session Key**      | Derived from both sides, used for encryption  |
| **Certificate**      | Verifies server's identity                    |

---

## TLS in Action

- Websites using **HTTPS** rely on TLS for secure login, payment, and data exchange.
- Email servers use TLS to encrypt messages during transfer.
- VPNs and remote work tools also rely on TLS tunnels.

---

## TLS vs SSL

- TLS is the modern replacement for SSL (Secure Sockets Layer).
- SSL 2.0 and 3.0 are now deprecated due to vulnerabilities.
- TLS 1.3 is the latest and most secure version.

---

## Lab Tools

In cybersecurity labs, you may analyze TLS using tools like:

- **Wireshark**: to inspect handshakes and encrypted traffic
- **OpenSSL**: to simulate and generate TLS certificates and connections

---

## Summary

- TLS secures communication across the internet.
- Combines symmetric encryption (for speed) and asymmetric encryption (for key exchange).
- TLS certificates validate the identity of the server.
- The handshake protocol enables secure connection setup.
