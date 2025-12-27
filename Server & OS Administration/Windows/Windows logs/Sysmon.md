# ✅ Sysmon (System Monitor) – Detailed Notes

*(Original content preserved exactly — additions marked clearly)*

---

Sysmon is a Windows system service / driver that logs detailed system activity (process creation, network connections, image loads, file time changes, driver install, etc.) to the Windows Event Log (`Applications and Services Logs → Microsoft → Windows → Sysmon → Operational`). It’s a defensive tool used for detection and forensics.

➕ **Added (Why Sysmon is important):**

- Provides **high-fidelity telemetry** not available in default Windows logs
- Widely used by **SOC analysts, DFIR teams, threat hunters**
- Helps detect:
    - Malware execution
    - Lateral movement
    - Persistence mechanisms
    - Command-and-control (C2)

---

## 1) Download Sysmon

Official source: Microsoft Sysinternals.

Common pages:

- `https://learn.microsoft.com/sysinternals/downloads/sysmon`
- (`https://docs.microsoft.com/sysinternals/...` — search “Sysmon Sysinternals”)

If you want to download via PowerShell (example — run as Administrator in test VM), replace `<URL>` with the official Microsoft Sysmon zip URL if you have it:

```powershell
# create a temp folder
$td = "$env:TEMP\sysmon_install"; New-Item -ItemType Directory -Path $td -Force

# download Sysmon zip (replace <SYS_MON_ZIP_URL> with official Sysinternals link)
$zipUrl = "<SYS_MON_ZIP_URL>"
Invoke-WebRequest -Uri $zipUrl -OutFile "$td\sysmon.zip"

# unzip
Expand-Archive -Path "$td\sysmon.zip" -DestinationPath $td -Force

# list files
Get-ChildItem $td

```

> If you prefer, manually download the Sysmon zip from the Microsoft Sysinternals page and copy the Sysmon.exe/Sysmon64.exe to your VM.
> 

➕ **Added (Files you will see after unzip):**

- `Sysmon.exe` (32-bit)
- `Sysmon64.exe` (64-bit)
- `Eula.txt`

---

## 2) Install Sysmon (basic)

Open an **elevated PowerShell (Run as Administrator)** or an elevated Command Prompt.

**Install with a config file** (recommended). Sysmon accepts an XML config during install. Example command:

```powershell
# From folder where Sysmon64.exe is located
.\Sysmon64.exe -i C:\path\to\sysmon-config.xml -accepteula

```

Options:

- `i <config>` : install service and load configuration
- `u` : uninstall Sysmon
- `c <config>` : update configuration (without reinstall)
- `s` : show current config
- `accepteula` : auto-accept EULA for automation

If you just run without config, Sysmon installs with default minimal rules — not ideal for detection.

➕ **Added (Best practice):**

- Always use a config
- Never deploy Sysmon without filtering rules (log noise + performance)

---

## 3) Recommended first config (compact, high value)

Below is a **compact Sysmon config** that covers high-value events (process creation, network connect, image loads for DLLs, pipe creation, driver loads, file create time changes, registry events, service install). Save as `C:\sysmon\sysmon-config.xml`.

```xml
<Sysmon schemaversion="4.80">
  <!-- Capture process creation with command line -->
  <EventFiltering>
    <ProcessCreate onmatch="include">
      <CommandLine condition="is not">""</CommandLine>
    </ProcessCreate>

    <!-- Network connections -->
    <NetworkConnect onmatch="include"/>

    <!-- Capture image loads (DLLs) to detect suspicious DLL injection / LSA) -->
    <ImageLoad onmatch="include" />

    <!-- Capture driver/Service install -->
    <DriverLoad onmatch="include" />
    <ServiceInstall onmatch="include" />

    <!-- File create times changed — sometimes used by timestomping -->
    <FileCreateTime onmatch="include" />

    <!-- Named pipe events -->
    <PipeCreate onmatch="include" />

    <!-- Registry changes for persistence (common keys) -->
    <RegistryEvent onmatch="include">
      <TargetObject condition="begin with">HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run</TargetObject>
      <TargetObject condition="begin with">HKCU\Software\Microsoft\Windows\CurrentVersion\Run</TargetObject>
    </RegistryEvent>

    <!-- Optional: process termination -->
    <ProcessTerminate onmatch="include" />
  </EventFiltering>
</Sysmon>

```

Notes:

- This is compact for learning.
- Production setups often use large community-maintained XMLs (SwiftOnSecurity, Sigma-mapped configs).
- `schemaversion` depends on Sysmon version.

➕ **Added (Why this config is “high value”):**

- Captures **execution**
- Captures **persistence**
- Captures **network activity**
- Captures **evasion techniques (timestomp, DLL load)**

---

## 4) Install with the config

Assuming `Sysmon64.exe` and `C:\sysmon\sysmon-config.xml` exist:

```powershell
cd C:\sysmon
.\Sysmon64.exe -i .\sysmon-config.xml -accepteula

# verify service installed
sc.exe query sysmon

```

Check the Sysmon event channel:

- Event Viewer → Applications and Services Logs → Microsoft → Windows → Sysmon → Operational

Or via PowerShell:

```powershell
Get-WinEvent -ListLog "*Sysmon*"
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 20 |
Format-Table TimeCreated, Id, Message -AutoSize

```

➕ **Added (Expected result):**

- Event ID **4** → Sysmon service started
- Event ID **1** → Process creation logs begin appearing

---

## Event ID:

| Event ID | Description |
| --- | --- |
| 1 | Process Creation |
| 2 | File Creation Time Changed |
| 3 | Network Connection Detected |
| 4 | Sysmon Service State Changed |
| 5 | Process Terminated |
| 6 | Driver Loaded |
| 7 | Image Loaded |
| 8 | Create Remote Thread |
| 9 | RawAccessRead |
| 10 | Process Accessed |
| 11 | File Created |
| 12 | Registry Value Set |
| 13 | Registry Value Deleted |
| 14 | Registry Key Deleted |
| 15 | File Create Stream Hash |
| 17 | Named Pipe Created |
| 18 | Named Pipe Connected |
| 19 | WMI Filter Event Activity |
| 20 | DNS Query Logged |
| 21 | File Deleted |
| 22 | Clipboard Change |
| 23 | Process Tampering Detected |

➕ **Added (High-risk IDs for detection):**

- **1, 3, 7, 8, 10, 11, 19, 20, 23**

---

## Sysmon Configuration ….

Perfect! Here’s a **full Sysmon config XML** covering **all main event IDs (1 to 23 except 16)** with basic include rules so you capture everything important. This is a solid base for learning and monitoring:

```xml
<Sysmon schemaversion="4.70">
  <HashAlgorithms>md5,sha256,sha1</HashAlgorithms>

  <EventFiltering>
    <ProcessCreate onmatch="include" />
    <FileCreateTime onmatch="include" />
    <NetworkConnect onmatch="include" />
    <ServiceStateChange onmatch="include" />
    <ProcessTerminate onmatch="include" />
    <DriverLoad onmatch="include" />
    <ImageLoad onmatch="include" />
    <CreateRemoteThread onmatch="include" />
    <RawAccessRead onmatch="include" />
    <ProcessAccess onmatch="include" />
    <FileCreate onmatch="include" />
    <RegistryValueSet onmatch="include" />
    <RegistryValueDeleted onmatch="include" />
    <RegistryKeyDeleted onmatch="include" />
    <FileCreateStreamHash onmatch="include" />
    <NamedPipeCreated onmatch="include" />
    <NamedPipeConnected onmatch="include" />
    <WmiEventFilter onmatch="include" />
    <DnsQuery onmatch="include" />
    <FileDelete onmatch="include" />
    <ClipboardChange onmatch="include" />
    <ProcessTampering onmatch="include" />
  </EventFiltering>
</Sysmon>

```

---

### How to use this:

1. Save XML as `sysmon-config.xml`
2. Open **elevated command prompt**
3. Install:

```bash
Sysmon64.exe -accepteula -i sysmon-config.xml

```

1. Update existing config:

```bash
Sysmon64.exe -c sysmon-config.xml

```

1. Verify:

```bash
Sysmon64.exe -s
Sysmon64.exe -m
sc query sysmon
get-service sysmon

```

---

Event Viwere / applicatin & serive logs / microsoft / windows / sysmon / operational

( log path )

➕ **Added (Exact log path):**

```
Event Viewer
→ Applications and Services Logs
→ Microsoft
→ Windows
→ Sysmon
→ Operational

```
