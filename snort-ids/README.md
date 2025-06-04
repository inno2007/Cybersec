# 🚨 Intrusion Detection System – Snort Rules, Alerts & Logs

## 🎯 Objective  
Use Snort in IDS mode to detect suspicious traffic. Write custom rules, trigger alerts, and analyze log outputs to understand how signature-based detection works.

---

## 🛠️ Tools Used  
- Snort (Command-line IDS)  
- Kali Linux / Ubuntu  
- Wireshark or TShark (optional)  
- Custom traffic generators (e.g., ping, netcat, curl)

---

## 🧪 A. Verify Snort Installation

```bash
snort -V
```

Output should show version, build info, and available preprocessors.

---

## 📁 B. Set Up Rules Directory

### Step 1: Create a test rules file
```bash
sudo nano /etc/snort/rules/test.rules
```

### Step 2: Add a basic ICMP detection rule
```bash
alert icmp any any -> any any (msg:"ICMP packet detected"; sid:1000001;)
```

---

## 🧪 C. Run Snort in NIDS Mode with Custom Rules

```bash
sudo snort -A console -q -c /etc/snort/snort.conf -i eth0 -l /var/log/snort -K ascii
```

To test only your rule:
```bash
sudo snort -A console -q -c /etc/snort/snort.conf -i eth0 -l /var/log/snort -K ascii -r test.pcap -s 65535
```

---

## 🧪 D. Trigger the Rule with ICMP

Open another terminal and ping:
```bash
ping <target_ip>
```

If rule works, Snort should print:
```
[**] [1:1000001:0] ICMP packet detected [**]
```

Also check:
```bash
cat /var/log/snort/alert
```

---

## 📄 E. Create More Rules

### Detect TCP to port 23 (Telnet):
```bash
alert tcp any any -> any 23 (msg:"Telnet attempt"; sid:1000002;)
```

### Detect specific string in HTTP:
```bash
alert tcp any any -> any 80 (msg:"Potential Web Attack"; content:"cmd.exe"; sid:1000003;)
```

---

## 📜 Snort Rule Structure

```
action proto src_ip src_port -> dst_ip dst_port (options)
```

Example:
```bash
alert tcp any any -> any 22 (msg:"SSH connection detected"; sid:1000004;)
```

---

## 📊 F. Logging & Alert Analysis

View alert logs:
```bash
cat /var/log/snort/alert
```

Each alert shows:
- Timestamp  
- Rule SID  
- Message  
- Protocol + IP details

---

## 🔐 Security Insights

- Snort is powerful for detecting known threats using signatures  
- Signature quality and rule tuning are critical to reduce false positives  
- Log analysis + proper action scripts are essential for real-time response

✅ Consider combining Snort with firewalls or SIEM tools in larger setups.
