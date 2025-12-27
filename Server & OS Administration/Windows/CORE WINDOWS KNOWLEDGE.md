---

## 1️⃣ WINDOWS ARCHITECTURE

**(Kernel, User Mode, Services, Threads, Boot Process, Subsystems, HAL)**

---

### 🔍 Core Concepts

### ✅ **Windows Architecture Layers**

| Layer | Components |
| --- | --- |
| **User Mode** | Applications, subsystems (Win32, .NET, POSIX), user DLLs |
| **Kernel Mode** | Kernel, HAL, Drivers, Executive components |
| **Hardware Abstraction Layer (HAL)** | Masks hardware differences for OS |
| **System Services** | Background services running via `svchost.exe` |

---

### 🧠 Important Theoretical Areas

### 🔹 1. **Processes and Threads**

- **Process**: An instance of a program. Has private memory space.
- **Thread**: Smallest execution unit inside a process. All processes have at least one thread.

### 🔹 2. **Windows Executive Components (in Kernel)**

- **Object Manager**: Manages OS resources as objects.
- **Memory Manager**: Handles physical/virtual memory, paging.
- **I/O Manager**: Manages communication with devices.
- **Security Reference Monitor (SRM)**: Enforces security policies.

### 🔹 3. **Windows Boot Process (UEFI BIOS → OS)**

1. BIOS/UEFI loads → POST
2. Bootmgr (boot manager) starts
3. Winload.exe loads the kernel
4. Ntoskrnl.exe is loaded
5. Session Manager (smss.exe) starts
6. Winlogon launches login screen

### 🔹 4. **Windows Subsystems**

- **Win32** – Primary subsystem for Windows GUI apps.
- **WOW64** – 32-bit support on 64-bit systems.
- **WSL** – Linux compatibility layer.

---

### 🛠️ Practical Commands

```powershell
# View running processes and services
Get-Process
Get-Service

# See thread count per process
Get-Process | Select Name, Id, Threads

# View boot configuration
bcdedit

# System information summary
systeminfo

# See system startup processes
Get-CimInstance Win32_StartupCommand

```

### 🔧 Tools:

- **Process Hacker / Process Explorer**
- **Windows Performance Analyzer (WPA)**
- **msinfo32** – hardware/software/system details
- **Autoruns** – advanced startup control

---

## **2️⃣ FILE SYSTEMS — Expanded Theory & Practical**

---

### ✅ 🔍 COMPARISON OF MAJOR FILE SYSTEMS

| File System | OS Support | Max File Size | Max Volume Size | Journaling | Encrypted | Windows Support |
| --- | --- | --- | --- | --- | --- | --- |
| **NTFS** | Windows (native) | 256 TB+ | 8 PB | ✅ | ✅ (EFS) | ✅ Native |
| **FAT32** | All (legacy) | 4 GB | 2 TB | ❌ | ❌ | ✅ Native |
| **exFAT** | All (modern) | 16 EB | 128 PB | ❌ | ❌ | ✅ Native |
| **ReFS** | Windows Server | 35 PB | 35 PB | ✅ | ❌ | ✅ (limited) |
| **ext4** | Linux (native) | 16 TB | 1 EB | ✅ | ❌ | ❌ (Read-only via 3rd party) |
| **HFS+** | macOS (legacy) | 8 EB | 8 EB | ✅ | ❌ | ❌ (Read-only via 3rd party) |
| **APFS** | macOS (modern) | 8 EB | 8 EB | ✅ | ✅ | ❌ (No support) |
| **ZFS** | Solaris, FreeBSD | 16 EB | 16 EB | ✅ | ✅ | ❌ (3rd party) |

---

### 💡 Real-World Use Cases

| Scenario | File System Best Fit |
| --- | --- |
| Windows boot drive | **NTFS** |
| Flash drives (Windows & Mac/Linux) | **exFAT** |
| Older external drives / legacy devices | **FAT32** |
| Windows Server file server | **ReFS** (if fault tolerance needed) |
| Linux root volume | **ext4** |
| macOS system drive | **APFS** |
| Dual boot Win+Linux | ext4 (Linux) + NTFS/exFAT for shared partitions |

---

### 🔧 WINDOWS SUPPORT FOR NON-NATIVE FILE SYSTEMS

| File System | Read | Write | Tool Needed |
| --- | --- | --- | --- |
| ext4 | ✅ | ❌ | **Ext2Fsd**, **Linux File Systems for Windows by Paragon** |
| HFS+ | ✅ | ❌ | **HFSExplorer** |
| APFS | ❌ | ❌ | **Paragon APFS** (paid) |
| ZFS | ❌ | ❌ | Requires Cygwin or WSL with ZFS on Linux |

---

## ⚙️ NTFS DEEP DIVE — Technical & Practical

### 🔬 NTFS Structures

- **MFT (Master File Table)**: Stores metadata and file record.
- **$LogFile**: Journaling.
- **$Bitmap**: Free/used cluster tracking.
- **$Security**: Stores security descriptors.
- **ADS (Alternate Data Streams)**: Can store hidden data in same file.

---

### 🛠️ NTFS PowerShell & CLI Commands

```powershell
# View volume FS types
Get-Volume

# Format NTFS
Format-Volume -DriveLetter E -FileSystem NTFS -NewFileSystemLabel "NTFSVolume"

# View NTFS permissions
Get-Acl C:\important.txt | Format-List

# Set permissions
icacls "C:\important.txt" /grant "DOMAIN\User":(R,W)

# Add alternate data stream
echo "SecretInfo" > file.txt:hidden.txt

# Check for ADS
Get-Item -Path file.txt -Stream *

```

---

### 🔎 NTFS Security Features

| Feature | Tool | Usage |
| --- | --- | --- |
| ACLs | `icacls`, `Get-Acl` | Set granular permissions |
| EFS | GUI or `cipher.exe` | Encrypt individual files |
| Auditing | GPO + Event Viewer | Track access to sensitive files |
| Junctions/Links | `mklink` | Create links like Linux symlinks |

---

## ⚙️ exFAT, FAT32 & ReFS — Practical

### ✅ exFAT (Great for USB drives)

```powershell
# Format to exFAT
Format-Volume -DriveLetter F -FileSystem exFAT -NewFileSystemLabel "USBDrive"

```

> ✅ Use exFAT for compatibility across Windows, macOS, Linux (modern).
> 

---

### ⚠️ FAT32 Limitations

- No support for files > 4 GB
- No security features
- Used in BIOS-based boot drives, embedded devices

```powershell
# Format to FAT32
Format-Volume -DriveLetter G -FileSystem FAT32 -NewFileSystemLabel "OldDevice"

```

---

### ✅ ReFS (Windows Server)

```powershell
# Format with ReFS (only on supported systems)
Format-Volume -DriveLetter R -FileSystem ReFS -NewFileSystemLabel "FaultTolerantDrive"

```

> ⚠️ Requires Windows Server or special provisioning to use in Windows 10/11 Enterprise (and may have limitations).
> 

---

## 3️⃣ WINDOWS EDITIONS

**(Home, Pro, Enterprise, Education) + Upgrade Paths + Features Table**

---

### 🧠 Theoretical Knowledge

| Feature | Home | Pro | Enterprise | Education |
| --- | --- | --- | --- | --- |
| BitLocker | ❌ | ✅ | ✅ | ✅ |
| GPO | ❌ | ✅ | ✅ | ✅ |
| Domain Join | ❌ | ✅ | ✅ | ✅ |
| AppLocker | ❌ | ❌ | ✅ | ✅ |
| Assigned Access | ❌ | ✅ | ✅ | ✅ |
| Azure AD Join | ❌ | ✅ | ✅ | ✅ |

### ✅ Upgrade Paths:

- **Home → Pro**: Retail upgrade
- **Pro → Enterprise**: Only via Volume License
- Use `changepk.exe` to enter a new product key

---

### 🛠️ Practical Commands

```powershell
# Get edition
(Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion").EditionID

# Get product name
wmic os get Caption, OSArchitecture, Version

# Open Activation window
start ms-settings:activation

# Trigger edition upgrade
changepk.exe /ProductKey XXXXX-XXXXX-XXXXX-XXXXX-XXXXX

```

---

## 4️⃣ WINDOWS LICENSING & ACTIVATION

**(Retail, OEM, Volume, KMS, MAK) + Troubleshooting Activation**

---

### 🧠 Deep Theoretical Info

| Type | Transferable | Activation |
| --- | --- | --- |
| OEM | ❌ | Hardware-bound |
| Retail (FPP) | ✅ | Online |
| MAK | ❌ (limited activations) | One-time online |
| KMS | ❌ (auto renewal) | Internal activation every 180 days |

### ✅ Activation Flow:

1. Install OS
2. Enter key or sign in with linked Microsoft account
3. Activation via:
    - Digital License
    - KMS Server
    - Manual MAK entry

---

### 🛠️ Practical Commands

```bash
# Check activation status
slmgr.vbs /xpr

# Display license info
slmgr.vbs /dli

# View verbose licensing
slmgr.vbs /dlv

# Install new product key
slmgr.vbs /ipk XXXXX-XXXXX-XXXXX-XXXXX-XXXXX

# Activate Windows
slmgr.vbs /ato

# Check last 5 characters of key
wmic path SoftwareLicensingService get OA3xOriginalProductKey

```

---

### 🔧 Licensing Tools

- **VAMT (Volume Activation Management Tool)**: Enterprise activation manager
- **RSOP / gpresult**: Check GPO-based activation
- **Settings → Activation**: GUI tool

---

## 

# 🚀 **Module 2: Windows Installation & Deployment — Theory + Practical**

---

## 🧱 1️⃣ **Manual Installation (ISO/USB)**

### 🔍 Overview

Manual installs are used for:

- Single machine setup
- Lab/testing environments
- Base image creation

### 🧠 Core Concepts

- Bootable media is created using tools like **Rufus**, **Media Creation Tool**, or **DISM**.
- Installs can be **interactive** (GUI) or **unattended** using **answer files (XML)**.

### 🛠️ Step-by-Step

1. **Download ISO** from Microsoft (via [MSDN](https://my.visualstudio.com/Downloads) or MCT).
2. Use **Rufus** to create bootable USB:
    - Choose ISO
    - Set partition scheme (MBR/UEFI)
3. Boot target machine from USB
4. Go through setup wizard:
    - Language, edition, partition, user creation

### 🔧 Advanced Manual Tools

```powershell
# Mount ISO
Mount-DiskImage -ImagePath "C:\ISOs\Win11.iso"

# List mounted volumes
Get-Volume

# Apply image using DISM (manual deployment)
dism /Apply-Image /ImageFile:D:\sources\install.wim /Index:1 /ApplyDir:C:\

```

---

## 🌐 2️⃣ **Windows Deployment Services (WDS)**

### 🔍 Overview

> WDS is a server role that allows PXE boot and network-based deployment of Windows images.
> 

Used in:

- Enterprises
- Labs
- Mass rollout of images to new PCs

### 🧠 Core Concepts

- PXE boot via DHCP/Network
- Uses **.WIM** files (captured images)
- Typically integrated with **Active Directory**

### ⚙️ WDS Architecture

| Component | Description |
| --- | --- |
| PXE Listener | Responds to PXE boot requests |
| Boot Images | WinPE images to launch setup |
| Install Images | Actual Windows OS to deploy |
| Image Store | Stores captured and default images |

### 🛠️ Setup Process

1. Install WDS role via Server Manager or PowerShell:

```powershell
Install-WindowsFeature -Name WDS -IncludeManagementTools

```

1. Configure WDS:
    - Point to remote installation folder
    - Add boot and install images from ISO
2. PXE Boot client device
3. Choose image via WDS menu

---

## 🖼️ 3️⃣ **Sysprep & Image Creation**

### 🔍 Purpose

> Sysprep prepares a Windows installation to be cloned and re-imaged across multiple machines.
> 

### 🧠 Key Concepts

| Term | Meaning |
| --- | --- |
| Sysprep | Removes SID and resets user data |
| Generalize | Removes hardware-specific drivers |
| OOBE | Out-of-box experience on first boot |
| Answer File | Automates deployment steps |

### 🛠️ Capture Custom Image

1. Configure Windows (install apps, updates, drivers)
2. Run:

```bash
sysprep /generalize /oobe /shutdown /unattend:C:\unattend.xml

```

1. Boot into WinPE from USB or PXE
2. Capture image:

```bash
dism /Capture-Image /ImageFile:D:\Images\custom.wim /CaptureDir:C:\ /Name:"CustomWin11"

```

1. Upload `.wim` to WDS or MDT

---

## ⚙️ 4️⃣ **Microsoft Deployment Toolkit (MDT)**

### 🔍 Overview

> MDT is a free Microsoft solution for automating Lite Touch (LTI) deployments.
> 

Great for:

- Mid-sized orgs
- Hybrid deployment (manual + semi-automated)
- Predefined task sequences (apps, drivers, updates)

### 🧠 Core Features

| Feature | Description |
| --- | --- |
| Deployment Shares | Central storage for images and scripts |
| Task Sequences | Step-by-step instructions (OS + apps) |
| Bootstrap.ini | Pre-deployment config |
| CustomSettings.ini | Deployment rules and automation |

### 🛠️ Setup Steps

1. Install **Windows ADK** + **WinPE Add-on**
2. Install **MDT**
3. Create **Deployment Share**
4. Import OS ISO
5. Create **Task Sequence**
6. Generate boot ISO or WDS-bootable image
7. Boot target machine using MDT media
8. Follow task sequence (optional prompts)

---

## ☁️ 5️⃣ **Windows Autopilot (Cloud-Based Deployment)**

> Designed for modern IT / zero-touch deployment — no imaging required.
> 

Best for:

- **Hybrid** or **Azure AD-joined** organizations
- **Remote users**
- **Cloud-native provisioning**

### 🧠 Core Concepts

- Autopilot works with **Intune** and **Azure AD**
- Devices are registered via **hardware hash**
- Configured via **profiles**: join type, OOBE behavior, apps

### 🛠️ Setup Flow

1. Export device hardware ID:

```powershell
Get-WindowsAutopilotInfo.ps1 -OutputFile device.csv

```

1. Upload CSV to Intune
2. Create **Autopilot profile**:
    - Azure AD join / Hybrid join
    - Auto-login, skip OOBE pages
3. Assign profile to device group
4. End-user connects to internet → Auto-configured

---

## ✅ Summary Table

| Method | Use Case | Automation Level | Tools Needed |
| --- | --- | --- | --- |
| Manual ISO | One-off installs / labs | Low | USB, ISO, GUI |
| WDS | On-prem deployment | Medium | Server, PXE, .WIM |
| Sysprep + DISM | Cloning customized OS | Medium | WinPE, Sysprep, DISM |
| MDT | Semi-automated mass deployment | High | MDT, ADK, Task Sequences |
| Autopilot | Cloud-native provisioning (Intune) | Very High | Intune, Azure AD, MS Endpoint |

---

## 🧰 Tools You Should Have

| Tool / Suite | Purpose |
| --- | --- |
| **Rufus / Media Creation Tool** | Create bootable USB |
| **DISM** | Apply or capture Windows images |
| **Sysprep** | Generalize systems |
| **WDS** | Network-based imaging |
| **MDT** | Task-sequenced deployment |
| **Intune + Autopilot** | Cloud-native deployment & management |
| **WinPE** | Boot environment for imaging |

# 👥 **Module 3: Windows User Management — Deep Theory + Practical**

---

## 🔐 1️⃣ LOCAL USERS AND GROUPS

---

### 🧠 Core Concepts

| Component | Description |
| --- | --- |
| **User Account** | Identity used to log into the system |
| **Local User** | Account stored in `SAM` database on local machine |
| **Group** | Collection of users with shared permissions |
| **Built-in Groups** | Administrators, Users, Guests, Power Users, Remote Desktop Users, etc. |

### 🛠️ PowerShell & CLI Commands

```powershell
# List local users
Get-LocalUser

# Create a new user
New-LocalUser -Name "John" -Password (Read-Host -AsSecureString)

# List local groups
Get-LocalGroup

# Add user to group
Add-LocalGroupMember -Group "Administrators" -Member "John"

# Delete user
Remove-LocalUser -Name "John"

```

```bash
# CMD equivalent
net user
net user John /add
net localgroup Administrators John /add
net user John /delete

```

---

### 🔐 Group Use Cases

| Group | Purpose |
| --- | --- |
| **Administrators** | Full access to system |
| **Users** | Standard access, limited rights |
| **Guests** | Very restricted access, often disabled |
| **Remote Desktop Users** | Allowed to log in via RDP |

---

## 🧍 2️⃣ USER PROFILES & PERMISSIONS

---

### 🧠 User Profiles

| Type | Description |
| --- | --- |
| **Local** | Stored locally in `C:\Users\username` |
| **Roaming** | Synced with network, used in domain setups |
| **Mandatory** | Read-only profiles; changes not saved |
| **Default** | Template used when creating a new user profile |

### 📁 Profile Structure (`C:\Users\Username\`)

- `Documents`, `Desktop`, `Downloads`, etc.
- `AppData`:
    - **Local**: Machine-specific
    - **Roaming**: Syncs in domain environments
    - **LocalLow**: Lower permissions (e.g., IE Protected Mode)

---

### 🛠️ Profile Commands & Tools

```powershell
# View user profile list
Get-CimInstance Win32_UserProfile | Select LocalPath, LastUseTime

# Delete profile (not just the user)
Remove-CimInstance -InputObject (Get-CimInstance Win32_UserProfile | Where-Object { $_.LocalPath -like "*John*" })

```

**Registry Location:**

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList`

> Match user SID to path. Useful in forensics or profile corruption scenarios.
> 

---

### 🛡️ File/Folder Permissions (NTFS)

- Controlled via **ACLs** (Access Control Lists)
- Each file/folder has a DACL (Discretionary ACL) that assigns access to users/groups

```powershell
# View ACL
Get-Acl C:\Folder\example.txt | Format-List

# Grant permissions
icacls "C:\Folder" /grant John:(OI)(CI)F

```

| Symbol | Meaning |
| --- | --- |
| R | Read |
| W | Write |
| F | Full Control |
| OI/CI | Object/Container Inherit |

---

## 🛑 3️⃣ USER ACCOUNT CONTROL (UAC)

---

### 🧠 What is UAC?

> UAC helps prevent unauthorized changes to the OS by requiring elevation.
> 

### 🔍 How it Works

- Even if logged in as Administrator, you run apps with **standard privileges**
- When elevation is required, UAC prompts for consent
- Protects against **malware**, **accidental system changes**, and **privilege escalation**

### 🎛️ UAC Levels

| Level | Behavior |
| --- | --- |
| Always notify | UAC prompts on all changes |
| Default | Prompt only for apps |
| Never notify | UAC is disabled (insecure) |

### 🔧 Change UAC Setting via GUI

- `Control Panel → User Accounts → Change User Account Control settings`

### 🛠️ Registry for UAC

```powershell
# Get UAC setting from registry
Get-ItemProperty "HKLM:\Software\Microsoft\Windows\CurrentVersion\Policies\System" | Select-Object EnableLUA, ConsentPromptBehaviorAdmin

```

---

## 🔐 4️⃣ CREDENTIAL MANAGER

---

### 🧠 What It Is

> Windows Credential Manager stores login credentials (web, network, RDP) securely.
> 

Types of credentials:

| Type | Description |
| --- | --- |
| **Windows** | Used for network shares, RDP, etc. |
| **Web** | Stored by Internet Explorer / Edge |
| **Generic** | App-specific credentials (VPN, tools, etc.) |

Stored in:

`Control Panel → Credential Manager`

Also stored in:

`%SystemDrive%\Users\<User>\AppData\Roaming\Microsoft\Credentials` (encrypted)

---

### 🛠️ Manage Credentials via CMD/PowerShell

```bash
# List credentials (cmd)
cmdkey /list

# Add credential
cmdkey /add:SERVERNAME /user:DOMAIN\User /pass:Password123

# Delete credential
cmdkey /delete:SERVERNAME

```

```powershell
# View stored credentials
$vault = New-Object -ComObject "Vault.Vault"
$vault.GetVaults()

```

> PowerShell access to Credential Manager is limited; use CredentialManager module or third-party tools for advanced scripting.
> 

---

## ✅ Summary Table

| Area | Key Tools | Practice Focus |
| --- | --- | --- |
| Local Accounts | PowerShell, net user | Create/delete users and groups |
| Profiles | `%USERPROFILE%`, Registry | Understand Local vs Roaming |
| NTFS Permissions | `icacls`, `Get-Acl` | Secure folders |
| UAC | UAC Settings, Registry | Test elevation prompts |
| Credentials | Credential Manager, cmdkey | Store/delete RDP/network passwords |

---

## 🧰 Recommended Tools

| Tool | Use Case |
| --- | --- |
| `lusrmgr.msc` | GUI for managing local users/groups |
| `Computer Management` | Access local users, event logs, etc. |
| `Sysinternals AccessChk` | View effective permissions |
| `Credential Manager` | GUI for managing saved credentials |
| `Policy Analyzer` | Audit UAC and permissions policies |

---

# 🗂️ File System & Storage Management — Full Practical Guide

---

## 1️⃣ DISK PARTITIONING & VOLUME TYPES

---

### Overview

- Understand **Basic vs Dynamic disks**
- Manage partitions and volumes with **GUI tools** and **PowerShell/DiskPart commands**

---

### A) Disk Partitioning with GUI

1. Open **Disk Management**:
    - Press `Win + R`, type `diskmgmt.msc`, hit Enter.
2. Right-click on an unallocated disk space → **New Simple Volume**.
3. Follow the wizard:
    - Specify volume size.
    - Assign drive letter.
    - Choose file system (NTFS recommended).
4. Click **Finish** to create and format the partition.

---

### B) Convert Basic to Dynamic Disk (GUI)

1. In Disk Management, right-click the disk (not partition) → **Convert to Dynamic Disk**.
2. Confirm your selection → click OK.

---

### C) Disk Partitioning and Conversion via PowerShell

```powershell
# List all disks and check if they are basic or dynamic
Get-Disk | Format-Table Number, FriendlyName, OperationalStatus, PartitionStyle, IsDynamic

# Create a new partition (e.g., 20GB) on disk 1 and assign drive letter
New-Partition -DiskNumber 1 -Size 20GB -AssignDriveLetter

# Format partition to NTFS with label "DataVolume"
Format-Volume -DriveLetter E -FileSystem NTFS -NewFileSystemLabel "DataVolume" -Confirm:$false

# Convert disk 1 to dynamic
Set-Disk -Number 1 -IsDynamic $true

```

---

### D) Disk Partitioning with DiskPart (Command Line)

```bash
diskpart
list disk
select disk 1
create partition primary size=20480
format fs=ntfs label="DataVolume" quick
assign letter=E
exit

```

---

### 

---

## 2️⃣ STORAGE SPACES

---

### Overview

- Pool multiple disks to create flexible virtual disks.
- Choose resiliency: Simple (no redundancy), Mirror, Parity.

---

### A) Create Storage Spaces Using GUI

1. Open **Control Panel → Storage Spaces** (search in Start menu).
2. Click **Create a new pool and storage space**.
3. Select physical disks (make sure they're empty or data backed up).
4. Name your Storage Space and choose:
    - Resiliency type (Simple, Mirror, Parity).
    - Size of virtual disk.
5. Click **Create Storage Space**.
6. The new virtual disk will appear in Disk Management. Format it to NTFS and assign a drive letter.

---

### B) Storage Spaces PowerShell Management

```powershell
# List physical disks that can be pooled
Get-PhysicalDisk | Where-Object CanPool -eq $true

# Create a storage pool named "Pool1" using all available physical disks
New-StoragePool -FriendlyName "Pool1" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks (Get-PhysicalDisk -CanPool $true)

# Create virtual disk of 50GB with mirror resiliency
New-VirtualDisk -StoragePoolFriendlyName "Pool1" -FriendlyName "MirrorDisk" -ResiliencySettingName Mirror -Size 50GB

# Initialize the virtual disk (replace <DiskNumber> with actual number)
Initialize-Disk -Number <DiskNumber>

# Create a new partition on the virtual disk using all available space and assign drive letter
New-Partition -DiskNumber <DiskNumber> -UseMaximumSize -AssignDriveLetter

# Format the new partition to NTFS with label "StoragePool"
Format-Volume -DriveLetter <Letter> -FileSystem NTFS -NewFileSystemLabel "StoragePool"

```

---

### C) Storage Spaces Health & Management

- Check health status:

```powershell
Get-StoragePool -FriendlyName "Pool1" | Get-PhysicalDisk | Select FriendlyName, OperationalStatus, HealthStatus

```

- Repair degraded storage spaces:

```powershell
Repair-VirtualDisk -FriendlyName "MirrorDisk"

```

---

### 

---

## 3️⃣ BITLOCKER DRIVE ENCRYPTION

---

### Overview

- Protect data by encrypting whole drives.
- Use TPM or password/USB key for authentication.
- Manage BitLocker via GUI or PowerShell.

---

### A) Enable BitLocker via GUI

1. Go to **Control Panel → BitLocker Drive Encryption**.
2. Click **Turn on BitLocker** for the target drive.
3. Choose unlock method:
    - Use TPM.
    - Use password or USB key.
4. Save recovery key (to Microsoft account, file, or print).
5. Start encryption (used space only or full).

---

### B) Enable BitLocker with PowerShell

```powershell
# Enable BitLocker on drive C: using TPM
Enable-BitLocker -MountPoint "C:" -EncryptionMethod Aes256 -UsedSpaceOnly -TpmProtector

# Add a recovery password protector
Add-BitLockerKeyProtector -MountPoint "C:" -RecoveryPasswordProtector

# Get BitLocker status
Get-BitLockerVolume

# Suspend BitLocker temporarily (for updates, etc.)
Suspend-BitLocker -MountPoint "C:" -RebootCount 1

# Resume BitLocker protection
Resume-BitLocker -MountPoint "C:"

```

---

### C) Backing Up Recovery Key to AD (Domain Environment)

- Use Group Policy to enable automatic backup.
- Or manually save key with:

```powershell
Backup-BitLockerKeyProtector -MountPoint "C:" -KeyProtectorId (Get-BitLockerVolume -MountPoint "C:").KeyProtector[0].KeyProtectorId

```

---

### 

---

## 4️⃣ FILE PERMISSIONS (NTFS vs SHARE)

---

### Overview

- Understand the difference between NTFS permissions (local) and Share permissions (network).
- Use GUI and command-line tools to manage permissions.

---

### A) Set NTFS Permissions via GUI

1. Right-click the folder → **Properties → Security tab**.
2. Click **Edit** to modify permissions.
3. Add/remove users and assign permissions (Read, Modify, Full Control).
4. Use **Advanced** to manage inheritance and auditing.

---

### B) Set Share Permissions via GUI

1. Right-click the folder → **Properties → Sharing tab**.
2. Click **Advanced Sharing** → Check **Share this folder**.
3. Click **Permissions** → Add users/groups and set permissions (Read, Change, Full Control).

---

### C) Set NTFS Permissions with PowerShell

```powershell
# View current NTFS permissions
Get-Acl "C:\SharedFolder" | Format-List

# Grant Full Control permission to user 'JohnDoe'
$acl = Get-Acl "C:\SharedFolder"
$rule = New-Object System.Security.AccessControl.FileSystemAccessRule("JohnDoe","FullControl","Allow")
$acl.SetAccessRule($rule)
Set-Acl "C:\SharedFolder" $acl

```

---

### D) Set Share Permissions with PowerShell (via `net share`)

```powershell
# Share folder C:\SharedFolder as ShareName with full access to everyone
net share ShareName=C:\SharedFolder /GRANT:Everyone,FULL

# To remove a share
net share ShareName /delete

```

---

### E) Using `icacls` for Quick Permission Changes

```bash
# Grant Modify permission to user 'JohnDoe' on folder
icacls "C:\SharedFolder" /grant JohnDoe:(M)

# Remove all permissions for user
icacls "C:\SharedFolder" /remove JohnDoe

# View permissions for folder
icacls "C:\SharedFolder"

```

---

### F) Understanding Effective Permissions

- When accessing a shared folder:
    - **Share permissions** limit access over the network.
    - **NTFS permissions** limit access locally and over network.
    - Effective permission = most restrictive of both.

---

###
