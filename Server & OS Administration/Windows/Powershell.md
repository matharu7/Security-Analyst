| **Category** | **Description** | **PowerShell Command** |
| --- | --- | --- |
| **Users & Groups** | List local users | `Get-LocalUser` |
|  | List local groups | `Get-LocalGroup` |
|  | List group members | `Get-LocalGroupMember -Group "Administrators"` |
| **Network Info** | View IP configuration | `Get-NetIPConfiguration` or `ipconfig` |
|  | View network adapters | `Get-NetAdapter` |
|  | Check network connections | `Get-NetTCPConnection` or `netstat -an` |
|  | View current DNS servers | `(Get-DnsClientServerAddress).ServerAddresses` |
|  | Ping a host | `Test-Connection google.com` |
| **Wi-Fi Info** | Show saved Wi-Fi profiles | `netsh wlan show profiles` |
|  | Show Wi-Fi password (replace `WiFiName`) | `netsh wlan show profile name="WiFiName" key=clear` |
| **Disk Info** | List all drives | `Get-PSDrive -PSProvider 'FileSystem'` |
|  | Disk usage info | `Get-Volume` or `Get-Disk` |
|  | Check partitions | `Get-Partition` |
| **System Info** | General system info | `systeminfo` |
|  | OS version | `Get-ComputerInfo |
|  | Uptime | `(Get-CimInstance Win32_OperatingSystem).LastBootUpTime` |
| **Firewall** | Check firewall status | `Get-NetFirewallProfile` |
|  | Enable firewall | `Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True` |
|  | Disable firewall | `Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False` |
| **Defender (Real-time)** | Check Defender status | `Get-MpComputerStatus` |
|  | Turn real-time protection off | `Set-MpPreference -DisableRealtimeMonitoring $true` (Admin) |
|  | Turn real-time protection on | `Set-MpPreference -DisableRealtimeMonitoring $false` (Admin) |
| **Processes & Tasks** | List running processes | `Get-Process` |
|  | List services | `Get-Service` |
|  | Kill a process (e.g. notepad) | `Stop-Process -Name notepad -Force` |
| **Active Connections** | View active network connections | `netstat -ano` |
|  | Get process by PID (replace PID) | `Get-Process -Id 1234` |
| **Startup Info** | List startup apps (via registry) | `Get-CimInstance Win32_StartupCommand |
| **Scheduled Tasks** | List all scheduled tasks | `Get-ScheduledTask` |
| **Programs** | List installed programs | `Get-WmiObject -Class Win32_Product` (⚠️ slow and not recommended) |
|  | Better way to list apps | `Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*` |
| **System Updates** | Check updates | `Get-WindowsUpdateLog` |
|  | View update history | `Get-HotFix` |

### ✅ PowerShell Software Installation Methods

| **Method** | **Description** | **Example Command** |
| --- | --- | --- |
| **1. Winget (Windows 10/11)** | Built-in package manager for Windows | `winget install --id=Google.Chrome` |
| 🔹 **Check Installed Software** | List software already installed using Winget | `winget list` |
| 🔹 **Check If Available in Repository** | Search Winget repo for available packages | `winget search vlc` |
| **2. Chocolatey (3rd-party)** | Popular 3rd-party Windows package manager (needs setup) | `choco install notepadplusplus -y` |
| 🔹 **List Installed Software** | See all Chocolatey-installed packages | `choco list --local-only` |
| 🔹 **Search Software Package** | Find a package in Chocolatey repo | `choco search vlc` |
| **3. Manual Installer** | Run a `.exe` or `.msi` installer silently | `Start-Process "setup.exe" -ArgumentList "/silent" -Wait` |
| 🔹 **Download Installer via PowerShell** | Grab `.exe` or `.msi` from the web | `Invoke-WebRequest -Uri "<URL>" -OutFile "installer.exe"` |
| **4. PowerShell Script** | Custom script to download & install apps | `Invoke-WebRequest -Uri <URL> -OutFile setup.exe; Start-Process ./setup.exe -ArgumentList "/quiet"` |
