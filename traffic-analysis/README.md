#  Traffic Analysis – Wireshark - Sensitive Data In Plaintext

##  Objective
Analyze unencrypted network traffic using Wireshark and Netwag to extract sensitive information such as usernames and passwords transmitted via insecure protocols (HTTP, Telnet, FTP).

##  Tools Used
- Wireshark  
- Netwag  
- Kali Linux / VMWare   

##  Activities
- Captured live packets from HTTP, FTP, and Telnet sessions. 
- Used filters to isolate login traffic.
- Followed TCP streams to extract cleartext credentials .
- Analyzed packet headers and payloads.
- Identified security risks in legacy protocols

##  Key Outcomes
- Understood how insecure protocols expose user data  
- Learned to reconstruct full communication sessions using Wireshark


##  Commands and Setup

### 1. Enable network interface monitoring
To identify interface: 
```bash
ip a
```
```bash
sudo ifconfig eth0 promisc
```

### 2. Start Wireshark (as root if needed)
```bash
sudo wireshark
```

### 3. Begin packet capture
- Select interface (eth0)
- Apply filters (see below)
- Start capturing while a Telnet or HTTP session is active

##  Filters Used in Wireshark
Capture Filters:
- port 23      # Telnet
- port 21      # FTP
- port 80      # HTTP

## Display Filters:
- telnet
- ftp
- http
- tcp contains "password"
- tcp contains "login"

##  Activities Performed
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

##  Key Observations
1. Unencrypted protocols (Telnet, FTP, HTTP) transmit sensitive data in plaintext.
2. Wireshark TCP stream reconstruction makes it easy to read entire conversations.
3. Even local attackers on the same

## Exact Task Being Done 
- For extracting unecrpyted data, we need to set up Wireshark in the Server VM. sudo wireshark
- Filter http on the wireshark 
- Open the website http//testphp.vulnweb.com/login.php
- Entered my name and password then click submit 
- Reopen the wireshark and find the POST INFO
- Opened the packet and open the Line-based test data
- Image extraction = Still uses wireshark. 
- Click the packets -> File (Top Left) -> Extract -> HTTP -> Click the image/gif -> Save


## Outcome
![Image](https://github.com/user-attachments/assets/bfd0e26d-22f4-4429-9e35-381071d0973e)

![Image](https://github.com/user-attachments/assets/a4feb3b7-5aee-4858-ae09-1bce1d4bf0fd)
