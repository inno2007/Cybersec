# 🛠️ Session Hijacking & Protocol Abuse – Telnet, SSH, ICMP Reset, TCP Hijack

## 🎯 Objective  
Simulate session-level and packet-based attacks to demonstrate how insecure services like Telnet and poorly filtered ICMP/TCP traffic can be exploited for hijacking or disruption.

---

## 🛠️ Tools Used  
- Wireshark / TShark  
- Netwag  
- Kali Linux  
- Telnet and SSH services  
- Custom scripts for packet manipulation

---

## 🧪 A. Telnet Credential Interception

### 🔧 Steps:

1. Start Telnet service on server:
```bash
sudo apt install telnetd
sudo service inetd restart
```

2. Connect from client:
```bash
telnet <server_ip>
```

3. On attacker, run Wireshark:
- Filter: `telnet`

4. Follow TCP Stream to view:
- Username and password in plaintext

🧠 _Telnet sends everything unencrypted, making sniffing trivial._

---

## 🔐 B. SSH Login Attempt Monitoring (Optional)

1. Try brute-forcing or logging in with known credentials:
```bash
ssh user@<ip>
```

2. Run Wireshark with filter: `tcp.port == 22`

🧠 Although encrypted, SSH traffic patterns may still reveal login behavior or allow MITM setup with prior key insertion.

---

## 🧨 C. ICMP Blind Connection Reset

### 🔧 Steps:

1. Identify open TCP connection (e.g., from client to web server)

2. Use Netwag:
- Tool `#98: Send ICMP Destination Unreachable`
- Tool `#100: Send ICMP Source Quench`
- Tool `#101: Send ICMP Redirect`

3. Observe disconnection or degraded performance on target

🧠 ICMP reset floods can drop valid sessions, causing denial-of-service.

---

## 🕶️ D. TCP Session Hijacking (Conceptual)

1. Sniff sequence numbers of an active session  
2. Send forged TCP packets with predicted seq/ack values  
3. Inject spoofed commands into session (e.g., echo or ls)

🧠 Works best on non-encrypted protocols like Telnet or older FTP

💡 **This attack is now mostly theoretical unless ARP poisoning or MITM is active.**

---


## 🔐 Defense Tips

- Disable Telnet; use SSH only with key auth  
- Drop unexpected ICMP types on firewalls  
- Use encrypted sessions with strong sequence randomness  
- Monitor ARP and DNS poisoning signs to prevent MITM setups
