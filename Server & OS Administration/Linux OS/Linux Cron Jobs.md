---

## 📌 1. What Is a Cron Job?

A **cron job** is an automated task scheduled to run at specific intervals on Unix/Linux systems.

It’s like an **alarm clock for your operating system** — you define *when* and *how often* something runs, and Linux executes it automatically.

Cron jobs are managed by a background service called **`cron`**, and tasks are defined inside **crontab files** (*cron table*).

---

### 📌 Common Use Cases

- Automatically update the system
- Run backup scripts
- Clean temporary files
- Send logs or reports
- Schedule Python or shell scripts

---

## ⚙️ 2. Check If the Cron Service Is Running

Before creating cron jobs, ensure the **cron daemon** is active.

```bash
sudo systemctl status cron

```

If not running:

```bash
sudo systemctl start cron
sudo systemctl enable cron

```

📌 On some systems (CentOS/RHEL):

```bash
sudo service crond status

```

---

## 🧰 3. Understanding Crontab (Cron Table)

Each user (including **root**) has a **separate crontab file**.

### Edit Your Crontab

```bash
crontab -e

```

### View or Remove Jobs

```bash
crontab -l
crontab -r   # ⚠️ removes all jobs

```

---

## 🕐 4. Cron Job Syntax (Very Important)

Each line in a crontab represents **one scheduled job**.

```
MIN HOUR DOM MON DOW COMMAND

```

| Field | Meaning | Range |
| --- | --- | --- |
| MIN | Minute | 0–59 |
| HOUR | Hour | 0–23 |
| DOM | Day of Month | 1–31 |
| MON | Month | 1–12 |
| DOW | Day of Week | 0–6 |
| COMMAND | Command or script | — |

---

### 🧠 Common Examples

| Schedule | Crontab Entry |
| --- | --- |
| Every minute | `* * * * *` |
| Every 5 minutes | `*/5 * * * *` |
| Daily at 2 AM | `0 2 * * *` |
| Every Monday 8 AM | `0 8 * * 1` |
| On reboot | `@reboot` |

---

## 📘 5. First Cron Job (Testing)

### Step 1: Edit Crontab

```bash
crontab -e

```

### Step 2: Add Job

```
* * * * * echo "Cron test at $(date)" >> /tmp/cron_test.log

```

### Step 3: Verify

```bash
cat /tmp/cron_test.log

```

✅ If timestamps appear, cron is working.

---

## 🧩 6. Important Cron Concepts

### 🧱 Minimal Environment

- Cron does **not** load your shell environment
- Always use **absolute paths**

Example:

```
* * * * * /usr/bin/python3 /home/user/script.py

```

---

### 📜 Redirect Output & Errors

```
* * * * * /path/to/script.sh >> /var/log/script.log 2>&1

```

- `>>` → append output
- `2>&1` → capture errors

---

### 📅 Special Keywords

| Keyword | Meaning |
| --- | --- |
| `@reboot` | Run at startup |
| `@hourly` | Every hour |
| `@daily` | Every day |
| `@weekly` | Weekly |
| `@monthly` | Monthly |

---

## 🧠 7. Real Example: Daily System Update

### Step 1: Create Script

```bash
sudo nano /usr/local/bin/daily_update.sh

```

```bash
#!/bin/bash
echo "===== $(date) Starting update =====" >> /var/log/daily_update.log
apt update -y >> /var/log/daily_update.log 2>&1
echo "===== $(date) Update complete =====" >> /var/log/daily_update.log

```

Make executable:

```bash
sudo chmod +x /usr/local/bin/daily_update.sh

```

---

### Step 2: Schedule Job

```bash
crontab -e

```

```
0 3 * * * /usr/local/bin/daily_update.sh

```

🕒 Runs daily at **3 AM**

---

### Step 3: Test Manually

```bash
sudo /usr/local/bin/daily_update.sh
cat /var/log/daily_update.log

```

---

## 📚 8. Managing Multiple Cron Jobs

Example crontab:

```
0 3 * * * /usr/local/bin/daily_update.sh
0 1 * * * /usr/local/bin/daily_backup.sh
0 2 * * 0 /usr/local/bin/clean_temp.sh
0 * * * * echo "$(date) - $(uptime)" >> /var/log/uptime.log
@reboot echo "System rebooted at $(date)" >> /var/log/reboot.log

```

Check jobs:

```bash
crontab -l

```

---

## 🧩 9. System-Wide Cron Jobs

| Location | Purpose |
| --- | --- |
| `/etc/crontab` | Global cron config |
| `/etc/cron.hourly/` | Hourly jobs |
| `/etc/cron.daily/` | Daily jobs |
| `/etc/cron.weekly/` | Weekly jobs |
| `/etc/cron.monthly/` | Monthly jobs |

Example:

```bash
sudo nano /etc/cron.daily/check_disk.sh

```

```bash
#!/bin/bash
df -h > /var/log/disk_usage.log

```

```bash
sudo chmod +x /etc/cron.daily/check_disk.sh

```

---

## 🔧 10. Debugging Cron Jobs

1️⃣ Check logs:

```bash
grep CRON /var/log/syslog
journalctl -u cron

```

2️⃣ Check permissions:

```bash
chmod +x script.sh

```

3️⃣ Use full paths

4️⃣ Redirect output

5️⃣ Remember cron uses `/bin/sh`

---

## 🧮 11. Check Next Run Time (Optional)

```bash
sudo apt install cron-utils
cronsched "0 3 * * *"

```

---

## 🎯 12. Exam / Interview Summary

- Cron automates Linux tasks
- Uses **crontab**
- Five time fields + command
- Uses **absolute paths**
- Logs are essential for debugging
- Supports user-level & system-wide jobs

---

## 🧠 Bonus Practice (Kept)

1. Email daily disk usage
2. Python script every 15 minutes
3. Weekly log archive
4. Use `@reboot` logging

---
