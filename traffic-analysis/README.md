#  Traffic Analysis – Wireshark & Netwag

##  Objective
Analyze unencrypted network traffic using Wireshark and Netwag to extract sensitive information such as usernames and passwords transmitted via insecure protocols (HTTP, Telnet, FTP).

##  Tools Used
- Wireshark  
- Netwag  
- Kali Linux / VMWare   

##  Lab Activities
- Captured live packets from HTTP, FTP, and Telnet sessions. 
- Used filters to isolate login traffic.
- Followed TCP streams to extract cleartext credentials .
- Analyzed packet headers and payloads.
- Identified security risks in legacy protocols

##  Key Outcomes
- Understood how insecure protocols expose user data  
- Learned to reconstruct full communication sessions using Wireshark

##  Useful Wireshark Filters
telnet
ftp
http
tcp.port == 21
tcp contains "username"

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

## 🧪 Activities Performed
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

