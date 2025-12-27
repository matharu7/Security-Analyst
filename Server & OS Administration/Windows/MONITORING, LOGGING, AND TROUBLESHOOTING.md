# **Event Viewer & Logs**

---

## Topics Covered

- Understanding Application, System, and Security logs
- Creating Custom Views and using filters for efficient analysis
- Setting up Log Forwarding for centralized monitoring

---

## 1️⃣ **Windows Event Logs — Overview**

---

### Types of Logs

| Log Type | Description | Common Use Cases |
| --- | --- | --- |
| **Application** | Events logged by applications or programs (e.g., IIS, SQL Server, custom apps) | Troubleshooting app errors, crashes |
| **System** | Logged by Windows OS components and drivers | Diagnosing OS/hardware issues |
| **Security** | Audit logs related to login events, resource access, and policy changes (requires auditing enabled) | Tracking user authentications, permission changes, security breaches |

---

### Event Levels (Severity)

| Level | Description |
| --- | --- |
| **Critical** | Severe errors requiring immediate attention |
| **Error** | Significant problems that might cause issues |
| **Warning** | Potential issues or alerts |
| **Information** | Normal operations and events |
| **Verbose** | Detailed tracing info |

---

### Accessing Event Viewer (GUI)

- Press **Windows + R**, type `eventvwr.msc`, press Enter.
- Expand **Windows Logs** → select **Application**, **System**, or **Security**.
- Double-click an event to view details.

---

## 2️⃣ **Custom Views & Filtering**

---

### Why Use Custom Views?

- Quickly access important or frequent event types.
- Filter by Event ID, Level, Source, Date, User, or Computer.
- Saves time during troubleshooting and monitoring.

---

### Creating a Custom View

---

**Step-by-Step GUI Guide:**

1. Open **Event Viewer**.
2. Right-click **Custom Views** → **Create Custom View**.
3. Choose time range and event levels (e.g., Errors and Warnings).
4. Select log(s) to include (Application, System, Security, or others).
5. Add filters:
    - Event IDs (e.g., 4624 for successful logon in Security log).
    - Source (e.g., “Microsoft-Windows-Security-Auditing”).
6. Name the view and save it.

---

### Using Filters in Event Viewer

- Select any log (e.g., Application).
- Click **Filter Current Log** on the right pane.
- Apply filters like event levels, event sources, keywords, or user accounts.
- This narrows down event lists for focused troubleshooting.

---

### Exporting Logs and Views

- Right-click on any log or custom view.
- Select **Save All Events As…** (.evtx format).
- Useful for sharing or archiving.

---

## 3️⃣ **Log Forwarding & Centralized Monitoring**

---

### Why Forward Logs?

- Aggregate logs from multiple servers for centralized analysis.
- Improve security monitoring and compliance.
- Simplify incident response.

---

### Windows Event Forwarding (WEF) Setup

---

### Components

- **Source Computers**: Servers/workstations sending event logs.
- **Collector Computer**: Central server receiving and storing logs.

---

### Step 1: Configure the Collector Server

- Open **Event Viewer** → **Subscriptions** node.
- Click **Create Subscription**.
- Choose **Collector initiated** (source computers initiate sending logs).
- Define subscription name and description.
- Add computers (source servers) or use Active Directory groups.
- Choose events to collect (e.g., Security, Application, System).
- Set destination log on collector (e.g., ForwardedEvents).

---

### Step 2: Configure Source Computers

- On each source computer:

```powershell
# Enable WinRM service
Enable-PSRemoting -Force

# Configure WinRM to allow the collector computer to connect
Set-Item wsman:\localhost\Client\TrustedHosts -Value "CollectorComputerName"

# Start and configure Windows Event Collector service
Set-Service -Name Wecsvc -StartupType Automatic
Start-Service -Name Wecsvc

```

- Use Group Policy for large environments:

```
Computer Configuration → Policies → Administrative Templates → Windows Components → Event Forwarding

```

---

### Step 3: Verify Logs

- On the collector server, check the **ForwardedEvents** log for incoming events.
- Use Event Viewer or PowerShell to analyze:

```powershell
Get-WinEvent -LogName ForwardedEvents | Format-Table -AutoSize

```

---

## 4️⃣ **PowerShell for Event Logs**

---

### Query Events

```powershell
# Get latest 20 errors from System log
Get-WinEvent -LogName System -FilterXPath "*[System/Level=2]" -MaxEvents 20

```

---

### Export Logs

```powershell
# Export Security log to EVTX file
wevtutil epl Security C:\Logs\SecurityLog.evtx

```

---

### Clear Logs

```powershell
Clear-EventLog -LogName Application, System

```

---

## 5️⃣ **Best Practices for Event Log Management**

- Enable **auditing policies** (e.g., account logon, object access) to generate relevant Security logs.
- Regularly **archive and clear logs** to prevent disk space issues.
- Use **custom views** and filters to focus on critical events.
- Implement **log forwarding** for centralized SIEM integration.
- Monitor **Event Log size** and configure **retention policies** (overwrite as needed).
- Integrate logs with security tools like **Microsoft Defender for Endpoint**, **Splunk**, or **ELK Stack**.

---

## Additional Resources & Tools

| Resource / Tool | Purpose | Link |
| --- | --- | --- |
| Microsoft Docs on Event Viewer | Official guidance | https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-logs-and-event-viewer |
| Wevtutil Tool | CLI for managing event logs | https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/wevtutil |
| Windows Event Forwarding Setup | Detailed WEF setup guide | https://learn.microsoft.com/en-us/windows/security/threat-protection/use-windows-event-forwarding-to-assist-in-intrusion-detection |
| PowerShell Cmdlets for Event Logs | Automate log management | https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-winevent |

---

# Summary Table

| Feature | Description | How to Use |
| --- | --- | --- |
| Application Logs | Logs from software/apps | Event Viewer → Windows Logs → Application |
| System Logs | OS and hardware events | Event Viewer → Windows Logs → System |
| Security Logs | Audit events (requires enabled auditing) | Event Viewer → Windows Logs → Security |
| Custom Views | Filtered, saved event views | Event Viewer → Custom Views → Create New |
| Log Forwarding | Centralized log collection | Configure Subscriptions in Event Viewer + Configure source clients |

# ⚙️ **16. Performance Monitoring**

---

## Topics Covered

- Task Manager & Resource Monitor overview
- In-depth use of Performance Monitor (Perfmon)
- Creating & managing Data Collector Sets (DCS) for performance logging and analysis

---

## 1️⃣ Task Manager & Resource Monitor — Quick Insight Tools

---

### Task Manager

- **Launch**: Ctrl + Shift + Esc or right-click taskbar → Task Manager.
- Tabs useful for performance monitoring:
    - **Processes**: View CPU, memory, disk, and network usage per process.
    - **Performance**: Real-time graphs for CPU, Memory, Disk, Network, GPU.
    - **App history**: Resource usage history per app.
    - **Details**: Detailed process info with PID and status.
    - **Services**: Check running services.

---

### Advanced Tips

- Right-click column headers in **Processes** tab → Add columns like **I/O Reads/Writes**, **Power Usage** for deeper insight.
- **Resource Monitor** button in Performance tab opens advanced resource analysis tool.

---

### Resource Monitor

- Launch via Task Manager → Performance tab → **Open Resource Monitor**, or run `resmon.exe`.
- Provides detailed real-time info for:
    - CPU (process threads, services)
    - Memory (hard faults, usage by processes)
    - Disk (per process disk activity, storage queues)
    - Network (per process network activity, TCP connections)

---

### Practical Use Cases

- Identify processes hogging CPU, memory, disk, or network.
- Troubleshoot system slowdowns by pinpointing resource bottlenecks.
- Monitor disk latency or network bandwidth usage per application.

---

## 2️⃣ Performance Monitor (Perfmon) — Deep & Custom Monitoring

---

### What is Perfmon?

- A powerful, built-in tool to monitor system and application performance using counters.
- Supports real-time graphing, alerts, and data logging.
- Can track hundreds of counters like CPU usage, disk I/O, network throughput, memory pressure, and much more.

---

### Launching Perfmon

- Run `perfmon.exe` or via Server Manager → Tools → Performance Monitor.

---

### Understanding Perfmon Interface

| Section | Purpose |
| --- | --- |
| **Performance Monitor** | Real-time graph display of selected counters |
| **Data Collector Sets** | Configurations to record performance data over time |
| **Reports** | View historical performance logs |

---

### Adding Counters

---

**Step-by-step:**

1. In the left pane, click **Performance Monitor**.
2. Click the green **+** icon (Add Counters).
3. In the **Add Counters** dialog:
    - Select counters from the list (e.g., **Processor → % Processor Time**).
    - Choose the specific instance(s) if available (e.g., *Total*, or individual CPU cores).
    - Click **Add** → **OK**.

---

### Useful Counters to Monitor

| Category | Counter | Why Monitor |
| --- | --- | --- |
| Processor | % Processor Time | CPU utilization |
| Memory | Available MBytes | Free RAM |
| Memory | Pages/sec | Paging activity |
| PhysicalDisk | Avg. Disk Queue Length | Disk latency |
| Network Interface | Bytes Total/sec | Network throughput |
| System | Processor Queue Length | Threads waiting for CPU |
| Process | % Processor Time (per process) | Identify resource-heavy apps |

---

### Customizing Graph View

- Right-click graph → Properties.
- Adjust colors, scale, update interval (default 1000ms).
- Switch between line, histogram, or report view for analysis.

---

## 3️⃣ Data Collector Sets (DCS) — Automated Performance Data Collection

---

### Why Use Data Collector Sets?

- Automate performance data capture over time.
- Useful for diagnostics, trend analysis, and troubleshooting intermittent issues.
- Can combine Performance Counters, Event Trace Data, and System Configuration info.

---

### Creating a Data Collector Set (GUI)

---

**Step-by-step:**

1. Open **Performance Monitor**.
2. Expand **Data Collector Sets** → **User Defined**.
3. Right-click **User Defined** → **New** → **Data Collector Set**.
4. Name it (e.g., `CPU_And_Disk_Monitor`), choose **Create manually (Advanced)** → Next.
5. Choose which data to collect:
    - **Performance counter**
    - **Event trace data**
    - **System configuration data**
6. For **Performance counters**:
    - Click **Add…**
    - Select required counters (e.g., Processor → % Processor Time, PhysicalDisk → Avg. Disk sec/Transfer).
    - Click **OK**, then **Next**.
7. Choose where to save logs.
8. Select to run the data collector set now or save for later.
9. Finish.

---

### Starting and Stopping DCS

- Right-click your Data Collector Set → **Start**.
- To stop: right-click → **Stop**.

---

### Viewing Reports

- After stopping the DCS, reports are generated under:
    
    ```
    Reports → User Defined → Your Data Collector Set name
    
    ```
    
- Review graphs and statistics to analyze trends and bottlenecks.

---

### Scheduling Data Collector Sets

- Right-click DCS → Properties → **Schedule** tab.
- Create schedules to start/stop data collection automatically (e.g., during peak hours).

---

### Exporting and Sharing Logs

- Data Collector Sets save logs in `.blg` (binary) format.
- Use **Performance Monitor** to open and analyze `.blg` files on any Windows machine.
- Convert `.blg` to `.csv` for external analysis with:

```powershell
# Example to convert .blg to csv
relpadmin.exe /convert /input:"C:\PerfLogs\YourLog.blg" /output:"C:\PerfLogs\YourLog.csv"

```

---

## 4️⃣ PowerShell Commands for Performance Monitoring

---

### Get CPU usage

```powershell
Get-Counter '\Processor(_Total)\% Processor Time'

```

---

### Collect performance data and save to file

```powershell
# Define counters
$counters = '\Processor(_Total)\% Processor Time', '\Memory\Available MBytes'

# Collect for 30 seconds, sampling every 5 seconds
Get-Counter -Counter $counters -SampleInterval 5 -MaxSamples 6 | Export-Counter -Path "C:\PerfLogs\PerfmonOutput.blg"

```

---

### Query Performance Logs

```powershell
# Read a counter log
Import-Counter -Path "C:\PerfLogs\PerfmonOutput.blg" | Format-Table -AutoSize

```

---

## 

---

## Additional Resources & Tools

| Tool / Doc | Purpose | Link |
| --- | --- | --- |
| Microsoft Perfmon Docs | Official documentation | https://learn.microsoft.com/en-us/windows-server/administration/performance-tuning/ |
| Windows Sysinternals Process Explorer | Advanced process and resource monitoring | https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer |
| PowerShell Get-Counter | Command-line performance data collection | https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/get-counter |
| Performance Monitor Blog (MS Tech Community) | Deep dive tutorials | https://techcommunity.microsoft.com/t5/performance-monitor/ct-p/PerformanceMonitor |

# 🛠️ **17. Windows Troubleshooting — Deep & Practical Guide**

---

## Topics Covered

- BSOD & Crash Dump analysis
- System repair tools: SFC, DISM, chkdsk, MSConfig
- Using Services.msc and Device Manager for troubleshooting

---

## 1️⃣ BSOD (Blue Screen of Death) & Crash Dump Analysis

---

### What is BSOD?

- A critical system error causing Windows to stop completely.
- Generates a **crash dump file** (.dmp) for post-mortem debugging.
- Common causes: faulty drivers, hardware errors, corrupted system files.

---

### Locate Crash Dump Files

- Usually stored in:
    
    ```
    C:\Windows\Minidump\
    C:\Windows\MEMORY.DMP
    
    ```
    

---

### Analyzing Crash Dumps

---

### Tools to Use:

- **Windows Debugger (WinDbg)** from Windows SDK
- **BlueScreenView** (NirSoft) — simpler GUI alternative
- **WhoCrashed** — easy crash dump analyzer

---

### Using WinDbg (Advanced):

1. Download and install **WinDbg** from the [Windows SDK](https://learn.microsoft.com/en-us/windows-hardware/drivers/debugger/).
2. Open WinDbg, then **File → Open Crash Dump**.
3. Load `MEMORY.DMP` or `.dmp` files from Minidump folder.
4. Run command `!analyze -v` to get detailed error analysis.
5. Check driver/module info, bug check code, and probable causes.

---

### Using BlueScreenView:

1. Download [BlueScreenView](https://www.nirsoft.net/utils/blue_screen_view.html).
2. It automatically loads dump files.
3. Displays faulty driver/module causing the crash.
4. Useful for quick analysis without steep learning curve.

---

## 2️⃣ System Repair Tools

---

### SFC (System File Checker)

- Scans and repairs corrupted Windows system files.

---

**Run SFC:**

```powershell
sfc /scannow

```

**Steps (GUI/Command Prompt):**

1. Open Command Prompt as Administrator.
2. Type `sfc /scannow` and press Enter.
3. Wait for scan to complete; review results.

---

### DISM (Deployment Image Servicing and Management)

- Repairs the Windows system image used by SFC.

---

**Run DISM:**

```powershell
DISM /Online /Cleanup-Image /RestoreHealth

```

**Steps:**

1. Open Admin Command Prompt.
2. Run above command.
3. After DISM completes, run `sfc /scannow` again for full repair.

---

### CHKDSK (Check Disk)

- Checks and repairs disk errors.

---

**Run CHKDSK:**

```powershell
chkdsk C: /f /r /x

```

- `/f` fixes errors, `/r` locates bad sectors, `/x` forces volume dismount.
- Requires reboot if system drive is in use.

---

### MSConfig (System Configuration)

- Manage startup programs, services, and boot options.

---

**Steps:**

1. Press **Windows + R**, type `msconfig`, Enter.
2. **General tab**: Choose Normal, Diagnostic, or Selective startup.
3. **Boot tab**: Enable Safe Boot, set timeout.
4. **Services tab**: Enable/disable services for troubleshooting.
5. **Startup tab** (Windows 10/11 redirects to Task Manager).
6. Use it to disable problematic startup items or services.

---

## 3️⃣ Services.msc — Managing Windows Services

---

### What is Services.msc?

- A GUI tool to manage Windows services (start, stop, disable, automatic/manual startup).

---

**Steps to Open:**

- Press **Windows + R**, type `services.msc`, Enter.

---

### Troubleshooting with Services.msc

- Identify and restart failed services.
- Change startup type if a service is causing issues.
- Check dependencies before disabling services.
- Useful for network, print spooler, or database server troubleshooting.

---

## 4️⃣ Device Manager — Hardware Troubleshooting

---

### What is Device Manager?

- GUI tool to manage hardware devices and drivers.

---

**Open Device Manager:**

- Press **Windows + X**, select **Device Manager** or `devmgmt.msc`.

---

### Troubleshooting Steps

- Look for devices with yellow warning triangles or red Xs.
- Right-click device → **Properties** → Check **Device status**.
- Update, rollback, disable, or uninstall drivers.
- Scan for hardware changes.
- Use **View → Show hidden devices** to reveal problematic devices.

---

## 5️⃣ Advanced Troubleshooting Tips

- Enable **Boot Logging** via MSConfig to identify problematic drivers (`ntbtlog.txt` in Windows folder).
- Use **Safe Mode** (F8 or MSConfig) to troubleshoot driver or software conflicts.
- Use **Event Viewer** to correlate errors or warnings during system crashes or service failures.
- Collect **System Information** (`msinfo32`) for comprehensive diagnostics.

---

## Additional Tools & Resources

| Tool / Utility | Purpose | Download / Docs Link |
| --- | --- | --- |
| WinDbg (Windows Debugger) | Crash dump debugging | https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/ |
| BlueScreenView | Easy crash dump viewer | https://www.nirsoft.net/utils/blue_screen_view.html |
| WhoCrashed | Crash dump analyzer | https://www.resplendence.com/whocrashed |
| SFC & DISM Documentation | System repair tools | https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/repair-a-windows-image |
| MSConfig Guide | Managing startup & services | https://learn.microsoft.com/en-us/windows/client-management/msconfig |

---

# Summary Table

| Tool / Topic | Use Case | Access Method | Key Commands/Actions |
| --- | --- | --- | --- |
| BSOD Crash Dump | Analyze system crashes | WinDbg / BlueScreenView | `!analyze -v` in WinDbg |
| SFC | Repair corrupted system files | Admin CMD | `sfc /scannow` |
| DISM | Repair system image | Admin CMD | `DISM /Online /Cleanup-Image /RestoreHealth` |
| CHKDSK | Disk integrity check | Admin CMD | `chkdsk C: /f /r /x` |
| MSConfig | Manage startup & boot options | Run `msconfig` | Enable/disable services/startup |
| Services.msc | Manage Windows services | Run `services.msc` | Start/stop/restart services |
| Device Manager | Hardware & driver troubleshooting | Run `devmgmt.msc` | Update/rollback/uninstall drivers |
