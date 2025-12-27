# ☁️  **Azure Integration**

---

## ✅ Topics Covered:

1. 🔁 **Azure AD vs Active Directory Domain Services (AD DS)**
2. 🔄 **Hybrid Azure AD Join**
3. 🔧 **Azure AD Connect** (Directory Sync)
4. 📱 **Microsoft Intune & Endpoint Manager**

---

## 1️⃣ Azure AD vs Active Directory (AD DS)

---

| Feature | Azure Active Directory (Azure AD) | Active Directory Domain Services (AD DS) |
| --- | --- | --- |
| **Type** | Cloud-based identity provider (IDaaS) | On-premises directory service |
| **Authentication** | OAuth, OpenID, SAML | Kerberos, NTLM |
| **Protocols** | HTTP/HTTPS, REST APIs | LDAP, SMB, RPC |
| **Device Management** | Cloud-joined, Intune | Domain-joined PCs, GPO |
| **Use Cases** | SaaS (Microsoft 365, Azure), Remote access | On-premises infra (File Server, AD FS, etc.) |
| **Management** | Azure Portal / PowerShell | AD Users & Computers (dsa.msc), GPMC |
| **Group Policy** | ❌ Not supported | ✅ Supported |

---

🔹 **Important**: Azure AD ≠ AD in the cloud. They serve different purposes.

🔹 **Hybrid is the bridge** between the two.

---

## 2️⃣ Hybrid Azure AD Join

---

### 🔍 What is Hybrid Join?

A **Hybrid Azure AD Join** allows **domain-joined devices** (on-prem AD DS) to also **register in Azure AD**. This enables **single sign-on (SSO)** to cloud apps and **conditional access policies** while keeping domain management.

---

### 💼 Use Case:

- A company with on-prem Windows Server AD that also uses **Microsoft 365**, **Intune**, or **Azure-based** apps.

---

### ✅ Requirements:

- Windows 10/11 or Server 2016+ devices
- On-prem Active Directory + Azure AD
- Azure AD Connect configured

---

### 🔧 Step-by-Step GUI Setup

### Step 1: Check Azure AD Connect is Installed

- Log into your Azure AD Connect server.
- Open **Azure AD Connect** tool.
- Click **"Configure Device Options"** > Next.

### Step 2: Enable Hybrid Azure AD Join

1. Select **"Configure device options"** > Next.
2. Choose **"Hybrid Azure AD Join"** > Next.
3. Select **Windows 10 or later**.
4. Enter **AD forest** and credentials.
5. Complete configuration and **sync devices**.

---

### Step 3: Confirm Device Registration

On any domain-joined PC:

```bash
dsregcmd /status

```

Check the following:

- **AzureADJoined**: YES
- **DomainJoined**: YES
- **DeviceId** and **TenantId** present

---

## 3️⃣ Azure AD Connect

---

### 🔍 What is Azure AD Connect?

A tool that **synchronizes users, passwords, groups, and devices** from **on-prem Active Directory → Azure AD**.

---

### ✅ Key Features:

- Password hash synchronization (PHS)
- Pass-through authentication (PTA)
- Federation support (AD FS)
- Scheduled sync every 30 minutes

---

### 🔧 Installation (GUI)

### Step 1: Download & Install

- Download Azure AD Connect:
    
    [Download Link](https://www.microsoft.com/en-us/download/details.aspx?id=47594)
    
- Install on a **domain-joined** server (not DC recommended)

### Step 2: Basic Setup

1. Choose **"Express Settings"** (for small environments)
    
    Or use **"Custom Setup"** for:
    
    - Filtering
    - Password writeback
    - Multiple forests
2. Enter **Global Azure AD Admin credentials**.
3. Enter **on-prem AD credentials**.
4. Configure **password sync**, **device sync**, or **Hybrid join**.
5. Start initial sync and verify in Azure AD.

---

### 🔧 Post-Setup Check

Run:

```powershell
Get-ADSyncScheduler
Start-ADSyncSyncCycle -PolicyType Delta

```

Verify users/devices in Azure AD:

- Go to [**https://portal.azure.com](https://portal.azure.com/) → Azure AD → Users / Devices**

---

## 4️⃣ Intune & Microsoft Endpoint Manager

---

### 🔍 What is Intune?

Microsoft Intune is a **cloud-based endpoint management tool** used to:

- Manage Windows, macOS, Android, iOS
- Push apps, apply compliance policies
- Remotely wipe, lock, configure devices

---

### 🛠 Setup Steps:

### Step 1: Assign Licenses

- Assign **Microsoft 365 E3/E5**, **Enterprise Mobility + Security (EMS)**, or **Intune** license to users.

---

### Step 2: Configure MDM Authority

1. Go to:
    
    [https://intune.microsoft.com](https://intune.microsoft.com/)
    
    (or in Azure: Intune → Device Enrollment)
    
2. Set **MDM authority** to **Intune**.

---

### Step 3: Configure Auto-Enroll (for Windows 10/11)

1. Go to **Azure Portal → Azure AD → Mobility (MDM and MAM)**.
2. Select **Microsoft Intune**.
3. Enable **MDM user scope** for your group or all users.

---

### Step 4: Join Device to Azure AD + Intune

On client PC:

- Go to **Settings → Accounts → Access work or school → Connect**
- Sign in with Azure AD account
- Device becomes **Azure AD joined + Intune enrolled**

---

### 💻 Verify Enrollment

- Login to **Microsoft Endpoint Manager Admin Center**
- Devices → Windows → Look for device
- Confirm **compliance**, **configurations**, **policies** applied

---

### ✅ What You Can Do with Intune

| Action | Description |
| --- | --- |
| Deploy Apps | MSI, EXE, MS Store, Win32 apps |
| Apply Security Baselines | Windows, Edge, Defender configuration profiles |
| Push Compliance Policies | Require BitLocker, AV, secure boot, OS version check |
| Conditional Access | Block access to apps if device is non-compliant |
| Remote Wipe / Lock | Reset or lock lost/stolen devices |

---

## 🔐 Security Tip: Use Conditional Access + MFA

- Combine **Hybrid Join + Intune + Conditional Access** to secure access to corporate resources.
- Example: **Block Microsoft 365 access from non-compliant devices**.
- Configure in:
    
    Azure AD → Security → Conditional Access
    

---

## 📚 Additional Resources

| Topic | Resource / Link |
| --- | --- |
| Azure AD vs AD DS | [Microsoft Docs](https://learn.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-whatis) |
| Hybrid Azure AD Join Setup | [Docs](https://learn.microsoft.com/en-us/azure/active-directory/devices/hybrid-azuread-join-plan) |
| Azure AD Connect Docs | [Docs](https://learn.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-custom) |
| Intune Deployment Guide | [Docs](https://learn.microsoft.com/en-us/mem/intune/fundamentals/intune-enrollment-windows) |
| Endpoint Manager | [Admin Portal](https://endpoint.microsoft.com/) |

---

# ☁️  **Windows 365 & Azure Virtual Desktop (AVD)**

---

## 🚀 What You'll Learn

| Feature | Focus |
| --- | --- |
| **Windows 365 Basics** | What is Cloud PC, licensing, use cases |
| **Azure Virtual Desktop (AVD)** | Setup, host pools, FSLogix, scaling |
| **Remote Management of VMs** | Using Intune, RDP, Azure tools |

---

## 1️⃣ **Windows 365 (Cloud PC)** — Deep Overview & Setup

---

### 🧠 What is Windows 365?

- **Cloud PC**: Personal, persistent desktop running in the cloud (not shared).
- Hosted and managed fully by Microsoft (no infrastructure setup required).
- Users access it via browser or Remote Desktop client.

---

### 🧾 Use Cases:

- Work-from-anywhere for employees
- Secure remote work for contractors
- BYOD (Bring Your Own Device) environments

---

### 🎯 Key Features:

- Always-on Cloud PC with dedicated resources
- Auto-provisioned and assigned to users
- Seamless Azure AD + Intune integration
- Fully managed by Microsoft (no manual VM patching)

---

### ✅ Requirements:

- Microsoft 365 E3/E5 or Business Premium licenses +
    
    **Windows 365 Business** or **Enterprise** add-on license
    
- Azure AD
- Microsoft Intune (for Enterprise edition)

---

### 🛠 Windows 365 Setup (Enterprise)

---

### Step 1: Licensing

- Assign **Windows 365 Enterprise licenses** to users in Microsoft 365 Admin Center.

### Step 2: Prepare Microsoft Endpoint Manager (Intune)

1. Go to: [https://endpoint.microsoft.com](https://endpoint.microsoft.com/)
2. Ensure **Intune MDM authority is set**.
3. Create **Provisioning Policy** for Cloud PCs:
    - Devices → Windows 365 → Provisioning policies
    - Choose region, network (Azure VNet or Microsoft-hosted)
    - Assign to security group (users who will get Cloud PCs)

### Step 3: Assign Users

- Add users to the security group tied to the provisioning policy.
- Cloud PCs will be **auto-deployed** for each user.

### Step 4: Connect to Cloud PC

- End users go to: [https://windows365.microsoft.com](https://windows365.microsoft.com/)
    
    → Sign in → Click on **"Open in browser"** or **Remote Desktop App**
    

---

### 🎯 Windows 365 Admin Tasks

- Manage with **Microsoft Endpoint Manager (Intune)**.
- Assign compliance policies, deploy apps, track usage.
- Use **Endpoint analytics** for performance monitoring.

---

## 2️⃣ **Azure Virtual Desktop (AVD)** — Deep Practical Setup

---

### 🧠 What is AVD?

- **Pooled** or **personal desktop environments** hosted on Azure VMs.
- Fully customizable — unlike Windows 365, you manage the infra.
- Ideal for shared desktops, apps, or remote development environments.

---

### 🧾 AVD Key Components

| Component | Description |
| --- | --- |
| Host Pool | Group of session hosts (VMs) |
| Session Hosts | The actual VMs running Windows 10/11 |
| Application Group | Set of Remote Apps or Desktops |
| Workspace | Logical group where users connect |
| FSLogix | Profile container for roaming user profiles |

---

### ✅ Prerequisites:

- Azure subscription
- Azure AD + optionally on-prem AD
- Domain-joined or Azure AD-joined VMs
- Microsoft 365 or AVD-eligible licenses

---

### 🔧 AVD Setup Steps (GUI)

---

### Step 1: Setup Networking

- Create a **Virtual Network** in Azure
- Ensure DNS can resolve your domain (if hybrid join)

---

### Step 2: Deploy AVD Host Pool

- Go to Azure Portal → Search for **Azure Virtual Desktop**
- Click **Create a host pool**
    - Choose: **Pooled or Personal**
    - VM size (e.g., D2s_v3), number of session hosts
    - Provide **domain join credentials**

---

### Step 3: Configure App Group

- Automatically created during host pool setup.
- Can create **Remote App Group** or **Desktop Group**.
- Add users who should access desktops or apps.

---

### Step 4: Workspace Setup

- Link **host pool’s app group** to a **Workspace**.
- Users will access AVD via Workspace (RD client or web).

---

### Step 5: FSLogix Profile Container (Optional but Recommended)

- Install FSLogix agent on session hosts.
- Use **Azure Files** or **Azure NetApp Files** for user profile storage.

```powershell
# Sample FSLogix configuration via registry:
New-ItemProperty -Path "HKLM:\Software\FSLogix\Profiles" -Name "VHDLocations" -Value "\\storageaccount.file.core.windows.net\profileshare"

```

---

### Step 6: Connect as a User

- Users go to:
    
    https://rdweb.wvd.microsoft.com/arm/webclient
    
- Or use **Remote Desktop Client** for Windows/macOS with AVD support.

---

### 🔧 Remote Management of VMs

| Method | Tool or Portal | Usage |
| --- | --- | --- |
| **Azure Portal** | portal.azure.com → VMs | Start/Stop, connect, reset passwords |
| **Remote Desktop (RDP)** | Built-in Remote Desktop client | GUI access to VMs |
| **Intune / MEM** | endpoint.microsoft.com | App deployment, compliance, wipe, etc. |
| **PowerShell / CLI** | `az vm` or `Get-AzVM` | Scripting and automation |
| **Azure Bastion** | Secure RDP in browser, no public IP needed | Secure web-based VM access |

---

## 🧠 Windows 365 vs AVD — Which to Choose?

| Feature | Windows 365 | Azure Virtual Desktop (AVD) |
| --- | --- | --- |
| Management Model | Fully managed (SaaS) | You manage infra (PaaS/IaaS) |
| Desktop Type | Personal (dedicated) | Pooled or personal |
| Cost Model | Fixed per-user/month | Usage-based (per hour) |
| Scaling | Auto-scaling not required | Manual or scripted scaling |
| Customization | Limited | Full control over images, apps |
| Ideal For | Simplicity, predictable costs | Complex, custom or app-sharing setups |

---

## 🔐 Security & Best Practices

- Use **Conditional Access** for login protection
- Enable **MFA** for users accessing Cloud PCs or AVD
- Regularly patch AVD VMs (via Update Management or Intune)
- Monitor performance with **Azure Monitor** or **Log Analytics**
- Enable **BitLocker + Azure Disk Encryption** for session hosts

---

## 📚 Key Resources

| Topic | Docs Link |
| --- | --- |
| Windows 365 Overview | [Windows 365 Docs](https://learn.microsoft.com/en-us/windows-365/) |
| AVD Full Setup | [AVD Docs](https://learn.microsoft.com/en-us/azure/virtual-desktop/) |
| FSLogix Setup | [FSLogix Docs](https://learn.microsoft.com/en-us/fslogix/) |
| Intune for VMs | [Intune Docs](https://learn.microsoft.com/en-us/mem/intune/) |
| Azure Bastion | [Bastion Docs](https://learn.microsoft.com/en-us/azure/bastion/) |

---

##
