---

# **Windows Security Basics**

---

## Overview

| Topic | Purpose |
| --- | --- |
| 🛡️ Windows Defender & Security Center | Built-in antivirus + centralized security dashboard |
| 🔥 Windows Firewall (Advanced Settings) | Control inbound/outbound traffic on the server |
| 📜 Security Logs & Auditing | Track security-related events and user activities |
| 🔄 Patch Management (WSUS, Windows Update for Business) | Keep system secure with timely updates |

---

## 🛡️ PART 1: Windows Defender & Security Center

### What is Windows Defender & Security Center?

- **Windows Defender Antivirus**: Real-time protection against malware.
- **Windows Security Center**: Unified dashboard showing system health, virus protection, firewall status, device performance, and more.

---

### Practical Setup & Monitoring

### Step 1: Open Windows Security Center

- Press **Start**, type **Windows Security**, open the app.
- Dashboard sections:
    - Virus & threat protection
    - Account protection
    - Firewall & network protection
    - App & browser control
    - Device security
    - Device performance & health
    - Family options

### Step 2: Enable Real-Time Protection

- In **Virus & threat protection** → Click **Manage settings**
- Make sure **Real-time protection** is ON
- Turn on **Cloud-delivered protection** for faster updates

### Step 3: Run a Quick Scan

- Click **Quick scan** to check for malware immediately.

### Step 4: Configure Defender Updates

- Defender updates are delivered via **Windows Update**.
- You can manually update signatures via:
    
    ```
    Windows Security > Virus & threat protection > Check for updates
    
    ```
    

---

## 🔥 PART 2: Windows Firewall (Advanced Settings)

### What is Windows Firewall?

- Controls inbound/outbound traffic based on rules.
- Protects the server by restricting unwanted connections.

---

### GUI Setup: Configure Windows Firewall

### Step 1: Open Windows Defender Firewall with Advanced Security

- Press **Win + R**, type `wf.msc` and press Enter.

You will see:

- **Inbound Rules**: Traffic allowed/blocked coming in.
- **Outbound Rules**: Traffic allowed/blocked going out.
- **Connection Security Rules**: IPsec rules for secure connections.

### Step 2: Create a New Inbound Rule

- Right-click **Inbound Rules** → **New Rule**
- Select **Port** → Next
- Choose **TCP** or **UDP**, enter port number (e.g., 3389 for RDP)
- Choose **Allow the connection**
- Choose profile (Domain, Private, Public)
- Give it a name, e.g., “Allow RDP”

### Step 3: Disable or Block a Port

- Repeat above, but select **Block the connection**

### Step 4: Review Existing Rules

- Look for default rules for core services (DNS, DHCP, etc.)
- Enable/disable rules as needed.

---

## 📜 PART 3: Security Logs & Auditing

### What is Auditing?

- Logging of important security events such as logins, file access, policy changes.
- Critical for detecting unauthorized activities.

---

### GUI Setup: Enable Audit Policies

### Step 1: Open Group Policy Editor or GPMC

- For local: run `gpedit.msc`
- For domain: open **Group Policy Management Console** (`gpmc.msc`)

### Step 2: Navigate to Audit Policies

```
Computer Configuration → Policies → Windows Settings → Security Settings → Advanced Audit Policy Configuration → Audit Policies

```

### Step 3: Enable Important Audit Categories

Examples:

- **Logon/Logoff** → Audit logon events (Success, Failure)
- **Account Management** → Audit user account changes
- **Object Access** → Audit access to files and folders
- **Policy Change** → Audit changes to security policies

Enable **Success** and **Failure** auditing where applicable.

### Step 4: View Logs in Event Viewer

- Press **Win + R**, type `eventvwr.msc`
- Navigate to:
    
    ```
    Windows Logs → Security
    
    ```
    
- Look for Event IDs such as:
    - 4624 — Successful logon
    - 4625 — Failed logon
    - 4732 — Added member to security-enabled local group
    - 4670 — Permissions on an object were changed

---

## 🔄 PART 4: Patch Management (WSUS, Windows Update for Business)

### What is Patch Management?

- Keeping your OS and apps up to date with security patches is vital to prevent exploits.

---

### Option A: Windows Update (Simple)

- Open **Settings** → **Update & Security** → **Windows Update**
- Click **Check for updates** to manually update.
- Configure **Active hours** to avoid downtime.

---

### Option B: WSUS (Windows Server Update Services)

### What is WSUS?

- A centralized patch management server.
- Allows admins to approve, deploy, and report on updates for all computers.

---

### WSUS Installation & Basic Setup (GUI)

### Step 1: Install WSUS Role

- Open **Server Manager → Add Roles and Features**
- Select **Windows Server Update Services**
- Follow the wizard and select:
    - Role Services: **WSUS Services**, **Database**
    - Specify update storage location

### Step 2: Configure WSUS

- Launch **WSUS Console** from Tools menu.
- Run the post-installation tasks.
- Configure:
    - Products and classifications (e.g., Windows Server 2019 updates)
    - Synchronization schedule
    - Automatic approvals (optional)
    - Target groups (organize clients)

### Step 3: Client Configuration

- Group Policy:
    - Navigate to:
        
        ```
        Computer Configuration → Administrative Templates → Windows Components → Windows Update
        
        ```
        
    - Set **Specify intranet Microsoft update service location** to your WSUS server URL (e.g., `http://wsus.corp.local`)

Clients will now get updates from WSUS.

---

## ✅ Summary of What You Did

| Task | Result |
| --- | --- |
| Windows Defender | Enabled real-time antivirus protection |
| Firewall | Created inbound/outbound rules for traffic control |
| Auditing | Configured detailed security event logging |
| Patch Management | Set up WSUS for centralized update control |

---

# 🔐 **12. User Rights & Permissions — Deep Guide + Practical Setup**

---

## Overview

| Topic | Focus |
| --- | --- |
| 📜 ACLs (Access Control Lists) | Manage who can access files, folders, registry keys, services |
| 🛡️ UAC (User Account Control) | Elevate privileges securely and reduce malware impact |
| 📋 SRP (Software Restriction Policies) | Control which software can run on your server |
| 🔍 File & Folder Permission Auditing | Track access to sensitive resources |

---

## 📜 PART 1: ACLs (Access Control Lists)

### What is an ACL?

- An ACL is a list of permissions attached to an object (file, folder, registry key).
- It defines **which users or groups** can access the object and **what operations** they can perform (read, write, modify, execute).

---

### Practical: View and Modify ACL on a Folder

### Step 1: Open Properties

- Right-click a folder → **Properties**
- Go to **Security** tab

### Step 2: View Permissions

- Click **Advanced**
- See **Permission entries** for users/groups and their rights

### Step 3: Modify or Add Permissions

- Click **Add**
- Select user/group (e.g., `Domain Users`)
- Assign permissions (e.g., Read, Write, Modify)
- Click **OK** to apply

---

## 🛡️ PART 2: UAC (User Account Control)

### What is UAC?

- UAC prompts users for consent or administrator credentials when a task requires elevated privileges.
- Helps prevent unauthorized changes and malware escalation.

---

### Practical: Configure UAC Settings

### Step 1: Open Local Security Policy

- Run `secpol.msc`

### Step 2: Navigate to UAC Policies

- Go to:
    
    ```
    Local Policies → Security Options
    
    ```
    
- Look for policies starting with **User Account Control**

Key settings:

- **Behavior of the elevation prompt for administrators** (set to Prompt for consent or credentials)
- **Run all administrators in Admin Approval Mode** (should be enabled)
- **Detect application installations and prompt for elevation**

### Step 3: Adjust to suit your security posture

- Stricter UAC = better security, but may cause user friction.

---

## 📋 PART 3: SRP (Software Restriction Policies)

### What is SRP?

- SRP controls which software can run on your server, blocking unauthorized or malicious apps.
- Helps prevent malware execution.

---

### Practical: Create SRP in Group Policy

### Step 1: Open Group Policy Management Console (`gpmc.msc`)

- Edit or create a GPO linked to your servers or OUs

### Step 2: Navigate to Software Restriction Policies

- Go to:
    
    ```
    Computer Configuration → Policies → Windows Settings → Security Settings → Software Restriction Policies
    
    ```
    
- If none exist, right-click **Software Restriction Policies** → **New Software Restriction Policies**

### Step 3: Define Rules

- **Designated File Types**: Which file extensions are considered executable (.exe, .bat, etc.)
- **Security Levels**:
    - **Disallowed**: Blocks execution
    - **Unrestricted**: Allows execution
- **Additional Rules** (most important):
    - **Path Rules**: Block or allow software based on file path
    - **Hash Rules**: Block software by cryptographic hash
    - **Certificate Rules**: Allow only signed software
    - **Network Zone Rules**: Control software from Internet zones

### Step 4: Example Rule: Block all .exe in Temp folder

- Add a **Path Rule** for `%TEMP%\*.exe`
- Set security level: **Disallowed**

---

## 🔍 PART 4: File and Folder Permission Auditing

### Why Audit?

- To track who accessed, modified, or deleted sensitive files/folders.
- Important for compliance and security investigations.

---

### Practical: Enable Auditing on a Folder

### Step 1: Enable Audit Policy

- Use Group Policy:
    
    ```
    Computer Configuration → Policies → Windows Settings → Security Settings → Advanced Audit Policy Configuration → Object Access → Audit File System
    
    ```
    
- Enable **Success** and/or **Failure** auditing

### Step 2: Configure Auditing on Folder

- Right-click folder → Properties → Security → Advanced → Auditing tab
- Click **Add**
- Select principal (e.g., Everyone or specific users/groups)
- Select types of access to audit (Read, Write, Delete)
- Apply and OK

### Step 3: View Events

- Open **Event Viewer** (`eventvwr.msc`)
- Navigate to:
    
    ```
    Windows Logs → Security
    
    ```
    
- Look for Event ID 4663 — Object Access

---

## ✅ Recap

| Area | Key Takeaway |
| --- | --- |
| ACLs | Manage precise permissions on objects |
| UAC | Protect privilege escalation with prompts |
| SRP | Control which software can run to block malware |
| Auditing | Monitor access to sensitive files/folders |

---

# **Authentication & Identity**

---

## 🔑 TOPICS COVERED

1. Kerberos Authentication — architecture, troubleshooting, and advanced config
2. NTLM — legacy issues, when it’s used, and security implications
3. Credential Guard — deployment, configuration, and troubleshooting
4. Smart Card & Biometric Authentication — PKI integration, smart card login setup, Windows Hello for Business
5. Azure AD Join vs On-premises AD — hybrid identity, device registration, conditional access
6. Additional topics: Authentication policies, ticket lifetimes, delegation, claims-based auth

---

## 1️⃣ **Kerberos Authentication — Deep Architecture & Practical**

---

### What is Kerberos?

- **Kerberos** is a secure, ticket-based authentication protocol designed for Active Directory environments.
- Uses **symmetric cryptography** and a trusted third party called the **Key Distribution Center (KDC)**, which runs on the domain controllers.
- Prevents replay attacks, eavesdropping, and password sniffing.

---

### Key Components:

| Component | Role |
| --- | --- |
| **Client** | Requests tickets to access services |
| **KDC (Domain Controller)** | Issues tickets: TGT & Service Tickets |
| **Ticket Granting Ticket (TGT)** | Proof that user authenticated |
| **Service Ticket** | Allows access to services |
| **Service Principal Name (SPN)** | Unique ID of services in AD |

---

### Kerberos Workflow (Advanced)

1. User logs in and requests TGT from KDC.
2. KDC verifies credentials against AD.
3. KDC issues encrypted TGT (valid for default 10 hours).
4. User requests Service Ticket using TGT.
5. Service validates ticket and grants access.

---

### Practical: Viewing Kerberos Tickets

- On client/server:
1. Open **Command Prompt** as user
2. Run:
    
    ```
    klist
    
    ```
    
    Shows current Kerberos tickets.
    
3. Run:
    
    ```
    klist tickets
    
    ```
    
    To view detailed ticket info: Service name, expiration, etc.
    

---

### Advanced: Resetting Kerberos Tickets

- If authentication issues occur, clear tickets with:
    
    ```
    klist purge
    
    ```
    
- Then reauthenticate by logging off/on.

---

### Configuring Ticket Lifetimes

- Ticket lifetimes are controlled via Group Policy:
    
    ```
    Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Kerberos Policy
    
    ```
    
- Key settings:
    - **Maximum lifetime for user ticket** (default 10 hrs)
    - **Maximum lifetime for service ticket**
    - **Maximum tolerance for computer clock synchronization** (default 5 mins, crucial for Kerberos)

---

### Troubleshooting Kerberos

- Use Event Viewer on DC/client:
    - **Event ID 4768** — TGT request
    - **Event ID 4771** — Pre-authentication failure (bad password or time skew)
    - **Event ID 4769** — Service ticket request
    - **Event ID 4776** — NTLM logon
- Use **Kerbtray.exe** or **Microsoft Kerberos Configuration Manager** tool to diagnose issues.

---

## 2️⃣ **NTLM Authentication — Legacy Protocol Deep Dive**

---

### When is NTLM used?

- When Kerberos isn’t possible (e.g., no domain trust, older systems).
- Used for fallback authentication or local logons.

---

### Security Issues with NTLM

- Vulnerable to **Pass-the-Hash** attacks.
- Does not support mutual authentication.
- Lack of ticketing means more network traffic.

---

### How to Check for NTLM Usage

- Enable NTLM auditing via GPO:
    
    ```
    Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → Audit Policy → Audit NTLM authentication in domain
    
    ```
    
- Check Event Viewer (Event ID 4624 with NTLM as auth package).

---

### Reducing NTLM Usage

- Use Group Policy to restrict NTLM usage:
    
    ```
    Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → Security Options
    
    ```
    
- Policy: **Network Security: Restrict NTLM: NTLM authentication in this domain** — set to **Deny all accounts** (test carefully).

---

## 3️⃣ **Credential Guard — Advanced Deployment & GUI Configuration**

---

### What is Credential Guard?

- Uses **Virtual Secure Mode (VSM)** and hardware virtualization to protect secrets like NTLM hashes and Kerberos tickets.

---

### Deployment Requirements:

| Requirement | Notes |
| --- | --- |
| Windows 10/Server 2016+ | Enterprise or Datacenter editions |
| UEFI Firmware with Secure Boot | Must be enabled in BIOS |
| CPU Virtualization Support | Intel VT-x or AMD-V enabled |
| Hyper-V role | Needs to be enabled |

---

### GUI Setup: Enable Credential Guard via Group Policy

1. Open **Group Policy Management Console** (`gpmc.msc`).
2. Edit domain or local GPO applied to target machine.
3. Navigate to:
    
    ```
    Computer Configuration → Administrative Templates → System → Device Guard
    
    ```
    
4. Double-click **Turn On Virtualization Based Security**.
5. Select **Enabled**.
6. In Options, check:
    - **Enabled with UEFI lock**
    - **Credential Guard configuration**: Select **Enabled with UEFI lock** or **Enabled without lock** (for testing).
7. Apply and reboot the system.

---

### Verify Credential Guard

- Run `msinfo32.exe`
- Under **System Summary**, check **Device Guard Properties** and **Credential Guard Running** = Yes

---

### Troubleshooting Tips

- Use **DG_Readiness_Tool** from Microsoft to validate environment readiness.
- Check Event Viewer logs under:
    
    ```
    Applications and Services Logs → Microsoft → Windows → DeviceGuard → Operational
    
    ```
    

---

## 4️⃣ **Smart Card & Biometric Authentication**

---

### Smart Card Authentication Overview

- Uses **PKI certificates** stored on physical cards.
- Strong two-factor authentication.
- Requires **Certificate Authority (CA)** infrastructure.

---

### Practical: Set Up Smart Card Login

1. **Deploy Active Directory Certificate Services (AD CS)**:
    - Add **Certification Authority** role via Server Manager.
    - Configure Enterprise CA.
2. **Enroll Smart Card Certificates**:
    - Issue **Smart Card Logon certificates** to user smart cards.
3. **Configure Group Policy for Smart Card Login**:
    - Navigate to:
        
        ```
        Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → Security Options
        
        ```
        
    - Enable **Interactive logon: Require smart card**.
    - Configure **Smart card removal behavior** (lock workstation).
4. **Enroll users’ smart cards** and test login.

---

### Windows Hello for Business (Biometric)

- Replaces passwords with biometrics or PIN.
- Requires on-prem AD or Azure AD + MDM (Intune).

---

### Practical Setup for Windows Hello for Business

1. Enable via Group Policy:
    
    ```
    Computer Configuration → Administrative Templates → Windows Components → Windows Hello for Business
    
    ```
    
2. Configure:
    - Use biometrics (fingerprint, facial recognition).
    - Enable PIN sign-in.
    - Configure certificate or key trust model.
3. Enroll devices/users.

---

## 5️⃣ **Azure AD Join vs On-premises AD — Hybrid Identity Explained**

---

| Feature | Azure AD Join | On-premises AD |
| --- | --- | --- |
| Authentication | OAuth2, OpenID Connect, SAML | Kerberos, NTLM |
| Device Management | Managed via Intune/MDM | Managed via GPO |
| Access | Cloud-first apps, Conditional Access | Internal network resources |
| Hybrid Join | Devices joined to both Azure AD & on-prem AD | Traditional domain joined |

---

### Practical: Azure AD Join a Windows 10/11 Device

1. Open **Settings → Accounts → Access work or school → Connect**.
2. Choose **Join this device to Azure Active Directory**.
3. Enter Azure AD credentials.
4. Device will be registered in Azure AD.
5. Manage via **Azure Portal** and **Intune**.

---

### Hybrid Azure AD Join Setup

- Requires **Azure AD Connect** with device writeback enabled.
- Devices joined to on-prem domain automatically registered in Azure AD.
- Enables seamless Single Sign-On (SSO) for cloud and on-prem apps.

---

## 6️⃣ **Advanced Concepts**

---

### Constrained Delegation

- Allows a service to impersonate users to access resources.
- Configured in AD Users and Computers on service account → Delegation tab.
- Use **Kerberos constrained delegation** for security.

---

### Claims-based Authentication & AD FS

- AD FS provides federated identity using claims (SAML tokens).
- Useful for SSO with external SaaS and web applications.

---

# ✅ **Summary Table**

| Topic | What You Did/Understood |
| --- | --- |
| Kerberos | Ticket-based, secure authentication; troubleshooting tickets & logs |
| NTLM | Legacy fallback; risks and reducing its use |
| Credential Guard | Enabled virtualization-based security to protect credentials |
| Smart Card / Biometrics | PKI-based login + Windows Hello for strong auth |
| Azure AD Join | Cloud device registration; hybrid identity models |
| Delegation & Claims | Advanced delegation & federated auth for complex setups |

# **Windows Hardening Techniques**

---

## Overview of Topics

- Disabling unnecessary services to reduce attack surface
- Applying Secure Baseline Templates (CIS, STIG) for standardized hardening
- Attack Surface Reduction (ASR) rules via Microsoft Defender Exploit Guard
- Deploying LAPS (Local Administrator Password Solution) to manage local admin passwords securely

---

## 1️⃣ Disabling Unnecessary Services

---

### Why Disable Services?

- Every running service is a potential attack vector.
- Minimizing running services reduces CPU, memory usage, and security risks.
- Unnecessary services may be exploited by attackers or malware.

---

### Identifying Services to Disable

- Use **baseline Microsoft documentation** or CIS benchmarks to identify non-essential services for your server role.
- Common services to review for disabling:

| Service Name | Description | Notes |
| --- | --- | --- |
| Telnet | Remote command-line interface | Use SSH instead |
| Remote Registry | Remote registry editing | Disable unless required |
| Print Spooler | Manages printing | Disable if no printing |
| Windows Remote Management (WinRM) | Remote management | Disable if unused |
| SSDP Discovery | UPnP device discovery | Often unnecessary on servers |

---

### How to Audit Running Services (GUI + PowerShell)

- **GUI:**
    1. Run `services.msc`.
    2. Sort by **Startup Type** and **Status**.
    3. Research each running service’s role.
- **PowerShell:**
    
    ```powershell
    Get-Service | Where-Object {$_.Status -eq "Running"} | Select-Object Name, DisplayName, StartType
    
    ```
    

---

### Disabling Services

- **GUI:**
    1. In **Services** console, right-click service → Properties.
    2. Set **Startup type** to **Disabled** or **Manual**.
    3. Click **Stop** if running.
    4. Click **OK** to save.
- **PowerShell:**
    
    ```powershell
    # Disable and stop service
    Set-Service -Name "ServiceName" -StartupType Disabled
    Stop-Service -Name "ServiceName" -Force
    
    ```
    

---

### Best Practices

- Document changes and reasons.
- Test thoroughly before production.
- Use Group Policy Preferences to enforce service states across many servers.
- Review services regularly as server roles evolve.

---

## 2️⃣ Secure Baseline Templates (CIS, STIG)

---

### What are Secure Baselines?

- Baselines are **pre-configured security settings** designed by security experts to harden Windows systems.
- **CIS (Center for Internet Security)** baselines are widely used, community-driven.
- **STIG (Security Technical Implementation Guide)** by DISA is stricter, used by government and defense sectors.

---

### Why Use Baselines?

- Standardize security across servers.
- Reduce configuration drift.
- Simplify audits and compliance.
- Quickly apply industry best practices.

---

### How to Obtain Baselines

- CIS Benchmarks:
    
    https://www.cisecurity.org/cis-benchmarks/ (free registration required)
    
- DISA STIGs:
    
    https://public.cyber.mil/stigs/downloads/
    
- Microsoft Security Compliance Toolkit:
    
    https://learn.microsoft.com/en-us/windows/security/threat-protection/windows-security-configuration-framework/security-compliance-toolkit-10
    

---

### Import & Apply Baseline Templates

- **Import baseline GPO into Group Policy Management Console (GPMC):**
    1. Open `gpmc.msc`.
    2. Right-click **Group Policy Objects** → **Import Settings**.
    3. Select downloaded baseline `.xml` or `.pol` file.
    4. Link the GPO to target OU/domain.
- **Customize:**
    - Adjust policies as per your environment’s requirements.
    - Review password policies, audit settings, firewall rules, user rights assignments.
- **Testing:**
    - Always test baselines in lab environments.
    - Use tools like **Microsoft Security Compliance Toolkit (SCT)** to analyze compliance.

---

### Monitoring Baseline Compliance

- Use **Microsoft Security Compliance Manager (SCM)** or third-party tools like **Lepide** or **SolarWinds**.
- Regularly review **Event Logs**, especially Security logs for audit failures.

---

## 3️⃣ Attack Surface Reduction (ASR)

---

### What is ASR?

- ASR is a part of **Microsoft Defender Exploit Guard** designed to reduce your Windows systems’ attack surface.
- It blocks behaviors commonly exploited by malware (e.g., suspicious scripts, Office macro attacks, lateral movement techniques).

---

### Key ASR Rules Examples

| Rule Name | Description |
| --- | --- |
| Block executable content from email client | Blocks malicious attachments from running |
| Block Office apps from creating child processes | Prevents macro malware execution |
| Block credential stealing from LSASS | Protects against credential dumping |
| Block executable content from removable drives | Protects against USB malware |

---

### Enabling ASR Rules

- Via **Group Policy:**
    1. Open **Group Policy Management Editor**.
    2. Navigate to:
        
        ```
        Computer Configuration → Administrative Templates → Windows Components → Microsoft Defender Antivirus → Microsoft Defender Exploit Guard → Attack Surface Reduction
        
        ```
        
    3. Enable rules as per your security policy.
    4. Set each rule to **Audit** initially to monitor impacts, then switch to **Block**.
- Via **PowerShell:**
    
    ```powershell
    # Example: Enable "Block executable content from email client"
    Set-MpPreference -AttackSurfaceReductionRules_Ids D4F940AB-401B-4EFC-AADC-AD5F3C50688A -AttackSurfaceReductionRules_Actions Enabled
    
    ```
    
- ASR Rule IDs can be found in Microsoft Docs.

---

### Monitoring ASR Events

- ASR events are logged here:
    
    ```
    Event Viewer → Applications and Services Logs → Microsoft → Windows → Windows Defender → Operational
    
    ```
    
- Event IDs 1121-1135 show ASR rule triggers.

---

### Best Practices for ASR

- Start with Audit mode to understand rule impact.
- Gradually move rules to Block.
- Combine with other protections like Exploit Protection and Controlled Folder Access.
- Monitor logs continuously.

---

## 4️⃣ LAPS (Local Administrator Password Solution)

---

### Why LAPS?

- Avoids weak, static local admin passwords.
- Automatically randomizes and stores passwords securely in AD.
- Helps prevent lateral movement attacks.

---

### Components

- AD Schema extension (adds ms-Mcs-AdmPwd attribute).
- Client-side LAPS agent installed on managed machines.
- Group Policy template for configuring LAPS behavior.
- Password retrieval tools (GUI or PowerShell).

---

### Deployment Steps

---

### Step 1: Extend AD Schema

- On a domain controller:

```powershell
Import-Module AdmPwd.PS
Update-AdmPwdADSchema

```

- This adds the necessary attributes for storing passwords.

---

### Step 2: Delegate Permissions

- Allow computers to update their password attribute:

```powershell
Set-AdmPwdComputerSelfPermission -OrgUnit "OU=Servers,DC=domain,DC=com"

```

- Allow administrators or helpdesk to read passwords:

```powershell
Set-AdmPwdReadPasswordPermission -OrgUnit "OU=Servers,DC=domain,DC=com" -AllowedPrincipals "HelpDeskGroup"

```

---

### Step 3: Install LAPS Client

- Install the LAPS MSI on each managed computer.
- Use SCCM, Intune, or manual installation.

---

### Step 4: Configure Group Policy

- Open Group Policy Management Editor.
- Navigate to:
    
    ```
    Computer Configuration → Policies → Administrative Templates → LAPS
    
    ```
    
- Enable:
    - **Enable local admin password management**
    - **Password Settings** (length, complexity, expiration)

---

### Step 5: Retrieve Passwords

- Using **LAPS GUI** (`LAPS.UI.exe`).
- Using PowerShell:

```powershell
Get-AdmPwdPassword -ComputerName "Computer1"

```

---

### Best Practices

- Restrict password read permissions.
- Audit and log password retrieval.
- Periodically check for client compliance (LAPS installed & reporting).
- Integrate with SIEM for alerting.

---

## Additional Context & Resources

| Topic | Useful Links & Tools |
| --- | --- |
| Disabling Services | Microsoft Docs for service roles: https://docs.microsoft.com/en-us/windows-server/roles/ |
| Secure Baselines | CIS Benchmarks: https://www.cisecurity.org/cis-benchmarks/  STIG: https://public.cyber.mil/stigs/ |
| ASR Rules | MS Docs: https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/attack-surface-reduction-rules |
| LAPS | Official Download & Docs: https://www.microsoft.com/en-us/download/details.aspx?id=46899  PowerShell Module: AdmPwd.PS |

---

# Summary Table

| Hardening Technique | Description | Implementation | Benefits |
| --- | --- | --- | --- |
| Disable Services | Turn off non-essential Windows services | services.msc or PowerShell | Reduced attack surface and resource usage |
| Secure Baseline Templates | Use industry-standard GPOs | Import CIS/STIG GPOs via GPMC | Consistent security hardening, compliance |
| Attack Surface Reduction (ASR) | Block common attack vectors | GPO or PowerShell | Prevents malware execution, credential theft |
| LAPS | Manage local admin passwords securely | Extend AD schema, deploy client, GPO | Eliminates shared passwords, reduces lateral movement risk |

###
