# 

## 5️⃣ WINDOWS SERVER EDITIONS & ROLES

---

### 🔍 Core Concepts

### 1. **Windows Server Versions Overview**

| Version | Released | Key Features | Support Status |
| --- | --- | --- | --- |
| **2012** | Sep 2012 | Introduced Server Manager overhaul, Hyper-V enhancements, IPAM | Mainstream ended 2018 |
| **2016** | Oct 2016 | Nano Server, Containers, Shielded VMs | Supported |
| **2019** | Oct 2018 | Hybrid Cloud, Windows Admin Center, Kubernetes | Supported |
| **2022** | Aug 2021 | Secured-core, improved Azure integration | Supported |

---

### 2. **Server Core vs Desktop Experience**

| Feature | Server Core | Desktop Experience (Full GUI) |
| --- | --- | --- |
| GUI | ❌ None (CLI only) | ✅ Full Windows GUI |
| Footprint | Smaller | Larger |
| Security | Reduced attack surface | Full GUI attack surface |
| Management | PowerShell, Remote Server Admin Tools (RSAT) | Local GUI + PowerShell + RSAT |
| Recommended Usage | Production, headless servers, containers | Legacy apps, user-friendly management |

---

### 3. **Server Roles & Features**

- **Roles:** Major server functions (e.g., Active Directory Domain Services, DNS, DHCP, File Services)
- **Features:** Supporting components (e.g., .NET Framework, Telnet Client)

---

### 🛠️ Installation & Configuration Methods

---

### A. **Using Server Manager (GUI)**

1. Open **Server Manager** → Dashboard → Add roles and features.
2. Use the **Wizard** to select:
    - Role-based or feature-based installation.
    - Select target server.
    - Choose roles/features.
3. Confirm and install, then **restart if needed**.

---

### B. **PowerShell Installation**

```powershell
# List available roles and features
Get-WindowsFeature

# Install AD Domain Services
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools

# Install DNS Server role
Install-WindowsFeature -Name DNS -IncludeManagementTools

# Install File Server role
Install-WindowsFeature -Name FS-FileServer

# Check installed features
Get-WindowsFeature | Where-Object {$_.InstallState -eq "Installed"}

```

---

### C. **Using DISM (Deployment Image Servicing and Management)**

```bash
# List available roles/features
dism /online /get-features /format:table

# Enable feature (e.g., Telnet Client)
dism /online /enable-feature /featurename:TelnetClient

# Disable feature
dism /online /disable-feature /featurename:TelnetClient

```

---

### D. **Server Core Management**

- **No GUI** — manage via:
    - PowerShell Remoting
    - sconfig.cmd (Server Configuration Tool)
    - Remote Server Administration Tools (RSAT) from another machine

```powershell
# Enable PowerShell Remoting (default on Server Core)
Enable-PSRemoting -Force

# Connect remotely
Enter-PSSession -ComputerName ServerCore01 -Credential Administrator

```

---

### 🔧 Important Server Roles Overview

| Role Name | Description | PowerShell Cmdlet |
| --- | --- | --- |
| **Active Directory Domain Services (AD DS)** | Domain controller service | Install-WindowsFeature AD-Domain-Services |
| **DNS Server** | Domain Name System resolver | Install-WindowsFeature DNS |
| **DHCP Server** | Dynamic IP address allocation | Install-WindowsFeature DHCP |
| **File and Storage Services** | File server roles, storage management | Install-WindowsFeature FS-FileServer |
| **Web Server (IIS)** | Internet Information Services | Install-WindowsFeature Web-Server |
| **Hyper-V** | Virtualization platform | Install-WindowsFeature Hyper-V |

---

### 

### 🧰 Recommended Tools & Resources

- **Server Manager** (GUI for roles/features)
- **PowerShell ISE** or VSCode with PowerShell extension
- **RSAT (Remote Server Administration Tools)**
- **sconfig** on Server Core
- **Books**: *Windows Server 2019 & 2022 Administration* (by William Stanek, etc.)
- **Microsoft Docs**: Roles and features overview

---

### ✅ Summary Table

| Area | GUI Tools | CLI/PowerShell Tools | Notes |
| --- | --- | --- | --- |
| Install roles | Server Manager Wizard | Install-WindowsFeature, dism | PowerShell preferred for automation |
| List roles/features | Server Manager, Server Manager Dashboard | Get-WindowsFeature, dism /get-features | Useful to audit installed features |
| Manage Server Core | None (Remote GUI) | sconfig, PowerShell remoting | Use remote RSAT tools |
| Manage remotely | RSAT on Windows Client | Enter-PSSession, Invoke-Command | Secure and scriptable management |

---

### 🏷️ 1. **Windows Server Versions**

Here are the main versions still used in companies and labs:

| Version | Released | Support Ends | Key Features |
| --- | --- | --- | --- |
| **2012 / 2012 R2** | 2012 / 2013 | *Support ended in Oct 2023* | Legacy; stable for basic AD, DHCP, DNS |
| **2016** | 2016 | Oct 2027 | Introduced Nano Server, improved PowerShell, containers |
| **2019** | 2018 | Jan 2029 | Better security, hybrid cloud features |
| **2022** | 2021 | Oct 2031 | Most secure, better performance, Azure integration |

> ✅ Recommendation for learning: Use Windows Server 2019 or 2022 (available as free trial ISO from Microsoft).
> 

---

### 🖥️ 2. **Server Core vs Desktop Experience**

| Type | Description | Use Case |
| --- | --- | --- |
| **Server Core** | Minimal GUI (Command line & PowerShell only) | **Lightweight, secure**; best for production servers |
| **Desktop Experience** | Full Windows GUI (looks like Windows 10/11) | Best for **learning and ease of use** |

> ✅ For beginners: Use Desktop Experience so you can learn with the full GUI, then later try Core for PowerShell practice.
> 

---

### 🧰 3. **What Are Server Roles and Features?**

**Server Roles** = main functions a server can perform.

**Features** = smaller tools that support roles.

🧩 **Examples of Server Roles**:

- **Active Directory Domain Services (AD DS)** → User/domain management
- **DHCP Server** → Assign IPs automatically
- **DNS Server** → Domain name resolution
- **File and Storage Services** → Share files over a network
- **Web Server (IIS)** → Host websites

🛠️ **Examples of Features**:

- **.NET Framework**, **Windows PowerShell**, **Failover Clustering**, etc.

---

### 🧪 4. **How to Install Server Roles (Step-by-Step)**

### ✅ Method 1: Using Server Manager (GUI)

1. Open **Server Manager**
2. Click **“Add roles and features”**
3. Click **Next** until you reach **“Server Roles”**
4. Choose a role (e.g., **DHCP Server**, **AD DS**, etc.)
5. Click **Next**, add any **features** required
6. Click **Install**
7. After installation, complete the **Post-Installation Wizard** if needed

### ✅ Method 2: Using PowerShell

Here’s how to install **DNS Server** using PowerShell:

```powershell
Install-WindowsFeature -Name DNS -IncludeManagementTools

```

To see all roles/features:

```powershell
Get-WindowsFeature

```

---

### 

### 🧩 **What Is AD DS (Active Directory Domain Services)?**

AD DS is a **centralized database** that:

- Stores information about **users**, **computers**, **groups**, **policies**, etc.
- Provides **authentication** (login) and **authorization** (permissions).
- Makes centralized **management** of networks possible.

---

## 🔧 PART 1: **AD DS Architecture** (Visual & Conceptual)

### 🧱 **Basic Components**

| Component | Description |
| --- | --- |
| **Domain** | A logical group of objects (users, computers, etc.) under one administration |
| **Tree** | A group of domains in a **namespace** (e.g. `corp.local`, `sub.corp.local`) |
| **Forest** | One or more trees that share a **common schema** & **trusts** |
| **OU (Organizational Unit)** | Containers inside a domain used to group objects for easier management (e.g. HR, IT, Sales) |
| **Sites** | Represent physical locations to control **replication** traffic between servers |
| **Trusts** | Relationship between domains/forests to allow access to resources |

🧠 **Visual Map**:

```
Forest: company.local
└── Tree: company.local
    ├── Domain: company.local
    │   ├── OU: HR
    │   ├── OU: IT
    │   └── OU: Sales
    └── Domain: branch.company.local (child domain)

```

---

## 🔧 PART 2: **FSMO Roles (Flexible Single Master Operations)**

There are **5 FSMO Roles**, divided into **2 types**:

### 🔹 Forest-wide Roles:

| Role | Description |
| --- | --- |
| **Schema Master** | Controls changes to AD schema |
| **Domain Naming Master** | Controls adding/removing domains in the forest |

### 🔸 Domain-wide Roles:

| Role | Description |
| --- | --- |
| **RID Master** | Allocates user/computer IDs |
| **PDC Emulator** | Syncs time; fallback for logins; handles password changes |
| **Infrastructure Master** | Maintains references between objects in different domains |

🔍 View current FSMO role holders:

```powershell
netdom query fsmo

```

---

---

### ✅ STEP 1: Install AD DS Role

1. Open **Server Manager**
2. Click **Manage > Add Roles and Features**
3. Choose **Role-based or feature-based installation**
4. Select your server
5. In **Server Roles**, check: **Active Directory Domain Services**
6. Add features when prompted
7. Click **Next** → **Install**

📸 *Visual Aid* (summarized):

```
Server Manager > Add Roles > AD DS > Install

```

---

### ✅ STEP 2: Promote Server to Domain Controller

1. After install, in Server Manager, click:
    - **"Promote this server to a domain controller"**
2. Choose:
    - **"Add a new forest"** (for first-time setup)
    - Root domain name: `corp.local` (or anything)
3. Set **DSRM password** (used for recovery)
4. Accept defaults for DNS and NetBIOS
5. Confirm and install

🔁 Server will reboot after promotion.

---

### ✅ STEP 3: Create OUs, Users, and Groups

Open **Active Directory Users and Computers**:

- Start → type `dsa.msc` → Enter
1. Right-click domain name → **New > Organizational Unit**
    - Example: HR, IT, Sales
2. Right-click OU → **New > User**
    - Create test users: Alice, Bob, etc.
3. Right-click OU → **New > Group**
    - Create Security Groups (e.g. "HR-Managers")

---

### ✅ STEP 4: Add a Client PC to the Domain

1. Install **Windows 10/11** in another VM
2. Set DNS to point to your **Domain Controller IP**
3. Go to:
    - Settings → System → About → **Join a Domain**
4. Enter domain: `corp.local`
5. Login using domain user (e.g. `corp\alice`)

---

## 

| Topic | What to Learn |
| --- | --- |
| **Group Policy (GPO)** | Apply policies like disabling USB, wallpaper, etc. per OU |
| **Sites and Services** | Configure replication between multiple locations |
| **Trusts** | Create trust between two domains or forests |
| **AD Certificate Services** | Setup internal CA (PKI) |
| **AD Recycle Bin** | Recover deleted AD objects |
| **PowerShell AD Module** | Use `Get-ADUser`, `New-ADUser`, `Set-ADGroup` etc. |

---

## 🧪 Bonus PowerShell Commands for AD Practice

| Task | Command |
| --- | --- |
| Create user | `New-ADUser -Name "Alice" -AccountPassword (ConvertTo-SecureString "P@ssw0rd" -AsPlainText -Force) -Enabled $true` |
| Create OU | `New-ADOrganizationalUnit -Name "IT" -Path "DC=corp,DC=local"` |
| Add user to group | `Add-ADGroupMember -Identity "HR-Managers" -Members "Alice"` |
| Search users | `Get-ADUser -Filter *` |

---

## 

## **Group Policy (GPO)**

---

## 🔍 PART 1: **What is GPO (Group Policy Object)?**

Group Policy allows administrators to:

- Control **user** and **computer** settings centrally
- Apply **security rules**, **software settings**, **login restrictions**, and more
- Manage multiple systems **without touching each one manually**

---

### 🧱 GPO Structure & Inheritance

| Component | Description |
| --- | --- |
| **Group Policy Object (GPO)** | A set of policies/settings |
| **Local GPO** | Stored on individual machines |
| **Domain GPO** | Stored and applied via Active Directory |
| **Linked GPO** | GPOs must be *linked* to an OU/domain/site to apply |
| **Inheritance** | GPOs flow from **Site → Domain → OU** |
| **Precedence** | **Closer GPOs (OU-level)** override domain/site level ones |
| **Block Inheritance** | Prevents GPOs from parent domains/OUs from applying |
| **Enforce** | Forces GPO to apply, ignoring block inheritance |

🔁 **Order of Application**:

`Local GPO → Site GPO → Domain GPO → OU GPO`

👉 OU wins if conflicts exist.

---

## 🛠️ PART 2: **GPO Practical Setup (GUI)**

### 🔧 LAB REQUIREMENTS:

- Domain Controller (with AD + GPO)
- At least 1 domain-joined client (Windows 10/11)
- Users organized into OUs (e.g. HR, IT)

---

### ✅ **Step-by-Step: Create & Link a GPO**

### Example: Disable Control Panel for "HR" users

1. Open **Group Policy Management** (`gpmc.msc`)
2. Expand your domain → Right-click **OU: HR**
3. Click **"Create a GPO in this domain, and Link it here…"**
    - Name it: `Disable Control Panel`
4. Right-click the new GPO → **Edit**
5. Go to:
    
    `User Configuration → Policies → Administrative Templates → Control Panel`
    
6. Double-click **"Prohibit access to Control Panel and PC settings"**
    
    → Set to **Enabled**
    

📌 Log in as a user in the HR OU on the client and try to open Control Panel → It will be blocked!

---

## 🔒 PART 3: **Security Settings via GPO**

### 🔐 Example 1: Set Password Policy

1. Open Group Policy Management
2. Edit **Default Domain Policy** (or create a new one)
3. Navigate:
    
    `Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy`
    

Set:

- Minimum password length
- Password complexity
- Maximum password age

---

### 🔐 Example 2: Disable USB Access

1. Create new GPO: `Disable USB`
2. Edit GPO:
    
    `Computer Configuration → Policies → Administrative Templates → System → Removable Storage Access`
    
3. Set **"All Removable Storage classes: Deny all access"** to **Enabled**
4. Link GPO to OU containing client machines

✅ Test: On client PC, USB will not be accessible.

---

## 💾 PART 4: **Software Deployment via GPO**

### 📦 Example: Deploy `.msi` Software (like 7-Zip)

### 🔧 Step-by-step:

1. Place the `.msi` installer file (e.g. `7z.msi`) in a shared folder:
    - e.g. `\\DC\Share\7z.msi`
    - Set **Read** permissions for `Domain Computers`
2. Create a new GPO: `Install 7-Zip`
3. Edit GPO:
    - `Computer Configuration → Policies → Software Settings → Software Installation`
    - Right-click → New → **Package**
    - Enter UNC path: `\\DC\Share\7z.msi`
    - Choose **Assigned**
4. Link GPO to an OU with computers
5. Reboot client → Software will install automatically at startup

---

## 🔍 PART 5: **GPO Troubleshooting Tools**

### 🛠️ Tool 1: **gpresult**

Shows applied GPOs per user or computer

```bash
gpresult /r

```

Use on client machine. It shows:

- User and Computer policies
- Applied GPOs and denied ones

### 🛠️ Tool 2: **Resultant Set of Policy (RSOP)**

Graphical tool

1. Run `rsop.msc` on a client PC
2. It shows **actual GPO settings** that applied to the user or computer
3. Helps find **conflicts or missing policies**

### 🛠️ Tool 3: **Group Policy Modeling**

In Group Policy Management:

- Right-click Group Policy Modeling → Launch wizard
- Simulates GPOs for specific user/computer

---

## 🧪 BONUS: ADVANCED GPO PRACTICES

| Task | How |
| --- | --- |
| **Logon Script** | `User Configuration → Windows Settings → Scripts` |
| **Map Network Drive** | `User Configuration → Preferences → Windows Settings → Drive Maps` |
| **Deploy Wallpaper** | `User Configuration → Admin Templates → Desktop → Desktop → Desktop Wallpaper` |
| **Redirect Folders (Documents/Desktop)** | `User Configuration → Windows Settings → Folder Redirection` |

---

# 8. **Active Directory Sites and Services**

### ➤ *Configure replication between multiple locations*

---

### 📘 **Concept Overview**

| Term | What it is |
| --- | --- |
| **Site** | A physical location (e.g., New York, London, etc.) |
| **Subnet** | Defines IP range used in a Site |
| **Site Link** | Connection between sites for replication |
| **Bridgehead Server** | Domain Controller that handles replication traffic |
| **Intersite Replication** | Replication *between* Sites |
| **Intrasite Replication** | Replication *within* the same Site |

💡 **Why use Sites?**

- Control AD replication over WAN links (low bandwidth)
- Help clients locate the nearest Domain Controller
- Simulate real-world distributed environments

---

### 🧪 PRACTICAL LAB – Sites and Services

### 🧰 REQUIREMENTS:

- Two Domain Controllers (e.g. `DC1`, `DC2`)
- One client VM (optional for testing DC locator)
- Different IP ranges for each DC (simulate different locations)

### Example:

- DC1 (New York): IP = 10.1.1.10
- DC2 (London): IP = 10.2.2.10

---

### ✅ Step-by-Step: Configure Sites and Replication

### 1. Open **Active Directory Sites and Services**

`Start > Run > dssite.msc`

### 2. Create New Site

- Right-click **Sites** > **New Site**
- Name it: `London`
- Select **DEFAULTIPSITELINK**
- Click OK

### 3. Create Subnets

- Right-click **Subnets** > **New Subnet**
    - Subnet: `10.1.1.0/24` → Site: `Default-First-Site-Name` (New York)
    - Subnet: `10.2.2.0/24` → Site: `London`

### 4. Move DC2 to the London site

- Expand **Servers**
- Find DC2 under `Default-First-Site-Name`
- Right-click and **Move** to `London`

Now, DC1 and DC2 are in separate sites.

---

### 🕸️ Step 5: Control Replication Schedule

1. Expand **Inter-Site Transports > IP**
2. Double-click **DEFAULTIPSITELINK**
3. Modify:
    - **Cost** (lower = preferred link)
    - **Replication schedule** (e.g., only replicate at night)

---

### 🔍 Step 6: Test Replication

**Force replication:**

```powershell
Repadmin /syncall /e /d /A

```

**View replication topology:**

```powershell
Repadmin /showrepl

```

---

### 🧠 Real-World Tip:

- Place **at least one DC per site**
- Use **subnets** carefully to ensure client machines contact the **nearest DC**
- Use **GPO site targeting** to apply settings based on location

---

## 🔐### 9. **AD Trusts**

### ➤ *Create Trusts Between Two Domains or Forests*

---

### 📘 Concept Overview

| Trust Type | Description |
| --- | --- |
| **Parent–Child Trust** | Automatically created when domains are in same forest |
| **Tree–Root Trust** | Auto when new tree is added |
| **External Trust** | Between domains in **different forests** (one-way or two-way) |
| **Forest Trust** | Between **two forests** |
| **Shortcut Trust** | Speeds up authentication in large trees |
| **Realm Trust** | Trust with **non-Windows** Kerberos realms (like Linux/Unix) |

---

### 🧪 PRACTICAL LAB – Create a Trust

### 🧰 REQUIREMENTS:

- Two Windows Server VMs as Domain Controllers
- Two domains in **separate forests**
    - `corp.local` on DC1
    - `branch.local` on DC2
- Proper **name resolution** between them (via DNS or HOSTS file)

---

### ✅ Step-by-Step: Create Two Forests

### On DC1 (corp.local)

1. Install AD DS → promote to domain: `corp.local`

### On DC2 (branch.local)

1. Install AD DS → promote to **new forest**: `branch.local`

🧠 Now you have two **separate forests** ready for a trust.

---

### 🔁 Step-by-Step: Create a Two-Way Trust

### ✅ On **corp.local** DC:

1. Open **Active Directory Domains and Trusts**
2. Right-click `corp.local` > **Properties**
3. Go to **Trusts** tab → Click **New Trust**
4. Trust Name: `branch.local`
5. Type: **Forest trust**
6. Direction: **Two-way**
7. Enter credentials for `branch.local` domain
8. Complete the wizard

✅ Now repeat the same steps on `branch.local` DC (or allow trust to be created from one side only and confirmed from the other).

---

### 🔐 Step-by-Step: Allow User Access Across Domains

1. Create a user in `branch.local`
2. Add that user to a group in `corp.local`
3. Apply permissions to shared folders, GPOs, etc.

---

### 🔍 Trust Verification

Run this on DC:

```bash
netdom trust branch.local /domain:corp.local /verify

```

---

### 🔒 Optional: DNS Setup for Name Resolution

If not using forwarding:

- Add conditional forwarders in each DNS
    - In `corp.local` DNS, forward `branch.local` to DC2 IP
    - In `branch.local` DNS, forward `corp.local` to DC1 IP

Or edit HOSTS file for testing:

```bash
C:\Windows\System32\drivers\etc\hosts
10.1.1.10 corp.local
10.2.2.10 branch.local

```

---

## 💡 NEXT-LEVEL IDEAS

| What to Try | How |
| --- | --- |
| 🗂️ Delegate admin to branch.local for shared resource access | Use trust permissions |
| 🛑 Block replication between sites on weekends | Edit **Site Link Schedule** |
| 🌎 Build 3-Site network | New York, London, Tokyo — replicate using site links |
| 🧠 Combine Sites + GPO targeting | Apply GPOs per location using `Item-Level Targeting` |

---

## 

# **Active Directory Certificate Services (AD CS)**

### ➤ Setup Internal CA for PKI in Windows Server

---

## 📘 What is AD CS?

**Active Directory Certificate Services** (AD CS) provides a **Certificate Authority (CA)** that issues and manages **digital certificates** in your AD domain.

### 🧱 Key PKI Components

| Component | Description |
| --- | --- |
| **CA (Certificate Authority)** | Issues and signs certificates (like a passport office) |
| **Certificate Templates** | Define how certificates are issued (for users, computers, etc.) |
| **CRL (Certificate Revocation List)** | List of revoked certs |
| **Autoenrollment** | Users/computers automatically get certificates |
| **Enterprise CA** | Integrated with AD for automatic issuance |
| **Standalone CA** | No AD integration (manual approval needed) |

> ✅ For domain environments, we use Enterprise Root CA.
> 

---

## 🧪 LAB SETUP – Install Internal PKI (Enterprise Root CA)

### 🧰 Requirements:

- Windows Server 2019/2022
- Domain joined
- AD DS already set up

---

## ✅ Step-by-Step: Install & Configure AD CS (GUI)

### 📦 1. Install AD CS Role

1. Open **Server Manager**
2. Click **Add Roles and Features**
3. Role-based installation → Select your server
4. Under **Server Roles**, select:
    - ✅ **Active Directory Certificate Services**
    - Optional: **Certification Authority Web Enrollment** (for web-based requests)
5. Click **Next** → Add required features → **Install**

---

### 🛠️ 2. Configure AD CS

Once installed:

1. In Server Manager → Click **flag icon** → **"Configure Active Directory Certificate Services on this server"**
2. Check:
    - ✅ **Certification Authority**
    - (Optional) **CA Web Enrollment** for web access
3. Click **Next**

---

### 🧰 3. Set Up CA Type

1. Select: ✅ **Enterprise CA**
2. CA Type: ✅ **Root CA**
3. Create new private key
4. Cryptography:
    - RSA 2048 or 4096 (default is fine)
5. Common name: auto-filled (e.g., `corp-DC1-CA`)
6. Validity Period: 5 or 10 years
7. Confirm → Install

✅ Your internal **Enterprise Root CA** is now running.

---

### 🌐 4. (Optional) Configure Web Enrollment

> If you installed Web Enrollment:
> 
1. Open browser → go to:
    
    ```
    http://<CA-SERVER>/certsrv
    
    ```
    
2. You can now:
    - Request a certificate
    - Download CA certificate
    - Check pending requests

📌 Web Enrollment uses **IIS** under the hood.

---

## 🔐 5. Test Certificate Auto-Enrollment

Let’s issue a certificate to a domain computer automatically.

### 📌 Step-by-Step:

### a. Open **Certification Authority** Console

- `Start > certsrv.msc`

### b. Enable Autoenrollment in GPO:

1. Open **Group Policy Management**
2. Edit **Default Domain Policy** (or new GPO)
3. Navigate:
    
    ```
    Computer Configuration → Policies → Windows Settings → Security Settings → Public Key Policies
    
    ```
    
4. Enable:
    - **Certificate Services Client - Auto-Enrollment**
        - ✅ Enabled
        - ✅ Renew expired certificates
        - ✅ Update certificates

✅ Now, computers will auto-enroll for certs from your internal CA.

### c. Force GP Update + Enrollment

On a domain-joined client (or the server):

```bash
gpupdate /force
certutil -pulse

```

Then run:

```bash
certmgr.msc

```

→ Under **Personal > Certificates**, you’ll see a cert issued by your CA.

---

## 🧰 Real-World Use Cases

| Use Case | How it Works |
| --- | --- |
| **HTTPS for internal sites** | Bind SSL cert from CA to IIS |
| **Smart Card login** | Enroll users with smart card certificates |
| **VPN/802.1x Auth** | Issue computer certs for network access |
| **Code Signing** | Sign PowerShell scripts, apps with internal certs |
| **Encrypt Emails** | Use S/MIME certificates in Outlook |

---

## ⚙️ OPTIONAL: PowerShell Setup (Advanced)

Install CA via PowerShell:

```powershell
Install-WindowsFeature ADCS-Cert-Authority
Install-AdcsCertificationAuthority `
    -CAType EnterpriseRootCA `
    -CryptoProviderName "RSA#Microsoft Software Key Storage Provider" `
    -HashAlgorithmName SHA256 `
    -KeyLength 2048 `
    -ValidityPeriod Years `
    -ValidityPeriodUnits 5

```

---

## 🧪 BONUS: Create a Certificate Template

1. Open **certsrv.msc**
2. Right-click **Certificate Templates > Manage**
3. Duplicate template (e.g., Computer)
4. Name it: "Corp Workstation Certificate"
5. Set:
    - Security → Allow **Domain Computers** to enroll
    - Subject name = From AD
6. Save & publish:
    - Go back to certsrv
    - Right-click **Certificate Templates > New > Certificate Template to Issue**

---

## 🧠 Advanced Topics for Later

| Topic | Use |
| --- | --- |
| **Subordinate CAs** | For multi-layer PKI hierarchy |
| **OCSP Responder** | Real-time cert status checking |
| **CRL Distribution Points** | Define where revoked certs are listed |
| **Key Archival & Recovery** | Recover lost encrypted data |
| **Auto-enroll Users for S/MIME** | Encrypt email with internal PKI |

---

## 

# **Active Directory Recycle Bin**

### ➤ *Recover Deleted AD Objects Without Restoring from Backup*

---

## 📘 What is the AD Recycle Bin?

| Feature | Description |
| --- | --- |
| 🔄 **Recycle Bin** | Stores deleted AD objects (with all attributes!) for a set time |
| 🧠 **Preserves** | Group memberships, passwords, SID, permissions |
| ⏳ **Retention Time** | Default = 180 days (can be changed) |
| 🔐 **Only works in Windows Server 2008 R2+** and **Forest Functional Level = 2008 R2 or higher** |  |

✅ Recovery is done **without rebooting, restoring backup, or restarting services**.

---

## ✅ Step-by-Step: Enable AD Recycle Bin

> 📌 Once enabled, it cannot be disabled.
> 
> 
> Do this **only in lab or production with care.**
> 

---

### 🖥️ METHOD 1: Enable via GUI (Server 2012+)

1. Open **Active Directory Administrative Center (ADAC)**
    
    > Run: dsac.exe
    > 
2. In the left pane, click your domain (e.g. `corp.local`)
3. In the **Tasks pane (right side)**, click:
    - 🟢 **Enable Recycle Bin**
4. You’ll get a warning — accept it.

✅ Done. The Recycle Bin is now active.

AD will **restart replication** across DCs briefly.

---

### ⚙️ METHOD 2: Enable via PowerShell

```powershell
# Check if Recycle Bin is enabled
Get-ADOptionalFeature -Filter 'name -like "*Recycle Bin*"' |
    Format-Table Name, EnabledScopes

# Enable Recycle Bin (permanent)
Enable-ADOptionalFeature `
  -Identity 'Recycle Bin Feature' `
  -Scope ForestOrConfigurationSet `
  -Target 'corp.local'

```

Replace `'corp.local'` with your forest root domain name.

---

## 🧪 LAB: Delete and Restore a User

### 1. Create a test user:

```powershell
New-ADUser -Name "John Doe" -SamAccountName jdoe -AccountPassword (ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force) -Enabled $true

```

OR via GUI: Use **Active Directory Users & Computers (ADUC)**

---

### 2. Delete the user

- Open ADUC → Find `John Doe` → Right-click → Delete

---

### 3. Recover the deleted object

### a. Open **Active Directory Administrative Center (ADAC)**

- Navigate to your domain (left panel)
- Click **“Deleted Objects”** (middle pane)

### b. Right-click on deleted object → **Restore**

✅ It will restore the user **with all attributes intact**: password, group memberships, etc.

---

## ✅ PowerShell Recovery Example

### List all deleted objects:

```powershell
Get-ADObject -Filter 'IsDeleted -eq $True' -IncludeDeletedObjects

```

### Restore by Object GUID:

1. Copy the object GUID from above
2. Run:

```powershell
Restore-ADObject -Identity <GUID>

```

OR restore by name:

```powershell
$del = Get-ADObject -Filter {SamAccountName -eq "jdoe"} -IncludeDeletedObjects
Restore-ADObject -Identity $del.ObjectGUID

```

---

## 🛡️ Permissions Needed to Restore

| Role | Requirement |
| --- | --- |
| Domain Admins | ✅ Can enable Recycle Bin and restore objects |
| Delegated Admins | ❌ Cannot restore unless explicitly granted |
| Backup Operators | ❌ Not enough by default |

You can delegate **"Deleted Object Recovery"** rights via custom permissions (advanced).

---

# DNS & DHCP

---

## ✅ DNS – Domain Name System (Windows Server)

---

### 📘 What is DNS?

| Purpose | What It Does |
| --- | --- |
| 💻 **Name Resolution** | Converts names (like `dc1.corp.local`) to IP addresses |
| 📚 **AD Dependency** | AD *requires* DNS to locate domain controllers |
| ⚙️ **DNS Zones** | Where DNS records live (think of them as folders for domain names) |

---

## 🧰 PART 1: Installing and Configuring DNS Server

### ✅ Step-by-Step: Install DNS Role (GUI)

1. Open **Server Manager**
2. Go to **Add Roles and Features**
3. Choose **Role-based or feature-based**
4. Select your server
5. ✅ Check **DNS Server**
6. Click **Next** → Install
7. After install, open **DNS Manager** (`dnsmgmt.msc`)

---

## 🔧 PART 2: DNS Zones (Core Types)

| Zone Type | Description | Used When |
| --- | --- | --- |
| **Primary Zone** | Read/write copy of DNS data | Main zone (AD-integrated) |
| **Secondary Zone** | Read-only copy from another DNS server | For backup or load distribution |
| **Stub Zone** | Partial copy (NS records only) | Used for faster referrals |

---

### ✅ Create a **Primary Zone** (GUI)

1. Open **DNS Manager**
2. Expand your server → Right-click **Forward Lookup Zones** → New Zone
3. Select:
    - Zone Type: **Primary zone**
    - ✅ Store in Active Directory (if AD-integrated)
    - Replication Scope: To all DNS servers in forest/domain
    - Zone Name: `corp.local`
    - Dynamic Updates: ✅ Allow only secure updates
4. Finish wizard

### ✅ Add DNS Records

- Right-click your zone → New Host (A or AAAA)
    - Name: `webserver`
    - IP: `192.168.10.50`

You’ve created:

```
webserver.corp.local → 192.168.10.50

```

---

### 🔁 Set Up a **Secondary Zone** (on another DNS server)

1. Install DNS role on second server
2. Open DNS Manager → Right-click Forward Lookup Zones → New Zone
3. Choose:
    - Type: **Secondary Zone**
    - Zone Name: `corp.local`
    - Master DNS server: `IP of Primary DNS`

➡️ It pulls a read-only copy of the zone from the primary.

---

### 🧠 BONUS: **Stub Zone**

Stub zones contain **only NS records** to help route queries faster across forests or subdomains.

> Use when you want referrals to another DNS server but don’t want full data replication.
> 

---

## 🌐 PART 3: Dynamic DNS (DDNS)

When clients join AD or get an IP from DHCP, they can auto-register their **A** and **PTR** records.

### To Enable Dynamic DNS:

- Must be **Primary AD-integrated zone**
- In zone settings, set:
    - **Allow only secure updates**
- On client PCs:
    - Enable: `Register this connection’s address in DNS`
    - Or run: `ipconfig /registerdns`

---

## ✅ DHCP – Dynamic Host Configuration Protocol

---

### 📘 What is DHCP?

DHCP automates IP address configuration:

| It Provides | Details |
| --- | --- |
| IP Address | Dynamic or static |
| Subnet Mask | Required for network communication |
| Default Gateway | Router IP |
| DNS Server | DNS IP address(es) |
| Lease Time | How long the IP is valid |

---

## 🛠️ PART 4: Install and Configure DHCP Server

### ✅ Step-by-Step: Install DHCP (GUI)

1. Open **Server Manager → Add Roles and Features**
2. Select:
    - ✅ **DHCP Server**
3. After install, go to **Tools → DHCP**
4. In the **DHCP console**, right-click your server → **Authorize**
5. Refresh — green check = authorized

---

## 📦 PART 5: Create DHCP Scope (IPv4)

A scope defines the range of IPs to lease.

### ✅ Create Scope

1. In DHCP console → Expand IPv4 → Right-click → **New Scope**
2. Name: `LAN-Scope`
3. Start IP: `192.168.10.100`
    
    End IP: `192.168.10.200`
    
4. Subnet Mask: `255.255.255.0`
5. Exclusions: (e.g. `192.168.10.110–115`) – reserved for static IPs
6. Lease Duration: Default 8 days or customize
7. Configure:
    - **Router (Default Gateway)**: `192.168.10.1`
    - **DNS Server**: `192.168.10.10` (your DNS server)
8. Finish

Clients will now receive IP addresses **automatically**.

---

### ✅ Add a DHCP Reservation

For devices that need **static IPs** (e.g. printers, servers):

1. Expand your scope → **Reservations** → Right-click → **New Reservation**
2. Name: `Printer01`
3. IP: `192.168.10.120`
4. MAC Address: `00-11-22-33-44-55`
5. Click Add

💡 Device will always get that IP when it connects.

---

## 🔄 PART 6: Link DHCP with Dynamic DNS

In DHCP console:

1. Right-click **IPv4** → **Properties**
2. Go to **DNS Tab**
3. Set:
    - ✅ Enable DNS dynamic updates
    - ✅ Always dynamically update DNS A and PTR records
    - ✅ Discard A and PTR when lease is deleted

This ensures DHCP clients are auto-added to DNS!

---

## 🧪 TEST IT OUT

1. Boot up a Windows client (e.g. Windows 10 VM)
2. Set it to auto obtain IP and DNS
3. Run:
    
    ```bash
    ipconfig /all
    
    ```
    
    - Confirm IP from DHCP
    - Confirm DNS points to server
4. Check **DNS Manager**:
    - Look under `corp.local` → A record should appear for the new client

---

## 🧠 Bonus Real-World Scenarios

| Task | Tip |
| --- | --- |
| 🖥️ Assign static IP to a server | Use outside DHCP scope (e.g., `192.168.10.10`) |
| 🖨️ Give printers fixed IPs | Use DHCP reservations |
| 🔁 Backup DHCP | `netsh dhcp server export` |
| 🌍 Multiple subnets | Use **DHCP relay agent** (on router or L3 switch) |
| 🔐 Secure DNS updates | Use **secure only** on AD-integrated zones |

---

## ✅ RECAP – You’ve Learned:

### 📡 DNS:

- Installed DNS Server
- Configured Primary, Secondary, and Stub Zones
- Dynamic updates with AD integration

### 📦 DHCP:

- Installed DHCP Server
- Created scopes, leases, and reservations
- Integrated DHCP with DNS for auto-records

---

## 

# **File & Print Services**

---

## ✅ PART 1: FILE SERVER ROLE SETUP

### 📘 What is the File Server Role?

It allows your server to:

| Feature | Use |
| --- | --- |
| 📁 File Sharing | Share folders on the network |
| 🔐 NTFS Permissions | Secure files with access control |
| 📦 Quotas | Limit space per user/folder |
| 🔄 DFS | Sync and replicate shares across locations |

---

### 🛠️ Step-by-Step: Install File Server Role

1. **Open Server Manager**
2. Click **Add Roles and Features**
3. Choose **File and Storage Services**
    
    → ✅ **File Server**
    
4. Click **Install**

✅ Now your server can host shared folders.

---

## 📂 PART 2: Create a Shared Folder

### 🧪 LAB: Create and Share a Department Folder

Let’s create a shared folder for the **HR Department**.

### Step 1: Create Folder on D: Drive

```
D:\Departments\HR

```

### Step 2: Set NTFS Permissions (Folder Level)

1. Right-click `HR` folder → **Properties**
2. Go to **Security** tab → **Edit**
3. Add group: `HRUsers`
4. Set permissions:
    - ✅ Modify
    - ❌ Full Control

> 🔐 NTFS permissions control what users can do inside the folder (locally or over network)
> 

---

### Step 3: Share the Folder (Network Sharing)

1. Go to **Sharing** tab → Click **Advanced Sharing**
2. ✅ Share this folder
3. Share name: `HRShare`
4. Click **Permissions**:
    - Allow: `Everyone` → **Read**
    - OR: Add `HRUsers` → **Full Control** (share-level)

> ⚠️ NTFS and Share permissions combine — the most restrictive applies
> 

### ✅ Effective Permissions Example:

| Share | NTFS | Result |
| --- | --- | --- |
| Full Control | Read | ❌ Only Read |
| Read | Modify | ❌ Only Read |
| Modify | Modify | ✅ Modify |

---

## 🧠 Optional: Test from a Client PC

On a client PC:

```bash
\\FileServer01\HRShare

```

Check access. Try creating, editing, deleting files.

---

## 📊 PART 3: Quota Management (Limit Folder Sizes)

> Useful if you don’t want users filling up the disk.
> 

### Step-by-Step: Enable File Server Resource Manager (FSRM)

1. Go to **Add Roles and Features**
2. Install:
    - ✅ **File Server Resource Manager**
3. Open **FSRM** → Start `fsrm.msc`

### Create a Quota:

1. Go to **Quota Management → Quotas**
2. Right-click → Create Quota
3. Path: `D:\Departments\HR`
4. Template: Use or create a new one (e.g., 2 GB limit)
5. Set notification (optional)

✅ Now, the HR folder can't exceed 2 GB.

---

## 🔄 PART 4: DFS (Distributed File System)

### 📘 What is DFS?

DFS allows you to:

| Feature | Description |
| --- | --- |
| 🌐 Create a single namespace | Users access shares via one path (e.g., `\\corp.local\Departments`) |
| 🔁 Replicate folders | Sync content across servers/sites |
| 🛡️ Increase redundancy | Automatic failover access if one server is down |

---

### Step-by-Step: Install DFS Role

1. Open **Server Manager → Add Roles and Features**
2. Install:
    - ✅ **DFS Namespaces**
    - ✅ **DFS Replication**

---

### Create DFS Namespace (GUI)

1. Open **DFS Management** (`dfsmgmt.msc`)
2. Right-click **Namespaces** → New Namespace
3. Host: `FileServer01`
4. Name: `Departments`
5. Namespace path:
    
    ```
    \\corp.local\Departments
    
    ```
    
6. Add folder targets:
    - `D:\Departments\HR`
    - `D:\Departments\IT`
7. Done

> ✅ Users can now access:
> 
> 
> `\\corp.local\Departments\HR` — even if underlying folder is on different servers.
> 

---

### (Optional) Setup DFS Replication

> To sync folders between two or more servers
> 
1. Open **DFS Management**
2. Right-click **Replication** → New Replication Group
3. Type: **Multipurpose**
4. Add both servers
5. Select folder to replicate: `D:\Departments\HR`
6. Choose Replication Topology: Full Mesh or Hub/Spoke
7. Set schedule and bandwidth limits
8. Finish

✅ DFS will now sync HR folder between servers automatically.

---

## 🖨️ PART 5: PRINT SERVER CONFIGURATION

---

### 📘 What is a Print Server?

| Feature | Description |
| --- | --- |
| 🎯 Centralized Printer Sharing | All users print to network printers |
| 📋 Queue Control | Admin can manage stuck print jobs |
| 🎫 Permissions | Restrict who can print or manage printers |
| 📜 Logging | See who printed what and when |

---

### Step-by-Step: Install Print Server Role

1. Go to **Add Roles and Features**
2. Install:
    - ✅ **Print and Document Services**
    - ✅ **Print Server**
3. Open **Print Management** from Tools menu

---

### Add and Share a Printer

> Requires physical printer or virtual PDF printer for lab.
> 
1. In **Print Management** → Right-click **Printers** → Add Printer
2. Choose **TCP/IP** or **locally connected** printer
3. Name it: `HRPrinter`
4. Share the printer: ✅ Share this printer as `HRPrinter`

---

### Set Printer Permissions

1. Right-click printer → Properties → **Security**
2. Add `HRUsers` → ✅ Print
3. Optional: Allow admins to **Manage Documents**

---

### Connect to Shared Printer from Client

On client PC:

```bash
\\FileServer01\HRPrinter

```

Or use PowerShell:

```powershell
Add-Printer -ConnectionName "\\FileServer01\HRPrinter"

```

---

## ✅ RECAP — What You’ve Learned

| Area | Covered |
| --- | --- |
| 📁 File Server | Installed role, shared folders, NTFS + share permissions |
| 📏 Quotas | Prevent users from filling disk space |
| 🔁 DFS | Unified namespace and replication |
| 🖨️ Print Server | Shared printers, security, and deployment |

---

## **Remote Access & VPN — Deep Dive + Practical Lab**

---

## ✅ OVERVIEW: What You’ll Learn

| Area | Description |
| --- | --- |
| 🔐 RDP / RD Gateway | Secure Remote Desktop access over HTTPS |
| 🌍 VPN (RRAS) | Let clients connect securely to internal network |
| 🎛️ NPS | Enforce authentication + policy (acts as RADIUS server) |

---

## 🖥️ PART 1: RDP & Remote Desktop Gateway

### 📘 What is RD Gateway?

RD Gateway allows users to connect to RDP **over HTTPS (port 443)**, **secured with SSL certificates**.

| Traditional RDP | RDP over Gateway |
| --- | --- |
| Port 3389 | Port 443 (HTTPS) |
| Easy to block/firewall | Secure and encrypted |
| No certificate | Requires SSL cert (public or internal) |

---

### 🛠️ Step-by-Step: Install RD Gateway Role

### 1. Install Role

1. Open **Server Manager → Add Roles and Features**
2. Select:
    - ✅ **Remote Desktop Services**
    - ✅ **Remote Desktop Gateway**
3. Install required features (IIS, NPS included)

---

### 🧪 Configure RD Gateway

1. Open **RD Gateway Manager** from Tools menu
2. In **RD Gateway Manager**, right-click your server → **Properties**

### a. Configure SSL Certificate

- Use an **internal CA** (from AD CS) or **public cert**
- Bind cert to RD Gateway
    - Path: **Properties → SSL Certificate Tab**

### b. Add Resource Authorization Policies (RAP)

- What computers can users connect to?
    - E.g., `Allow Domain Users to connect to all domain-joined PCs`

### c. Add Connection Authorization Policies (CAP)

- Who can use the Gateway?
    - E.g., `Allow Domain Users to log in via RD Gateway`

✅ Now users can RDP over HTTPS via:

```
Remote Desktop → advanced → use Gateway: rdgw.corp.local

```

---

## 🌍 PART 2: VPN Setup (RRAS – Routing and Remote Access Service)

### 📘 What is RRAS?

RRAS enables VPN, NAT, routing between networks.

We’ll use RRAS to set up a **Windows Server as a VPN server**.

---

### 🛠️ Step-by-Step: Install & Configure VPN (RRAS)

### 1. Install RRAS Role

1. Server Manager → Add Roles and Features
2. Select:
    - ✅ **Remote Access**
3. In **Role Services**, select:
    - ✅ **DirectAccess and VPN (RAS)**
4. After install, open **Routing and Remote Access**

---

### 🔧 2. Configure VPN Server

1. Open **Routing and Remote Access (rrasmgmt.msc)**
2. Right-click your server → **Configure and Enable Routing and Remote Access**
3. Choose:
    - ✅ **VPN Access**
4. Select network interface connected to the internet
5. Assign IP address pool for VPN clients:
    - e.g., `192.168.100.10 – 192.168.100.50`
6. Enable RADIUS if using NPS (covered below) or leave default

✅ VPN is now ready. Supports **PPTP**, **L2TP**, and **SSTP** (SSL over port 443).

---

### 🔐 3. Add a VPN User

1. Create user in AD: `vpnuser1`
2. Open **Active Directory Users & Computers**
3. Right-click user → **Properties → Dial-in tab**
4. Network Access Permission: ✅ **Allow access**

---

### 🧪 4. Test VPN Connection from a Client

1. On a Windows client:
    - Open **Network & Internet → VPN**
    - Add a VPN connection:
        - Server: `vpn.corp.local` or external IP
        - VPN type: PPTP (or SSTP if SSL cert is set)
        - Credentials: `vpnuser1` / password
2. Connect — you should be connected to your internal network.

---

## 🎛️ PART 3: NPS – Network Policy Server (RADIUS)

### 📘 What is NPS?

NPS allows you to **centralize VPN authentication and policy enforcement**.

| Feature | Use |
| --- | --- |
| 🎛️ Policies | Enforce who, when, how someone connects |
| 🔐 Authentication | Supports 802.1X, VPN, Wi-Fi, RD Gateway |
| 📜 Logs | Track who connected and when |

---

### 🛠️ Step-by-Step: Configure NPS for VPN

### 1. Install NPS Role (if not already)

- It installs with RD Gateway or can be added via **Add Roles**:
    - ✅ **Network Policy and Access Services**

### 2. Register NPS in Active Directory

Open **NPS Console (nps.msc)**

→ Right-click NPS (local) → **Register in Active Directory**

---

### 🧩 3. Add RRAS Server as RADIUS Client

1. In NPS Console:
    - Expand **RADIUS Clients and Servers → RADIUS Clients**
2. Right-click → **New**
    - Friendly name: `RRAS-VPN`
    - IP: Internal IP of RRAS server
    - Shared secret: Create one (e.g., `SuperSecretKey123`)

✅ RRAS will now forward VPN logins to NPS

---

### 🛡️ 4. Create Network Policy for VPN Access

1. In NPS:
    - Expand **Policies → Network Policies**
2. Right-click → **New**
    - Name: `VPN Policy`
    - Type: Remote Access Server (VPN-Dial-Up)
    - Conditions:
        - User Group: `VPNUsers`
        - Authentication: MS-CHAPv2 or EAP
    - Constraints: Time, encryption level, etc.
    - Access: **Grant Access**

✅ Now NPS decides who gets VPN access.

---

## 🔐 Security Recommendations

| Task | Recommendation |
| --- | --- |
| 📜 Use SSTP VPN | Encrypted over HTTPS (port 443) |
| 🔑 Use NPS + AD Groups | Enforce who can connect |
| 🧾 Enable logging in NPS | Audit who logs in |
| 🎯 Use Firewall Rules | Only allow VPN/IPSec traffic |
| 📛 Rename Administrator | Protect against brute force on VPN |

---

## 🧪 BONUS LAB IDEAS

| Lab | Goal |
| --- | --- |
| 🌐 Setup SSTP VPN | Use cert from AD CS |
| 🔄 Connect VPN + NPS | Enforce access by group |
| 🎯 Deploy RD Gateway + RDWeb | Full RDS environment |
| 📁 Map drive after VPN connects | Use script + GPO |
| 📊 Log VPN logins | Enable NPS logging to event viewer or SQL |

---

## ✅ RECAP — What You’ve Learned

| Component | Description |
| --- | --- |
| 🖥️ RD Gateway | Secure RDP over HTTPS (port 443) |
| 🌐 RRAS VPN | Allow secure client connections |
| 🎛️ NPS | Enforce access policies using RADIUS |

---
