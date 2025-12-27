---

## 🗂️ **1. Core Directory Structure (C:)**

| Folder | Purpose |
| --- | --- |
| `C:\Windows\` | Main OS folder — system files, drivers, logs, updates |
| `C:\Program Files\` | Default location for 64-bit apps |
| `C:\Program Files (x86)\` | Default location for 32-bit apps on 64-bit systems |
| `C:\Users\` | User profiles (each user's personal data, settings, AppData) |
| `C:\ProgramData\` | Shared settings across all users — often used by services |
| `C:\Temp\` | Optional — legacy temp folder (modern systems use AppData Temp) |
| `C:\$Recycle.Bin\` | Recycle Bin — per-user deleted files |
| `C:\System Volume Information\` | Restore points, indexing — highly protected |
| `C:\Recovery\` | Windows Recovery Environment (WinRE) |

---

## 🔐 **2. Hidden/System Root Files**

| File | Purpose |
| --- | --- |
| `pagefile.sys` | Virtual memory — OS uses this when RAM is full |
| `hiberfil.sys` | Stores memory state for hibernation |
| `swapfile.sys` | Used by UWP apps / modern suspend operations |
| `bootmgr`, `bcd` | Boot loader and configuration |
| `ntldr` (older) | Legacy boot loader (for XP/2003) |

🔧 *These files are not visible unless hidden and system files are enabled in Folder Options.*

---

## 🧩 **3. Key Windows Subfolders (Inside `C:\Windows\`)**

| Subfolder | Contents / Function |
| --- | --- |
| `System32\` | Core 64-bit system executables, libraries, drivers, configs |
| `SysWOW64\` | 32-bit versions of system files on 64-bit Windows |
| `DriverStore\FileRepository` | Trusted location for all installed drivers |
| `System32\drivers\` | Kernel-mode driver `.sys` files |
| `SystemApps\` | Built-in system UWP apps (like Settings, Edge) |
| `Fonts\` | System-installed fonts |
| `Resources\Themes\` | Themes and visual styles |
| `SoftwareDistribution\` | Windows Update temporary files |
| `Logs\` | System logs (CBS, DISM, Event Log data, etc.) |
| `Tasks\` | Stores **Scheduled Tasks** as XML (also managed via Task Scheduler) |
| `Temp\` | System temporary files |
| `WinSxS\` | Windows Side-by-Side — holds multiple DLL versions |
| `Panther\` | Windows setup logs |

---

## 🔐 **4. Credentials, Passwords & Security Files**

| Path / File | Purpose | Protection |
| --- | --- | --- |
| `C:\Windows\System32\config\SAM` | Stores hashed local passwords (Security Account Manager) | Only SYSTEM can access |
| `C:\Windows\System32\config\SYSTEM` | Registry hive — contains system-wide settings |  |
| `C:\Users\USERNAME\AppData\Roaming\Microsoft\Credentials` | Credential Manager stores encrypted creds | Protected per user |
| `C:\Users\USERNAME\AppData\Local\Microsoft\Vault` | Web passwords, modern app logins |  |

🔐 These files are **not plaintext** and are **encrypted** using Windows internal APIs. You can **view saved creds** via:

```powershell
# Open Credential Manager GUI
control /name Microsoft.CredentialManager

```

```powershell
# List stored credentials (read-only, limited)
cmdkey /list

```

---

## 🌐 **5. Networking & Hosts File**

| File / Folder | Purpose |
| --- | --- |
| `C:\Windows\System32\drivers\etc\hosts` | Manual DNS override (map hostnames to IPs) |
| `C:\Windows\System32\drivers\etc\lmhosts` | Legacy NetBIOS name resolution |
| `C:\Windows\System32\drivers\etc\protocol` | Protocol name-to-number mappings |
| `C:\Windows\System32\drivers\etc\services` | Port/service name mapping (like `/etc/services`) |

🛠️ *Edit `hosts` only with elevated permissions (Administrator).*

```powershell
# Open hosts file in Notepad as admin
Start-Process notepad.exe "C:\Windows\System32\drivers\etc\hosts" -Verb RunAs

```

---

## 🔧 **6. Registry File Locations (Offline)**

| Hive Name | File Path |
| --- | --- |
| HKEY_LOCAL_MACHINE\SAM | `C:\Windows\System32\config\SAM` |
| HKEY_LOCAL_MACHINE\SYSTEM | `C:\Windows\System32\config\SYSTEM` |
| HKEY_LOCAL_MACHINE\SOFTWARE | `C:\Windows\System32\config\SOFTWARE` |
| HKEY_USERS.DEFAULT | `C:\Windows\System32\config\DEFAULT` |

> ⚠️ These are used by the OS at runtime, and cannot be opened directly unless the system is offline (e.g., via recovery or dual-boot).
> 

---

## 🧠 **7. Environment & Profile Variables**

| Variable | Expands To |
| --- | --- |
| `%SystemRoot%` | `C:\Windows` |
| `%ProgramFiles%` | `C:\Program Files` |
| `%ProgramData%` | `C:\ProgramData` |
| `%USERPROFILE%` | `C:\Users\YourName` |
| `%APPDATA%` | `C:\Users\YourName\AppData\Roaming` |
| `%LOCALAPPDATA%` | `C:\Users\YourName\AppData\Local` |

Use these in PowerShell or CMD:

```powershell
echo $env:USERPROFILE
echo $env:SYSTEMROOT

```

---

## ⚙️ **8. Exploring & Managing File System**

### ✅ PowerShell Examples

```powershell
# List root folders, including hidden/system
Get-ChildItem -Path C:\ -Force

# Explore drivers
Get-ChildItem -Path "C:\Windows\System32\drivers"

# View credentials folder
Get-ChildItem -Force "$env:APPDATA\Microsoft\Credentials"

# List Scheduled Tasks folder
Get-ChildItem -Path "C:\Windows\System32\Tasks" -Recurse

# View permissions on Windows folder
Get-Acl "C:\Windows" | Format-List

# Edit hosts file
Start-Process notepad "C:\Windows\System32\drivers\etc\hosts" -Verb runAs

```

### ✅ CMD Examples

```bash
dir C:\ /a       :: Show hidden + system files
cd %APPDATA%
echo %SystemRoot%
bcdedit /enum    :: View bootloader entries

```

### ✅ GUI Navigation

- **File Explorer** → View → Show Hidden Items
- Folder Options → Uncheck "Hide protected OS files"
- **Run** → `%APPDATA%`, `%LOCALAPPDATA%`, `%ProgramData%`

---
