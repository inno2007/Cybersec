# OpenSSL Encryption, RSA, GPG & PKI Certificate Authority Lab

## Objective  
Simulate end-to-end encryption techniques including symmetric (AES), asymmetric (RSA), key management with GPG, and full certificate chain setup with OpenSSL. Build your own certificate authority and run a secure HTTPS server.

---

## Tools Used  
- OpenSSL (CLI)  
- GPG  
- Firefox  
- Apache Web Server  
- Localhost (`/etc/hosts`)  
- Kali Linux / VMWare

---

## A. AES Encryption & Decryption (Symmetric)

### Step 1: Encrypt using AES-128-CBC
```bash
openssl enc -aes-128-cbc -e -in Text_file.txt -out Encrypted.enc -K 11223344556677889900 -iv 0000
```

### Step 2: Decrypt with Same Key
```bash
openssl enc -aes-128-cbc -d -in Encrypted.enc -out Decrypted.txt -K 11223344556677889900 -iv 0000
```

🧠 Notes:  
- `-K` and `-iv` must be in **hex**  
- Use 128-bit key → 32 hex characters

To view output:
```bash
gedit Encrypted.enc
```

---

## B. RSA Encryption & Decryption (Asymmetric)

### Step 1: Generate RSA Key Pair
```bash
openssl genrsa -out private.key 2048
openssl rsa -in private.key -pubout -out public.pem
```

### Step 2: Encrypt with Public Key
```bash
openssl rsautl -encrypt -inkey public.pem -pubin -in plain.txt -out encrypted.enc
```

### Step 3: Decrypt with Private Key
```bash
openssl rsautl -decrypt -inkey private.key -in encrypted.enc -out decrypted.txt
```

---

## C. GPG – Digital Signatures & Key Exchange

### Generate Key Pair
```bash
gpg --gen-key
```

### List Keys
```bash
gpg --list-keys
```

### Export & Import Keys
```bash
gpg --armor --export your_email@example.com > mykey.asc
gpg --import partnerkey.asc
```

### Sign & Encrypt
```bash
gpg --sign-key partner@example.com
gpg --encrypt --armor -r partner@example.com file.txt
```

### Decrypt & Verify
```bash
gpg --decrypt file.txt.asc
gpg --verify signedfile.asc
```

---

## 🏛D. Create Certificate Authority (CA) – Root Cert

### Step 1: Setup CA Folder
```bash
mkdir CA && cd CA
cp /usr/lib/ssl/openssl.cnf ./
gedit openssl.cnf
```
Change:
```
dir = ./
```

### Step 2: Prepare CA Structure
```bash
mkdir certs crl newcerts
touch index.txt
echo 01 > serial
```

### Step 3: Create Root CA Certificate
```bash
openssl req -new -x509 -keyout ca.key -out ca.crt -config openssl.cnf
```
Use passphrase: `cybersec`  
Inputs:  
- Country: AU  
- State: NSW  
- City: Sydney  
- Org: UTS  
- Unit: FEIT  
- Common Name: cybersec.com.au  
- Email: root@cybersec.com.au

---

## E. Create Server Certificate (CSR + Sign)

### Step 1: Generate Private Key
```bash
openssl genrsa -aes128 -out server.key 1024
```
Password: `cybersec`

### Step 2: Generate CSR
```bash
openssl req -new -key server.key -out server.csr -config openssl.cnf
```

### Step 3: Sign with Root CA
```bash
openssl ca -in server.csr -out server.crt -cert ca.crt -keyfile ca.key -config openssl.cnf
```

Approve prompts with `y`

---

## 🌍 F. Run a Local HTTPS Website (PKI in Action)

### Step 1: Map Domain to Localhost
```bash
sudo gedit /etc/hosts
```
Add:
```
127.0.0.1 cybersec.com.au
```

### Step 2: Combine Server Cert + Key
```bash
cp server.key server.pem
cat server.crt >> server.pem
```

### Step 3: Launch HTTPS Server
```bash
openssl s_server -cert server.pem -www
```

### Step 4: Load in Firefox
Visit:
```
https://cybersec.com.au:4433/
```
- Shows warning until Root CA is trusted

---

### Step 5: Trust the Root CA
Firefox → Preferences → Privacy & Security → Certificates  
→ Import `ca.crt` → Check "Trust to identify websites"

Now reload the site → No warning 

---
