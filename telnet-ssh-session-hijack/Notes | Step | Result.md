# Session Hijacking & Protocol Abuse – Telnet, SSH, ICMP Reset, TCP Hijack

## Objective  
Simulate session-level and packet-based attacks to demonstrate how insecure services like Telnet and poorly filtered ICMP/TCP traffic can be exploited for hijacking or disruption.

---

## Tools Used  
- Wireshark / TShark  
- Netwag  
- Kali Linux  
- Telnet and SSH services  
- Custom scripts for packet manipulation

---

## A. Telnet Credential Interception

### Steps:

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

_Telnet sends everything unencrypted, making sniffing trivial._

![Image](https://github.com/user-attachments/assets/a47696a7-591a-431e-b50a-93645fd3bc8c)
---

## B. SSH Login Attempt Monitoring

1. Try brute-forcing or logging in with known credentials:
```bash
ssh user@<ip>
```

2. Run Wireshark with filter: `tcp.port == 22`

- Although encrypted, SSH traffic patterns may still reveal login behavior or allow MITM setup with prior key insertion.

![Image](https://github.com/user-attachments/assets/45e8bd7d-d8ee-4589-bf3f-28c72ab2ae34)
---

## C. ICMP Blind Connection Reset

### Steps:

1. Identify open TCP connection (e.g., from client to web server)

2. Use Netwag:
- Tool `#98: Send ICMP Destination Unreachable`
- Tool `#100: Send ICMP Source Quench`
- Tool `#101: Send ICMP Redirect`

3. Observe disconnection or degraded performance on target

- ICMP reset floods can drop valid sessions, causing denial-of-service.
![Image](https://github.com/user-attachments/assets/14d4d627-8edf-40e5-bd5a-ef7248048b52)
---

## D. TCP Session Hijacking

1. Sniff sequence numbers of an active session  
2. Send forged TCP packets with predicted seq/ack values  
3. Inject spoofed commands into session (e.g., echo or ls)

- Works best on non-encrypted protocols like Telnet or older FTP

-  **This attack is now mostly theoretical unless ARP poisoning or MITM is active.**
![Image](https://github.com/user-attachments/assets/195e0633-6984-470a-a992-0a357b2fa621)
---


## Defense Tips

- Disable Telnet; use SSH only with key auth  
- Drop unexpected ICMP types on firewalls  
- Use encrypted sessions with strong sequence randomness  
- Monitor ARP and DNS poisoning signs to prevent MITM setups
