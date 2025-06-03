# 🔐 Password Cracking – John the Ripper

## 🎯 Objective  
Simulate password cracking using John the Ripper to understand how dictionary-based and rule-based attacks work against hashed password files.

---

## 🛠️ Tools Used  
- John the Ripper  
- Kali Linux / Ubuntu  
- Wordlists (`password.lst`, `rockyou.txt`)  
- Lab files: `passwd.txt`, `shadow.txt`, `mypassword`, `password.lst`

---

## 🧪 Lab Activities

### 1. Navigate to Password Files
cd ~/Documents
ls

# 🔐 Password Cracking – John the Ripper

## 🎯 Objective  
Simulate password cracking using John the Ripper to understand how dictionary-based and rule-based attacks work against hashed password files.

---

## 🛠️ Tools Used  
- John the Ripper  
- Kali Linux / Ubuntu  
- Wordlists (`password.lst`, `rockyou.txt`)  
- Lab files: `passwd.txt`, `shadow.txt`, `mypassword`, `password.lst`

---

## 🧪 Lab Activities

### 1. Navigate to Password Files
cd ~/Documents
ls

### 2. View Combined Unshadowed Passwords
cat mypasswd
Make sure it contains hashed lines like:
alice:$6$hash...:18077:0:99999:7:::

### 3. Crack Using Default Wordlist
john mypasswd
Starts cracking with basic built-in wordlist.

### 4. View Cracked Passwords
john --show mypasswd
Example: alice:password123

### 5. Use a Custom Wordlist
john mypasswd --wordlist=password.lst

Also try:
john mypasswd --wordlist=/usr/share/wordlists/rockyou.txt

### 6. Rule-Based Cracking (Stronger)
john --wordlist=password.lst --rules mypasswd
This applies transformations: password → Password1, etc.

### 🔍 Key Learnings
- Weak passwords (simple or reused) are quickly cracked
- Custom wordlists and rules increase cracking success
- Hash-only storage without salt is insecure
---

Notes
- Format for password files must match /etc/passwd and /etc/shadow structure
- Use unshadow if needed to combine passwd and shadow files: unshadow passwd.txt shadow.txt > mypasswd
