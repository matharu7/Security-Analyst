| Hive Name | Abbreviation | Purpose / Description |
| --- | --- | --- |
| **HKEY_CLASSES_ROOT** | HKCR | Stores file associations and COM object registrations (how files open, shortcuts, protocols). |
| **HKEY_CURRENT_USER** | HKCU | Contains settings and preferences for the currently logged-in user. |
| **HKEY_LOCAL_MACHINE** | HKLM | Contains system-wide settings and configurations that apply to all users. |
| **HKEY_USERS** | HKU | Contains settings for all user profiles on the system, including inactive users. |
| **HKEY_CURRENT_CONFIG** | HKCC | Contains hardware profile information used at system startup. |
|  |  |  |

### 1. HKEY_CLASSES_ROOT (HKCR)

**Purpose:**

- Manages file associations (which program opens which file types), COM object registrations, and shortcut data.

**Key Locations:**

- `HKCR\.exe` — Default program assigned to `.exe` files
- `HKCR\Applications` — Installed application handlers

**Forensic Use:**

- Helps determine how files are opened or if malware hijacked file extensions or changed default programs.

### 2. HKEY_CURRENT_USER (HKCU)

**Purpose:**

- Stores settings and preferences for the currently logged-in user.

**Key Locations:**

- `HKCU\Software` — User-specific application settings (e.g., Chrome, Office)
- `HKCU\Microsoft\Windows\CurrentVersion\Run` — Programs that start automatically when the user logs in
- `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs` — List of recently accessed files

**Forensic Use:**

- Tracks user activity, installed software, and program execution history.

### 3. HKEY_LOCAL_MACHINE (HKLM)

**Purpose:**

- Contains system-wide settings affecting all users on the machine.

**Key Locations:**

- `HKLM\SOFTWARE` — Installed programs (64-bit)
- `HKLM\SOFTWARE\Wow6432Node` — Installed programs (32-bit on 64-bit systems)
- `HKLM\SYSTEM\CurrentControlSet\Services` — Windows services and device drivers
- `HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR` — History of USB devices connected

**Forensic Use:**

- Useful for checking installed software, device history (like USB devices), and persistence mechanisms used by malware.

### 4. HKEY_USERS (HKU)

**Purpose:**

- Contains configuration settings for all user profiles, including inactive ones.

**Key Locations:**

- `HKU\<SID>\Software` — Settings for each user, similar to HKCU but for each user profile
- `HKU\<SID>_Classes` — Per-user file associations

**Forensic Use:**

- Enables investigation of multiple users on a system, including deleted or inactive profiles.

### 5. HKEY_CURRENT_CONFIG (HKCC)

**Purpose:**

- Contains the hardware profile loaded at system startup, linked to hardware-related configurations.

**Key Locations:**

- `HKCC\Software\Fonts` — System fonts
- `HKCC\System\CurrentControlSet\Control\Print\Printers` — Printer settings

**Forensic Use:**

- Less commonly used, but can reveal hardware changes or printer configuration history.

### Common Registry Hive Files and Their Corresponding Registry Keys:

| Hive File | Registry Path | Description |
| --- | --- | --- |
| **SYSTEM** | `HKEY_LOCAL_MACHINE\SYSTEM` | Contains system configuration, hardware info, boot settings, and services. |
| **SOFTWARE** | `HKEY_LOCAL_MACHINE\SOFTWARE` | Stores installed software info, system settings, and application configurations. |
| **SAM** | `HKEY_LOCAL_MACHINE\SAM` | Security Account Manager data — user account info and security policies. Requires SYSTEM access. |
| **SECURITY** | `HKEY_LOCAL_MACHINE\SECURITY` | Contains security policy and local security authority (LSA) secrets. Requires SYSTEM access. |
| **DEFAULT** | `HKEY_USERS\.DEFAULT` | Default user profile settings applied before login. |
| **NTUSER.DAT** | `HKEY_CURRENT_USER` | Per-user settings and preferences for the currently logged-in user. One NTUSER.DAT per user. |

### 

### 🔑 1. User Account & Login Information

- **User Profiles & SIDs**
    - Registry Path: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList`
    - Description: Lists all user profiles on the system identified by their Security Identifiers (SIDs). Each SID key contains a `ProfileImagePath` showing the path to the user’s profile folder.
    - Forensics: Helps identify all users who have logged into the machine and their profile locations.
- **Last Logged-On User**
    - Registry Path: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Authentication\LogonUI`
    - Keys:
        - `LastLoggedOnUser` — Username of the last interactive login
        - `LastLoggedOnSAMUser` — Username of the last SAM (local) login
    - Forensics: Determines which user last logged into the system.
- **Stored Credentials (LSA Secrets)**
    - Registry Path: `HKEY_LOCAL_MACHINE\SECURITY\Policy\Secrets`
    - Description: Contains sensitive information such as cached passwords, auto-logon credentials, and service account credentials.
    - Note: Access requires SYSTEM or Admin privileges.
    - Forensics: Can reveal stored credentials useful for privilege escalation or lateral movement investigations.
- **UserAssist (Program Execution History)**
    - Registry Path: `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{GUID}\Count`
    - Description: Tracks the count of user-executed GUI applications. The data is ROT13 encoded.
    - Example GUIDs:
        - `{CEBFF5CD-ACE2-4F4F-9178-9926F41749EA}` — Windows 10+
        - `{F4E57C4B-2036-45F0-A9AB-443BCFE33D9F}` — Legacy
    - Forensics: Used to reconstruct user activity regarding program usage.

---

### 💻 2. System & Hardware Information

- **USB Device History**
    - Registry Path: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\USBSTOR` (USB storage devices)
    - Also: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\USB` (Generic USB devices)
    - Forensics: Reveals USB devices that have been connected to the system, useful in tracking data exfiltration or unauthorized device use.
- **Mounted Devices (Disks & Partitions)**
    - Registry Path: `HKEY_LOCAL_MACHINE\SYSTEM\MountedDevices`
    - Description: Maps drive letters to physical devices and volumes.
    - Forensics: Helps identify disk usage and drive assignments.
- **Network Adapters & Wi-Fi History**
    - Registry Paths:
        - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged` (Wi-Fi/Ethernet networks ever connected)
        - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles` (Wi-Fi profile names and timestamps)
    - Forensics: Reveals networks accessed by the system.
- **Installed Drivers**
    - Registry Path: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services`
    - Description: Lists all installed drivers and services.
    - Forensics: Checks for unauthorized or malicious drivers.

---

### 📂 3. Recently Opened Files (Office, PDF, Apps, etc.)

- **Microsoft Office Recent Files**
    - Word: `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\16.0\Word\File MRU`
    - Excel: `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\16.0\Excel\File MRU`
    - PowerPoint: `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\16.0\PowerPoint\File MRU`
    - Note: Replace `16.0` with version number (e.g., `15.0` for Office 2013).
    - Forensics: Tracks recently accessed Office documents.
- **Adobe Acrobat Recent Files**
    - Registry Path: `HKEY_CURRENT_USER\SOFTWARE\Adobe\Acrobat Reader\DC\AVGeneral\cRecentFiles`
    - Forensics: Tracks recently opened PDFs.
- **Windows Recent Documents (Legacy)**
    - Registry Path: `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs`
    - Forensics: Lists recently accessed files regardless of application.
- **RunMRU (Command History in Run Dialog)**
    - Registry Path: `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RunMRU`
    - Forensics: Shows commands previously executed via the Run dialog (`Win + R`).

---

### ⚙️ 4. System Boot & Installation

- **Windows Installation Date**
    - Registry Path: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion`
    - Key: `InstallDate` (Unix timestamp)
    - Forensics: Shows when Windows was installed.
- **Last Shutdown Time**
    - Registry Path: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Windows`
    - Key: `ShutdownTime` (Binary timestamp)
    - Forensics: Indicates last system shutdown time.
- **Boot Execute (Malware Persistence Check)**
    - Registry Path: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\BootExecute`
    - Description: Lists executables that run at system boot.
    - Forensics: Useful for detecting boot-time malware.

---

### 📌 5. Installed Software & Uninstall Keys

- **Installed Programs (64-bit and 32-bit)**
    - 64-bit: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall`
    - 32-bit (WoW64): `HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall`
    - Forensics: Lists all installed software.
- **Windows Updates (KB Patches)**
    - Registry Path: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing\Packages`
    - Forensics: Shows installed Windows updates.

---

### 🚀 6. Autorun & Startup Programs

- **User-Level Startup**
    - `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
    - `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`
- **System-Wide Startup**
    - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
    - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`
- **Scheduled Tasks**
    - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tasks`
    - Forensics: Identifies programs set to run automatically.

---

### 🌐 7. Browser & Internet Activity

- **Internet Explorer (Legacy)**
    - Typed URLs: `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Internet Explorer\TypedURLs`
    - Download History: `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Internet Explorer\Download`
- **Edge/Chrome**
    - Extensions: `HKEY_CURRENT_USER\SOFTWARE\Google\Chrome\Extensions`
    - *Note:* Most browsing data stored in SQLite databases, but some info is in the registry.

---

### 🔌 8. Remote Desktop & Terminal Services

- **RDP Connection History**
    - `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Terminal Server Client\Servers`
    - Lists previously connected RDP servers.
- **RDP Credentials (If Saved)**
    - `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Terminal Server Client\Default`

---

### 🕒 9. Timezone & Language Settings

- **System Timezone**
    - `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation`
- **Keyboard Layouts**
    - `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layouts`

---
