# 📝 Lab Notes – Traffic Analysis (Wireshark, Netwag)

## 🎯 Lab Goal
The main objective of this lab is to capture and analyze unencrypted traffic in order to extract sensitive information such as login credentials, session data, and insecure protocol behavior. This simulates how attackers can use packet sniffing to intercept data over insecure networks.

---

## 🔧 Environment Setup
- OS: Kali Linux (or Ubuntu)
- Tools: Wireshark, Netwag (optional), tcpdump
- Network setup: Virtual machines on the same network or host + guest (bridge or NAT)

---

## 🛠 Commands and Setup

### 1. Enable network interface monitoring
ip a        # Identify interface (e.g., eth0, wlan0)
sudo ifconfig eth0 promisc

### 2. Start Wireshark (as root if needed)
sudo wireshark

### 3. Begin packet capture
- Select interface (e.g., eth0)
- Apply filters (see below)
- Start capturing while a Telnet or HTTP session is active

### 🔍 Filters Used in Wireshark
Capture Filters:
port 23      # Telnet
port 21      # FTP
port 80      # HTTP

### Display Filters:
telnet
ftp
http
tcp contains "password"
tcp contains "login"

### 🧪 Activities Performed
#### ➤ Telnet Credential Capture
- Set up Telnet server and connected from client VM
- Captured session using Wireshark
- Used “Follow TCP Stream” to view full session
- Identified clear-text username and password

#### ➤ HTTP Form Submission
- Captured HTTP POST request containing login form data
- Parsed parameters from the payload

#### ➤ FTP Login Packet
- Observed FTP authentication (USER, PASS) in plaintext
- Noted command-response behavior in FTP protocol

🧠 Key Observations
1. Unencrypted protocols (Telnet, FTP, HTTP) transmit sensitive data in plaintext.
2. Wireshark TCP stream reconstruction makes it easy to read entire conversations.
3. Even local attackers on the same
