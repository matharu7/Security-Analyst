Primary ETW/Windows Event Log files (binary `.evtx`) live here:

```
C:\Windows\System32\winevt\Logs\
  - Security.evtx
  - System.evtx
  - Application.evtx
  - Setup.evtx
  - Windows PowerShell.evtx
  - ForwardedEvents.evtx
  - ...other app/service logs...

```

Do **not** edit these files in place. Export copies for analysis.

---

## Main Windows event channels (what to check)

### Security (audit) — `Security.evtx`

Security‑relevant events: logons, account management, privilege use, policy changes, audit log clears.

Common Event IDs you noted (and what they mean):

- **4624** — Successful logon
- **4625** — Failed logon (bad credentials)
- **4634** — An account was logged off
- **4647** — User‑initiated logoff
- **4648** — A logon was attempted using explicit credentials (alternate credentials)
- **4768** — Kerberos Authentication (TGT requested)
- **4776** — NTLM authentication attempt/validation
- **4688** — New process created (very useful for process‑creation auditing)
- **4689** — Process exited
- **4720** — User account was created
- **4726** — User account was deleted
- **4732 / 4728 / 4733** — Group membership changes (member added/removed to groups; 4728/4732 variants for scope)
- **4697** — A service was installed
- **4719 / 4739** — Audit / security policy changes (audit policy or group policy changes)
- **4723** — Attempt to change user password
- **4724** — Reset password (by privileged user)
- **1102** — The audit log was cleared (critical — indicates potential coverup)

**Why Security logs matter:** they show who did what and when (logons, privilege escalation, account lifecycle, process execution events if auditing enabled).

---

### System — `System.evtx`

System‑level events: drivers, services, hardware, kernel, shutdowns.

Useful Event IDs:

- **1074** — A process (or user) initiated a shutdown or restart
- **6006** — Event Log service stopped (clean shutdown)
- **6008** — Unexpected shutdown (unclean)
- **41** (Kernel‑Power) — System rebooted without clean shutdown (power loss, crash)
- **55** — NTFS file system structure corruption (serious disk/FS errors)

System logs are key for diagnosing crashes, hardware failures, unexpected reboots, or service crashes.

---

### Application — `Application.evtx`

Application-level errors, warnings, crashes from user‑level apps and frameworks.

Common IDs:

- **1000** — Application Error (app crash)
- **1001** — Windows Error Reporting (WER) report entry
- **1026** — .NET Runtime exception (legacy; newer .NET versions use different event IDs)

Use these logs to pinpoint app failures and stack traces (WER uploads may include dump metadata).

---

### Setup / Windows Update — `Setup.evtx`, WindowsUpdateClient

- Setup events (OS install, major updates, feature installs) appear in `Setup.evtx` and Windows Update provider logs.
- Use `Get-WindowsUpdateLog` (legacy) or check `Event Viewer → Applications and Services Logs → Microsoft → Windows → WindowsUpdateClient` for update install/start/failure events.

---

### Forwarded Events — `ForwardedEvents.evtx`

If you use Windows Event Forwarding (WEF), remote systems’ forwarded events are collected here.

---

## Quick investigations — sample hunts

1. **Multiple failed logons then success**
    - Search `4625` (failed) + correlate to `4624` (success) for same account/IP/time window.
    - Use `Event ID 4625` Message → check `IpAddress` field (if logon type indicates network).
2. **Suspicious new admin user**
    - Check `4720` (user created) and `4728/4732` (added to groups), `4726` (deleted) for unexpected activity.
3. **Process-based persistence**
    - Search `4688` for suspicious process names or parent process mismatches (e.g., `cmd.exe` spawned from `explorer.exe` at odd times).
4. **Audit log clearing (possible coverup)**
    - `1102` indicates audit log cleared. Immediately preserve logs and investigate who issued the clear and any preceding suspicious events.
5. **Service or driver install**
    - `4697` (service installed) or kernel drivers in System logs — investigate file paths, signatures, and when installed.
6. **Unusual shutdowns / kernel power**
    - `6008` or `41` — find preceding events in System and Application for crash source (BSOD code, driver names).

---

## Log forwarding & centralization

- Native Windows Event Forwarding (WEF) — forward events from clients to a collector (Event Collector) with subscriptions.
    - Collector stores forwarded events in `ForwardedEvents.evtx`.
    - Use WinRM and HTTPS for secure transport.
- Log shippers: **Winlogbeat** (Elastic), **NXLog**, **Fluentd**, **Syslog‑NG** agents — forward logs to SIEM/ELK/Splunk/Cloud logging.

**Recommendation:** forward Security, System, Application, and relevant AD/Domain logs to central SIEM with TLS and RBAC. Keep retention per policy.

---

# Windows Event Log IDs — Reference Notes

---

## Windows Setup Log Events

| Event ID | Description |
| --- | --- |
| 2 | Windows update installed |
| 4 | Setup started |
| 100 | Installation of a Windows feature |
| 200 | Installation succeeded |
| 300 | Installation failed |
| 101 | Setup operation completed |
| 102 | Setup rollback started |
| 103 | Setup rollback completed |
| 400 | Driver installation started |
| 401 | Driver installation succeeded |
| 402 | Driver installation failed |

---

## Security Log Events (Audit & Authentication)

| Event ID | Description |
| --- | --- |
| 4624 | Successful account logon |
| 4625 | Failed account logon |
| 4634 | Account logoff |
| 4647 | User initiated logoff |
| 4672 | Special privileges assigned to new logon |
| 4688 | New process created (process creation) |
| 4689 | Process exited |
| 4697 | A service was installed |

| Event ID | Description |
| --- | --- |
| 4720 | User account created |
| 4722 | User account enabled |
| 4723 | Attempt to change an account password |
| 4724 | Attempt to reset an account password |
| 4725 | User account disabled |
| 4726 | User account deleted |
| 4732 | Added to security-enabled local group |
| 4733 | Removed from security-enabled local group |
| 4740 | Account locked out |
| 4768 | Kerberos authentication ticket requested |
| 4769 | Kerberos service ticket requested |
| 4771 | Kerberos pre-authentication failed |
| 4776 | Domain controller attempted logon |
| 4781 | Account name changed |
| 4798 | User’s local group membership enumerated |
| 4800 | Workstation locked |
| 4801 | Workstation unlocked |
| 4816 | RPC detected an integrity violation |
| 4902 | Per-user audit policy table created |
| 4904 | Attempt to register a security event source |
| 4912 | Per-user audit policy changed |
| 4627 | Group membership enumerated |
| 1102 | Audit log cleared |

---

## Process & Service Events

| Event ID | Description |
| --- | --- |
| 4688 | Process creation (includes process name, PID) |
| 4689 | Process termination |
| 4697 | Service installed |
| 7045 | Service installed (System log) |
| 7036 | Service state changed (started, stopped) |
| 7000 | Service failed to start |
| 7001 | Service failed due to dependency failure |
| 7040 | Service start type changed |
| 4698 | Scheduled task created |
| 4699 | Scheduled task deleted |
| 4700 | Scheduled task enabled |
| 4701 | Scheduled task disabled |
| 4702 | Scheduled task updated |

---

## Network & Firewall Events

| Event ID | Description |
| --- | --- |
| 4946 | Windows Firewall settings changed |
| 4947 | Windows Firewall settings restored to default |
| 4948 | A Windows Firewall rule was added |
| 4949 | A Windows Firewall rule was modified |
| 4950 | A Windows Firewall rule was deleted |
| 4952 | Windows Firewall exception list changed |
| 4954 | Windows Firewall allowed a connection |
| 4956 | Windows Firewall blocked a connection |
| 4958 | Windows Firewall allowed a connection (Outbound) |
| 4960 | Windows Firewall blocked a connection (Outbound) |
| 5156 | Windows Filtering Platform permitted a connection |
| 5158 | Windows Filtering Platform blocked a connection |
| 5031 | Windows Firewall service was stopped |
| 5032 | Windows Firewall service was started |

---

## System Events (Expanded)

| Event ID | Description |
| --- | --- |
| 6005 | Event Log service started – system startup marker |
| 6006 | Event Log service stopped |
| 6008 | Unexpected shutdown – crash or power loss |
| 6009 | OS version logged during startup |
| 6013 | System uptime |
| 7000 | Service failed to start |
| 7001 | Service failed due to dependency |
| 7022 | Service hung on starting |
| 7034 | Service terminated unexpectedly |
| 7035 | Service control command sent (stop/start) |
| 7036 | Service entered a state (started/stopped) |
| 7040 | Service start type changed |
| 7045 | New service installed |
| 8003 | Network drive disconnected (browser service) |
| 41 | Kernel power loss (critical crash indicator) |
| 55 | NTFS corruption detected |

---

## Application Events (Expanded)

| Event ID | Description |
| --- | --- |
| 1000 | Application Error (crash, faulty module info) |
| 1001 | Windows Error Reporting (includes dump path) |
| 1002 | Application hang |
| 1004 | Application popup or error dialog |
| 1026 | .NET Runtime Error – unhandled exceptions |
| 1309 | ASP.NET Application Error (from IIS) |
| 11707 | MSI: Product installation success |
| 11708 | MSI: Installation failed |
| 11724 | MSI: Product removal success |
| 9020 | COM application error (DCOM) |
| 9031 | Application failed to start due to missing component |
