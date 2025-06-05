# DNS Attacks – Hosts File Poisoning, DNS Spoofing, Cache Poisoning

## Objective  
Simulate DNS-based attacks including:  
1. Local hosts file redirection  
2. Spoofing DNS replies to clients  
3. Poisoning DNS server cache to affect all users

---

## Tools Used  
- Kali Linux (Client, Attacker)  
- Wireshark  
- Netwag (DNS Spoofer)  
- dig / nslookup  
- Hosts file, `/etc/network/interfaces`

---

## A. Local DNS Attack – Hosts File Modification

### Steps:
1. **Check current DNS resolution:**
```bash
dig website.com
```

2. **Check and edit DNS settings:**
```bash
cat /etc/resolv.conf
cat /etc/network/interfaces
sudo nano /etc/network/interfaces
```

- Change to:
```
dns-nameserver 10.0.2.7
```

3. **Restart network interface:**
```bash
sudo ifdown eth0
sudo ifup eth0
```

4. **Test DNS redirect:**
```bash
dig website.com
```

Outcome_: Client gets spoofed IP address based on modified DNS setting.
![Image](https://github.com/user-attachments/assets/a9cc4ee6-e7f6-4902-88cd-efa3add580a6)

---

## B. DNS Response Spoofing – Targeting the Client

### Explanation  
Intercept and spoof a DNS response before the legitimate DNS server replies. This tricks the client into resolving the domain to a fake IP.

### Steps

1. **Run Wireshark on the client (filter for DNS):**
```bash
sudo wireshark
```

2. **Trigger a DNS query from client:**
```bash
dig website.com
```

3. **On attacker VM, open Netwag:**
```bash
sudo netwag
```

- Launch tool **#105 DNS Sniffer and Spoofer**

4. **Fill spoof response using info from Wireshark (domain, ID, fake IP)**

5. **Run `dig` again on client and observe spoofed reply in Wireshark**

DNS is **first-response-wins**, so spoofed reply must arrive before the real one.
![Image](https://github.com/user-attachments/assets/bfd13144-e5ac-49a5-879c-bf93919d9edb)

---

## C. DNS Server Cache Poisoning – Targeting the DNS Server

### Explanation  
Trick the DNS server into caching a forged record, so **every user** gets the wrong IP.

### Steps

1. **Do NOT use Wireshark here. Just dig from client:**
```bash
dig www.uts.edu.au
```

2. **On attacker VM, use Netwag (Tool 105) to spoof a reply with fake IP**

3. **Dig again from client – now the fake IP is served from cache**

4. **If server has cache already, flush it:**
```bash
sudo rndc flush
```

_Impact_: All users relying on this DNS server now get the attacker’s IP.
![Image](https://github.com/user-attachments/assets/255b3a91-1071-4125-a9ae-dbc613d62481)

---

## Defense Tips
- Use DNSSEC to verify integrity of responses  
- Restrict who can query or respond to your DNS server  
- Use secure DNS-over-HTTPS or DoT  
