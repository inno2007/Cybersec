# 📡 Network Layer Attacks – SYN Flooding, ARP Cache Poisoning, ICMP Redirect

## 🎯 Objective  
Simulate low-level network attacks that disrupt communication or redirect traffic using fake packets. Learn how attackers manipulate ARP tables, spoof ICMP messages, and flood TCP connections.

---

## 🛠️ Tools Used  
- Netwag  
- TShark / Wireshark  
- Kali Linux VMs  
- `arp`, `ping`, `tshark` commands

---

## 🌊 A. SYN Flood Attack – TCP Resource Exhaustion

### 🔧 Setup & Execution

1. Start packet capture on target (use TShark instead of Wireshark):
```bash
sudo tshark
```

2. Launch SYN flood using Netwag:
- Tool `#76: SynFlood`

🧠 **Effect**: Creates many half-open TCP connections to overwhelm the server.

3. Observe traffic with:
```bash
sudo tshark
```

---

## 🕸️ B. ARP Cache Poisoning – Man-in-the-Middle Setup

### 🔧 Steps:

1. On the server VM, check the ARP table:
```bash
arp -a
```

2. From attacker VM, launch Netwag:
- Tool `#80: Periodic ARP Reply Sender`

3. After the spoofing starts, check again:
```bash
arp -a
```

🧠 The server now associates the client’s IP with the attacker’s MAC address — this sets up a MITM scenario.

> Bonus: Use `arpspoof` as an alternative:
```bash
sudo apt install dsniff
sudo arpspoof -i eth0 -t <targetIP> <gatewayIP>
```

---

## 🔄 C. ICMP Redirect Attack – Route Hijacking

### 🧪 Steps:

1. On the client VM:
```bash
ping 10.0.2.6
sudo wireshark
```

- Filter: `icmp`

2. From attacker VM, open Netwag:
- Tool `#86: Send ICMP Redirect`

3. Observe Wireshark to verify the victim is now being redirected.

🧠 **Goal**: Trick the victim into routing future traffic through the attacker.


---

## 🔐 Mitigation Tips

- Enable ARP inspection and static bindings on switches  
- Use SYN cookies on servers to defend against SYN floods  
- Block or validate ICMP redirect messages  
- Segment and monitor internal traffic to catch MITM attempts
