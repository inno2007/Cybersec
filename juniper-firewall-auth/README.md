# 🔒 Juniper SRX300 – Security Policies & Firewall User Authentication

## 🎯 Objective  
Work with Juniper's SRX series firewall to control network access using two key layers of defense:
Security Policies between zones  
Firewall User Authentication (Pass-through Login)

---

## 🛠️ Tools & Devices  
- Juniper SRX300 (via SSH)  
- Terminal (PuTTY or Linux CLI)  
- Lab IPs (given by tutor)  
- SSH access to:  
  - Boundary Router: `138.25.62.61`  
  - Internal Router: `10.0.0.1XX`  

---

## 🔧 JUNOS Modes Overview

| Mode             | Prompt | Description                              |
|------------------|--------|------------------------------------------|
| Shell Mode       | `%`    | Unix shell (low-level)                   |
| Operational Mode | `>`    | Run commands (e.g. `show`)               |
| Configuration    | `#`    | Make config changes (`set`, `delete`)    |

Enter modes:
```bash
cli         # from shell to operational
configure   # from operational to configuration
```

---

## 🔁 – Configure Security Policies (Zone-Based Firewall)

### Step 1: SSH into the Routers

```bash
ssh student@138.25.62.61     # Password: cyber_student
ssh student@10.0.0.1XX       # Password: Juniper
```

---

### Step 2: View Existing Policies
```bash
show security policies
```

🧠 See all allowed/denied traffic between zones like Internet → Internal.

---

### Step 3: Create a New Security Policy

```bash
configure
edit security policies
edit from-zone Internet to-zone Internal
edit policy allow-ftp

set match source-address any
set match destination-address any
set match application junos-ftp
set then permit

top
commit
```

✅ This allows FTP traffic from any source to the internal server.

---

### Step 4: Add Multiple Applications (Optional)

```bash
edit from-zone Internet to-zone Internal
edit policy allow-web

set match application [ junos-http junos-https ]
set then permit

top
commit
```

---

### Step 5: Delete a Policy (If Needed)

```bash
configure
edit security policies
edit from-zone Internet to-zone Internal
delete policy allow-ftp

top
commit
```

---

## 🔐  – Configure Firewall User Authentication (Pass-through Login)

### 🔑 Concept  
When a user tries to access a service (Telnet, HTTP), the firewall intercepts and forces them to log in. If credentials are correct → access is granted.

---

### Step 1: Create an Access Profile

```bash
set access profile auth-profile authentication-order password
set access profile auth-profile client username student password "juniper123"
```

---

### Step 2: Enable Pass-through Authentication & Banner

```bash
set firewall authentication pass-through default-profile auth-profile
set firewall authentication pass-through banner "Welcome. Please authenticate to continue."
```

---

### Step 3: Add Firewall Auth to a Policy

```bash
edit security policies
edit from-zone Internet to-zone Internal
edit policy auth-policy

set match source-address any
set match destination-address any
set match application junos-telnet
set then permit application-services firewall-authentication

top
commit
```

🧠 Now, Telnet will require a login prompt before connecting.

---

### Step 4: Test It

1. Try to Telnet from your PC to the server:
```bash
telnet <server-ip>
```

2. You should see:
```
Welcome. Please authenticate to continue.
Username: student
Password: juniper123
```

3. On success:
```
Login Successful
```

✅ Firewall remembers IP temporarily to avoid repeated login.

---

### 🔁 Step 5: Delete User Auth Settings

```bash
delete access profile auth-profile
delete firewall authentication pass-through
delete security policies from-zone Internet to-zone Internal policy auth-policy
commit
```

---


## 🔐 Final Notes

- Juniper uses **zones + policies** to control traffic  
- User authentication adds a second security layer  
- Always commit carefully and test policies before deployment  
- JUNOS config tree makes it easy to manage rules at scale

✅ This lab demonstrates enterprise-grade access control in real-world networks.
