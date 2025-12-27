> ⚠️ Important Note (Kept):
> 
> 
> Paths are **case-sensitive**: always use lowercase `/var/log/...`
> 
> Where distributions differ, common variants are noted (Debian/Ubuntu vs RHEL/CentOS).
> 

---

## 📂 Overview: Why Linux Logs Matter (Extra Context)

Linux logs are critical for:

- 🔐 Security monitoring (intrusions, failed logins)
- 🕵️ Incident response & forensics
- ⚙️ Troubleshooting system & service failures
- 📜 Compliance and auditing

Most logs are stored under:

```
/var/log/

```

---

## 🔑 Core Login & Session Logs

### `/var/log/btmp`

- **Binary file** that records **failed login attempts** (bad passwords).
- Useful for detecting brute-force attacks.

View with:

```bash
sudo lastb

```

---

### `/var/log/wtmp`

- **Binary file** that records **login and logout history**.
- Tracks who logged in, when, and from where.

View with:

```bash
last

```

---

### `/var/log/utmp`

- Runtime file for **currently logged-in users**.
- Used by commands like `who` and `w`.

View with:

```bash
who
w

```

---

### `/var/log/lastlog`

- Reports **last login time for each user**.
- Helpful for identifying inactive or suspicious accounts.

View with:

```bash
lastlog

```

---

## 🔐 Authentication & Sudo Logs

### `/var/log/auth.log` (Debian/Ubuntu)

- Records:
    - SSH login attempts
    - `sudo` usage
    - PAM authentication messages
    - Other authentication events

🔁 **RHEL/CentOS equivalent:**

```
/var/log/secure

```

Inspect:

```bash
sudo tail -f /var/log/auth.log
sudo grep "sshd" /var/log/auth.log
sudo journalctl -u sshd

```

🛡️ **Security Tip (Extra):**

- Repeated “Failed password” entries may indicate brute-force attacks.

---

## 🧠 System & Kernel Messages

### `/var/log/syslog` (Debian/Ubuntu)

- General system activity (daemons, services).
- RHEL equivalent:

```
/var/log/messages

```

---

### `/var/log/kern.log`

- Kernel messages:
    - Hardware issues
    - Drivers
    - Memory errors

---

### `/var/log/dmesg` or `dmesg`

- Kernel ring buffer (boot & hardware messages).
- Often copied into `kern.log`.

View:

```bash
dmesg | less
sudo tail -f /var/log/kern.log

```

---

### `/var/log/boot.log`

- Boot process messages.
- Useful for debugging service failures during startup.

---

## 📦 Package Manager & Update Logs

### Debian / Ubuntu

```
/var/log/apt/

```

- `history.log` → package install/remove history
- `term.log` → command output

### RHEL / CentOS / Fedora

```
/var/log/yum.log
/var/log/dnf.log

```

---

## ⏰ Cron, Scheduler & Automation Logs

### `/var/log/cron` or `/var/log/cron.log`

- Records cron job executions.
- Useful for:
    - Verifying jobs ran
    - Finding malicious scheduled tasks

---

## 🖥️ X / Display Logs

### `/var/log/Xorg.0.log`

- X server / GUI issues
- Display driver troubleshooting

---

## ✉️ Mail / MTA Logs

Common files:

- `/var/log/mail.log`
- `/var/log/mail.err`
- `/var/log/maillog`

Used to detect:

- Spam activity
- Open relays
- Compromised mail accounts

---

## 🌐 Web Server Logs

### Apache

- `/var/log/apache2/access.log`
- `/var/log/apache2/error.log`

### Nginx

- `/var/log/nginx/access.log`
- `/var/log/nginx/error.log`

📌 Used for:

- Traffic analysis
- SQL injection attempts
- Path traversal attacks
- Suspicious user agents

---

## 📁 Application Logs

- Usually under:

```
/var/log/<appname>/

```

Examples:

- `/var/log/mysql/`
- `/var/log/docker/`

Check service configs or unit files for exact paths.

---

## 🐳 Container & Docker Logs

- Docker:

```bash
docker logs <container>

```

- Raw container logs:

```
/var/lib/docker/containers/<id>/<id>-json.log

```

- Kubernetes:

```bash
kubectl logs <pod>

```

---

## 🛡️ Auditd, SELinux & AppArmor

### Auditd

```
/var/log/audit/audit.log

```

Tracks:

- User logins
- `sudo`
- File access (if configured)

Commands:

```bash
sudo ausearch -m USER_LOGIN
sudo aureport -ts today

```

---

### SELinux

- Logs denials in:
    - `/var/log/audit/audit.log`
    - `/var/log/messages`

---

### AppArmor

- Denials logged in:
    - `/var/log/syslog`
    - `/var/log/kern.log`

Check status:

```bash
aa-status

```

---

## 🚨 IDS & Protection Logs

- **fail2ban**:

```
/var/log/fail2ban.log

```

- **rkhunter / chkrootkit**
    - Logs under `/var/log/`
- **Firewalls**
    - iptables → `kern.log` / `messages`
    - nftables → similar locations

---

## 🌍 Web App / WAF / Proxy Logs

- ModSecurity:

```
/var/log/apache2/modsec_audit.log

```

- External WAFs (e.g., Cloudflare) provide dashboard logs.

---

## 🧪 Forensic & Other Logs

- `/var/log/faillog`
- `/var/log/daemon.log`
- `/var/log/user.log`
- `/var/log/installer/`

Used for audits and investigations.

---

## ⚡ Commands & Quick Checks

```bash
sudo tail -f /var/log/auth.log /var/log/syslog
sudo journalctl -b
sudo journalctl -b -1
sudo journalctl -u sshd.service --since "2 hours ago"
sudo grep -i "failed password" /var/log/auth.log
sudo grep -i "error\|segfault\|panic" /var/log/kern.log
last
lastb
lastlog
sudo journalctl --since "2025-01-01" | less

```

---

# 📘 journalctl — systemd Log Viewer

`journalctl` is used to **view logs collected by systemd-journald**.

> 🧾 journalctl = command to read logs from the systemd journal
> 

Logs stored in:

- Persistent:

```
/var/log/journal/

```

- Volatile:

```
/run/log/journal/

```

---

## ⚙️ Basic journalctl Usage

### View all logs

```bash
sudo journalctl

```

### Logs since boot

```bash
sudo journalctl -b
sudo journalctl -b -1

```

### Real-time logs

```bash
sudo journalctl -f

```

---

## 🔎 Filter by Service

```bash
sudo journalctl -u cron
sudo journalctl -u ssh
sudo journalctl -u NetworkManager
sudo journalctl -u cron -f

```

---

## ⏱️ Filter by Time

```bash
sudo journalctl --since yesterday
sudo journalctl --since "2025-10-27 15:00:00"
sudo journalctl --since "2025-10-27" --until "2025-10-28 10:00:00"

```

---

## 🚦 Filter by Priority

| Level | Meaning |
| --- | --- |
| 0–2 | Critical |
| 3 | Error |
| 4 | Warning |
| 6 | Info |

```bash
sudo journalctl -p err
sudo journalctl -p 3

```

---

## 🧹 Log Management (Careful)

```bash
sudo journalctl --disk-usage
sudo journalctl --vacuum-time=7d
sudo journalctl --vacuum-size=500M

```

---
