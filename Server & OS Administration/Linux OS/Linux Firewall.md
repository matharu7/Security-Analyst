---

## 📌 Firewall Overview

| **Firewall** | **Description** | **Use Case** |
| --- | --- | --- |
| **UFW** | User-friendly frontend for iptables | Simple home servers, beginners |
| **iptables** | Powerful, low-level firewall tool | Advanced configurations, complex routing |

### ➕ Additional Info (for understanding)

- A **firewall** controls incoming and outgoing network traffic.
- It works based on **rules** that allow or block traffic.
- UFW simplifies firewall management, while iptables gives full control.

---

## 🔥 Part 1: UFW (Uncomplicated Firewall)

UFW is designed to make firewall configuration easy, especially for beginners.

---

### 📥 UFW Installation & Basic Commands

| **Action** | **Command** | **Description** |
| --- | --- | --- |
| Install UFW | `sudo apt install ufw` | Install the UFW package |
| Enable UFW | `sudo ufw enable` | Turn on the firewall |
| Disable UFW | `sudo ufw disable` | Turn off the firewall |
| Check Status | `sudo ufw status verbose` | Show detailed firewall status |
| Reset All Rules | `sudo ufw reset` | Remove all configured rules |

---

### 🧩 UFW Rule Management

| **Rule** | **Command** | **Description** |
| --- | --- | --- |
| Allow incoming on port 22 (SSH) | `sudo ufw allow 22` | Allow SSH traffic |
| Allow traffic from specific IP | `sudo ufw allow from 192.168.1.100` | Allow traffic from one IP |
| Allow TCP port range | `sudo ufw allow 8000:9000/tcp` | Allow TCP ports from 8000 to 9000 |
| Deny traffic on port 23 | `sudo ufw deny 23` | Block telnet traffic |
| Delete rule | `sudo ufw delete allow 22` | Remove a rule |

### ➕ Extra Explanation

- **Port 22** → Used for SSH
- **Port 23 (Telnet)** → Insecure, usually blocked
- Rules are processed in order

---

### ⚙️ UFW Advanced Configuration

| **Config** | **Command** | **Description** |
| --- | --- | --- |
| Limit SSH connections | `sudo ufw limit 22/tcp` | Protect from brute force by rate limiting SSH |
| Allow traffic on interface eth0 port 80 | `sudo ufw allow in on eth0 to any port 80` | Allow HTTP traffic only on eth0 |
| Set default deny incoming policy | `sudo ufw default deny incoming` | Deny all incoming by default |
| Set default allow outgoing policy | `sudo ufw default allow outgoing` | Allow all outgoing traffic by default |

### 🔒 Best Practice (Extra)

- Always set **default deny incoming**
- Only allow required services (SSH, HTTP, HTTPS)

---

## 🛡️ Part 2: iptables (Advanced Firewall)

iptables is a **low-level firewall tool** used for full control over packet filtering.

---

### 🧠 iptables Basic Concepts

| **Table** | **Purpose** |
| --- | --- |
| Filter | Packet filtering (default) |
| NAT | Network Address Translation |
| Mangle | Specialized packet alterations |

---

### 🔗 Main Chains in Filter Table

| **Chain** | **Purpose** |
| --- | --- |
| INPUT | Incoming packets to the local system |
| FORWARD | Packets routed through the system |
| OUTPUT | Outgoing packets from the local system |

---

### 📜 iptables Basic Commands

| **Command** | **Description** |
| --- | --- |
| List all rules | `sudo iptables -L -v -n` |
| List rules with line numbers | `sudo iptables -L -v -n --line-numbers` |
| Flush all rules (careful!) | `sudo iptables -F` |

---

### 🧩 iptables Rule Management

| **Rule** | **Command** | **Description** |
| --- | --- | --- |
| Allow incoming SSH | `sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT` | Accept SSH traffic |
| Allow established connections | `sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT` | Accept existing connections |
| Block an IP address | `sudo iptables -A INPUT -s 192.168.1.100 -j DROP` | Drop packets from specific IP |
| Allow loopback traffic | `sudo iptables -A INPUT -i lo -j ACCEPT` | Allow all traffic on loopback interface |

### ➕ Extra Notes

- `A` → Append rule
- `j ACCEPT/DROP` → Action taken
- Loopback (`lo`) is required for system services

---

### 💾 iptables Saving & Restoring Rules

| **Action** | **Command** | **Notes** |
| --- | --- | --- |
| Save rules (Ubuntu/Debian) | `sudo iptables-save > /etc/iptables.rules` | Save current rules |
| Restore rules | `sudo iptables-restore < /etc/iptables.rules` | Restore saved rules |
| Save rules (RHEL/CentOS) | `sudo service iptables save` | Save using service |

---

### 🚀 iptables Advanced Examples

| **Use Case** | **Command** | **Description** |
| --- | --- | --- |
| Port forwarding | `sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080` | Redirect HTTP traffic from 80 to 8080 |
| Rate limiting SSH | `sudo iptables -A INPUT -p tcp --dport 22 -m limit --limit 10/min -j ACCEPT` | Limit SSH connection rate |
| Log dropped packets | `sudo iptables -N LOGGING<br>sudo iptables -A INPUT -j LOGGING<br>sudo iptables -A LOGGING -m limit --limit 2/min -j LOG --log-prefix "IPTables-Dropped: " --log-level 4<br>sudo iptables -A LOGGING -j DROP` | Log and drop packets |

---

### ❌ Delete iptables Rule

```
sudo iptables -D INPUT 1

```

**Explanation (Extra):**

- `D` → Delete
- `INPUT` → Chain name
- `1` → Rule number (from `-line-numbers`)

---
