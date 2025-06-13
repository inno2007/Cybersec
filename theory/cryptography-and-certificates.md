# Cryptography and Certificates

This section introduces cryptographic techniques used in cybersecurity to protect data integrity, confidentiality, and authentication. It focuses on public key infrastructure (PKI), encryption, and digital certificates.

---

## What is Cryptography?

Cryptography is the science of protecting information through encoding so that only authorized parties can access it.

### Core Functions

| Function         | Description                                      |
|------------------|--------------------------------------------------|
| **Confidentiality** | Keep data private via encryption                |
| **Integrity**       | Ensure data is not modified                     |
| **Authentication**  | Verify identity of sender or receiver           |
| **Non-repudiation** | Prevent sender from denying a message was sent  |

---

## Types of Cryptography

### 1. Symmetric Key Encryption

- One key for both encryption and decryption.
- Fast but key must be securely shared in advance.
- Common algorithms: **AES**, DES

```bash
openssl enc -aes-256-cbc -in plain.txt -out encrypted.txt
```

### 2. Asymmetric Key Encryption

- Uses **public key** for encryption and **private key** for decryption.
- Enables secure communication without key exchange.
- Common algorithm: **RSA**

```bash
# Generate private key
openssl genpkey -algorithm RSA -out private.pem

# Extract public key
openssl rsa -pubout -in private.pem -out public.pem
```

---

## Public Key Infrastructure (PKI)

PKI provides the framework for issuing, managing, and validating digital certificates.

### Key Components:

| Component         | Role                                                |
|-------------------|------------------------------------------------------|
| **Certificate Authority (CA)** | Trusted entity that issues certificates       |
| **Digital Certificate**        | Confirms ownership of a public key           |
| **Root Certificate**           | Top-level certificate, trusted implicitly     |

---

## Certificate File Formats

- `.crt`, `.cer`, `.pem` – public certificate
- `.key`, `.pem` – private key
- `.csr` – certificate signing request

---

## Creating a Self-Signed Certificate (Lab)

```bash
# Step 1: Generate private key
openssl genrsa -out private.key 2048

# Step 2: Generate certificate
openssl req -new -x509 -key private.key -out cert.crt -days 365
```

---

## Certificate Chain

A chain of trust exists from:
- Root CA → Intermediate CA → Website Certificate

Browsers verify that the certificate chain ends in a trusted Root CA.

---

## Viewing a Certificate

```bash
openssl x509 -in cert.crt -text
```

---

## Summary

- Cryptography secures data through encryption, digital signatures, and hashing.
- Asymmetric encryption (RSA) allows secure key exchange.
- Certificates are part of PKI and validate identity for HTTPS and secure email.
- OpenSSL is used to generate and inspect keys and certificates.

