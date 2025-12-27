## 🔹 Part 1: Core PowerShell Concepts

---

### ✅ 1.1 PowerShell Basics

| Command | Description |
| --- | --- |
| `Get-Command` | Lists all available PowerShell cmdlets |
| `Get-Help` | Shows help for cmdlets |
| `Get-Alias` | Shows aliases like `ls` = `Get-ChildItem` |
| `Get-Member` | Shows properties/methods of objects |

---

### 🧪 Practical Lab 1: Exploring Commands

```powershell
# List all commands
Get-Command

# Get help for a command
Get-Help Get-Process -Full

# List services and explore their properties
Get-Service | Get-Member

```

---

## 🔄 Part 2: Scripting Essentials

---

### ✅ 2.1 Variables and Data Types

```powershell
$name = "Admin"
$number = 42
$isEnabled = $true

```

---

### ✅ 2.2 Conditionals (If-Else)

```powershell
if ($number -gt 10) {
    Write-Host "Greater than 10"
} else {
    Write-Host "Less than or equal to 10"
}

```

---

### ✅ 2.3 Loops

```powershell
# For loop
for ($i = 1; $i -le 5; $i++) {
    Write-Output "Loop $i"
}

# Foreach loop
$users = "admin1", "admin2", "admin3"
foreach ($user in $users) {
    Write-Output "User: $user"
}

```

---

## 🌐 Part 3: PowerShell Remoting

---

### ✅ 3.1 Enable PowerShell Remoting

```powershell
Enable-PSRemoting -Force

```

✔ Must be run **as Administrator**

✔ Required on target computers

---

### ✅ 3.2 Use Remoting

| Command | Purpose |
| --- | --- |
| `Enter-PSSession` | Start interactive remote session |
| `Invoke-Command` | Run a command or script remotely |

---

### 🧪 Practical Lab 2: Remote Management

```powershell
# Start interactive session
Enter-PSSession -ComputerName Server01 -Credential (Get-Credential)

# Run command remotely
Invoke-Command -ComputerName Server01 -ScriptBlock { Get-Service }

```

---

## 👨‍💼 Part 4: Active Directory Management Scripts

---

### ✅ 4.1 Import AD Module

```powershell
Import-Module ActiveDirectory

```

> ✅ Must have RSAT tools installed on the system or be on a domain controller.
> 

---

### 🧪 Common AD Tasks (Scripts)

### ➤ Create a New User

```powershell
New-ADUser -Name "John Doe" -SamAccountName jdoe -UserPrincipalName jdoe@yourdomain.com `
-AccountPassword (ConvertTo-SecureString "P@ssw0rd" -AsPlainText -Force) `
-Enabled $true -Path "OU=Users,DC=yourdomain,DC=com"

```

---

### ➤ Bulk Create Users from CSV

**CSV Example: `users.csv`**

```
FirstName,LastName,Username
John,Doe,jdoe
Jane,Smith,jsmith

```

**Script:**

```powershell
Import-Csv users.csv | ForEach-Object {
    $username = $_.Username
    $fullname = "$($_.FirstName) $($_.LastName)"
    $password = ConvertTo-SecureString "P@ssw0rd" -AsPlainText -Force

    New-ADUser -Name $fullname -SamAccountName $username `
    -UserPrincipalName "$username@yourdomain.com" `
    -AccountPassword $password -Enabled $true `
    -Path "OU=Users,DC=yourdomain,DC=com"
}

```

---

### ➤ List Disabled Accounts

```powershell
Get-ADUser -Filter {Enabled -eq $false} -Properties Name | Select Name

```

---

### ➤ Reset Password for User

```powershell
Set-ADAccountPassword -Identity jdoe -NewPassword (ConvertTo-SecureString "NewP@ssw0rd123" -AsPlainText -Force) -Reset

```

---

## 📂 Part 5: System Administration with PowerShell

---

### 🧪 Useful System Cmdlets

| Task | Command |
| --- | --- |
| List services | `Get-Service` |
| Kill a process | `Stop-Process -Name notepad` |
| List installed updates | `Get-HotFix` |
| Check disk space | `Get-PSDrive` or `Get-Volume` (Win 10/2016+) |
| Get system info | `Get-ComputerInfo` |

---

### 🧰 Script: Check Service Status on Multiple Servers

```powershell
$servers = "Server01", "Server02", "Server03"

foreach ($server in $servers) {
    Invoke-Command -ComputerName $server -ScriptBlock {
        Get-Service -Name w32time
    }
}

```

---

## 🎁 Bonus: PowerShell Profile Scripts

You can auto-load custom functions every time PowerShell opens:

```powershell
notepad $PROFILE

```

Add custom aliases, functions, or startup scripts there.

---

## 🧠 PowerShell Learning Strategy

| Stage | Focus | Resources |
| --- | --- | --- |
| 🟢 Beginner | Cmdlets, loops, AD basics | `Get-Help`, TechNet, YouTube |
| 🟡 Intermediate | Scripting, CSV, remoting, scheduled tasks | Docs, GitHub samples |
| 🔴 Advanced | Modules, error handling, advanced remoting | Books, Microsoft Learn, Pluralsight |

---

---

# 🧪 **Windows Task Scheduler – Advanced Guide**

## ✅ Automating Backups, Tasks, Scripts (GUI + Scripting)

---

## 🧠 What Is Task Scheduler?

Task Scheduler is a **built-in Windows service** that enables you to automatically perform routine tasks on a computer at predefined times or in response to specific events.

It can run:

- PowerShell scripts
- CMD/batch files
- EXEs
- Maintenance jobs

---

## 🔍 Key Concepts

| Component | Description |
| --- | --- |
| **Trigger** | What starts the task (e.g., time, logon, event) |
| **Action** | What the task does (run script, program, email, etc.) |
| **Conditions** | Extra rules (e.g., only run on AC power) |
| **Settings** | Control behavior (timeouts, retries, multiple instances) |
| **Security context** | Which user/account the task runs under |

---

## 🛠 1. CREATE A BASIC TASK – GUI Walkthrough

---

### 📁 Use Case: Run a Backup Script Every Day at 2 AM

---

### 🧪 Step-by-Step (GUI):

1. Open **Task Scheduler**
    - `Start → search "Task Scheduler"` → Run as admin
2. Click **“Create Task”** (⚠️ Not “Basic Task” for advanced settings)
3. **General Tab**:
    - Name: `DailyBackupTask`
    - Description: `Runs PowerShell backup script daily at 2 AM`
    - Security Options:
        - ✅ "Run whether user is logged in or not"
        - ✅ "Run with highest privileges"
        - Select user account (e.g., `DOMAIN\backupadmin`)
4. **Triggers Tab**:
    - Click **New**
    - Begin the task: **On a schedule**
    - Set to **Daily**, recur every 1 day
    - Start at: **2:00 AM**
    - Enabled: ✅
5. **Actions Tab**:
    - Click **New**
    - Action: **Start a program**
    - Program/script: `powershell.exe`
    - Add arguments:
        
        ```powershell
        -ExecutionPolicy Bypass -File "C:\Scripts\backup.ps1"
        
        ```
        
    - Start in: `C:\Scripts\` *(optional)*
6. **Conditions Tab**:
    - Uncheck **“Start task only if the computer is on AC power”** (if running on a server)
    - Enable **“Wake the computer to run this task”** if needed
7. **Settings Tab**:
    - Allow task to be run on demand: ✅
    - Stop task if it runs longer than 2 hours (optional)
    - If task fails, restart every 15 mins, up to 3 times ✅
8. Click **OK** → Enter credentials for the selected user
9. ✅ Task is now registered and will run at the scheduled time.

---

## 🔐 2. UNDERSTANDING **Security Contexts**

---

| Option | Description |
| --- | --- |
| "Run only when user is logged in" | Interactive sessions only; no background runs |
| "Run whether user is logged in..." | ✅ Needed for background/system tasks |
| "Run with highest privileges" | Runs task with admin rights (UAC bypass) |
| System account | Use `NT AUTHORITY\SYSTEM` for full system-level access |
| Service account (GMSA, etc.) | Preferred for server/service automation (e.g., backups, monitoring agents) |

> ⚠️ Note: To run under SYSTEM account via GUI:
> 
> 
> Use **schtasks.exe** or **PowerShell** — GUI does not let you browse for built-in accounts.
> 

---

## 💻 3. RUNNING TASKS WITH SYSTEM ACCOUNT (PowerShell)

---

```powershell
$Action = New-ScheduledTaskAction -Execute 'powershell.exe' -Argument '-File "C:\Scripts\backup.ps1"'
$Trigger = New-ScheduledTaskTrigger -Daily -At 2am
Register-ScheduledTask -Action $Action -Trigger $Trigger -TaskName "DailyBackupTask" -Description "Backup Job" -User "SYSTEM" -RunLevel Highest

```

---

## 🧪 4. REAL-WORLD AUTOMATION TASK EXAMPLES

---

### 🛡 4.1 Backup Files to Another Drive

**Script: `C:\Scripts\daily_backup.ps1`**

```powershell
$source = "C:\ImportantData"
$dest = "E:\Backups\$(Get-Date -Format yyyy-MM-dd)"
New-Item -ItemType Directory -Path $dest -Force
Copy-Item -Path $source\* -Destination $dest -Recurse -Force

```

→ Schedule this with a daily trigger.

---

### 📦 4.2 Cleanup Temp Files Weekly

```powershell
$paths = "C:\Windows\Temp", "$env:TEMP"
foreach ($p in $paths) {
    Get-ChildItem $p -Recurse -Force -ErrorAction SilentlyContinue | Remove-Item -Force -Recurse -ErrorAction SilentlyContinue
}

```

→ Trigger: Weekly, Sundays at 3 AM

---

### 📈 4.3 Generate Weekly System Report

```powershell
Get-ComputerInfo > "C:\Reports\SystemReport_$(Get-Date -Format yyyyMMdd).txt"

```

→ Save reports automatically, or even email them using `Send-MailMessage`

---

## 🧰 5. MONITORING & TROUBLESHOOTING TASKS

---

### 📌 Check task logs:

- In Task Scheduler → Click the task → **History tab** (if enabled)
- Enable task history: **Right-click → Enable all task history**

### 🔍 Event Viewer:

- Path:
    
    `Event Viewer → Applications and Services Logs → Microsoft → Windows → TaskScheduler`
    

### 🛠 Common Errors:

| Error | Fix |
| --- | --- |
| **0x1 (Incorrect function)** | Bad script path or missing arguments |
| **0x2 (File not found)** | File doesn’t exist at path |
| **0x8007010B (path does not exist)** | Wrong “Start in” folder path |
| **Runs only when user is logged in** | Set task to run whether logged in or not |
| **Script doesn't run** | Check execution policy: `Set-ExecutionPolicy` |

---

## 🔄 6. EXPORT / IMPORT TASKS

Useful for backing up or copying scheduled jobs.

### Export:

- Task Scheduler → Right-click task → **Export**
- Saves as `.XML`

### Import:

- Task Scheduler → **Import Task** → Select `.XML`
- Update credentials and paths as needed

---

## 🧠 Bonus Tips

- Use **`PowerShell transcripts`** to log output of automated tasks:
    
    ```powershell
    Start-Transcript -Path "C:\Logs\tasklog.txt"
    # your code
    Stop-Transcript
    
    ```
    
- Combine with **Event Triggers**: E.g., run a script when a specific Event ID (like failed login) appears.
- Schedule **PowerShell modules updates**, Defender scans, disk cleanups, or patch compliance reports.

---

## 📚 Resources

| Topic | Link |
| --- | --- |
| Task Scheduler Docs | [Microsoft Docs](https://learn.microsoft.com/en-us/windows/win32/taskschd/task-scheduler-start-page) |
| PowerShell Task Scheduling | [PS Docs](https://learn.microsoft.com/en-us/powershell/module/scheduledtasks/) |

---

---
