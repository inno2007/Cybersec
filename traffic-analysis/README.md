#  Traffic Analysis – Wireshark & Netwag

##  Objective
Analyze unencrypted network traffic using Wireshark and Netwag to extract sensitive information such as usernames and passwords transmitted via insecure protocols (HTTP, Telnet, FTP).

## 🛠 Tools Used
- Wireshark  
- Netwag  
- Kali Linux / Ubuntu  
- Optional: tcpdump, Netcat

##  Lab Activities
- Captured live packets from HTTP, FTP, and Telnet sessions  
- Used filters to isolate login traffic  
- Followed TCP streams to extract cleartext credentials  
- Analyzed packet headers and payloads  
- Identified security risks in legacy protocols

##  Key Outcomes
- Gained experience in passive traffic analysis  
- Understood how insecure protocols expose user data  
- Learned to reconstruct full communication sessions using Wireshark

##  Useful Wireshark Filters
```bash
telnet
ftp
http
tcp.port == 21
tcp contains "username"
