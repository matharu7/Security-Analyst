## 📁 1. Basic File & Directory Commands

### 🔄 Navigation & File Operations

| Command | Description | Example |
| --- | --- | --- |
| `pwd` | Show current working directory | `pwd` |
| `cd` | Change directory | `cd /var/www` |
| `ls` | List files and directories | `ls -lha` |
| `cp` | Copy files or directories | `cp -r dir1 dir2` |
| `mv` | Move or rename files/directories | `mv old.txt new.txt` |
| `rm` | Delete files or directories | `rm -rf dir/` |
| `touch` | Create an empty file | `touch file.txt` |
| `mkdir` | Create a new directory | `mkdir -p dir1/dir2` |
| `cat` | Display file content | `cat file.txt` |
| `less` / `more` | View file content one screen at a time | `less file.txt` / `more file.txt` |
| `head` | Show first 10 lines of a file | `head file.txt` |
| `tail` | Show last 10 lines of a file | `tail file.txt` |
| `file` | Identify file type | `file file.txt` |
| `stat` | Show detailed file info | `stat file.txt` |

---

## ✍️ 2. Text Editors

### 🧰 Common Linux Text Editors

| Editor | Usage Example | Shortcuts / Notes |
| --- | --- | --- |
| `nano` | `nano file.txt` | `Ctrl + O` = Save, `Ctrl + X` = Exit |
| `vim` | `vim file.txt` | `i` = Insert mode, `:wq` = Save and quit |
| `gedit` | `gedit file.txt` (GUI) | Use on systems with desktop environment |

## 📂 3. File Permissions & Ownership

| **Command** | **Purpose** | **Example** |
| --- | --- | --- |
| `chmod` | Change file or directory permissions | `chmod 755 script.sh` → `rwxr-xr-x` |
| `chown` | Change owner (and optionally group) | `chown user:group file.txt` |
| `chgrp` | Change group ownership only | `chgrp admin file.txt` |
| `umask` | Set default permission mask for new files/dirs | `umask 022` → files: `644`, dirs: `755` |

## 🌐 4. Networking Commands

| **Command** | **Purpose** | **Example** |
| --- | --- | --- |
| `ping` | Test connectivity to a host | `ping google.com` |
| `ifconfig` / `ip` | View network configuration | `ip a` (modern alternative to `ifconfig`) |
| `netstat` | Show network stats and open ports | `netstat -tuln` (TCP/UDP listening ports, numeric) |
| `ss` | Show sockets (modern replacement for `netstat`) | `ss -tuln` |
| `wget` | Download files via HTTP/HTTPS/FTP | `wget https://example.com/file.zip` |
| `curl` | Transfer data to/from a server (flexible) | `curl -O https://example.com/file.zip` |
| `traceroute` | Show path packets take to a host | `traceroute google.com` |

## 👤 5. User & Group Management

### 👥 User Management

| **Command** | **Description** | **Example** |
| --- | --- | --- |
| `useradd` | Add a new user | `useradd -m john` (creates home directory) |
| `adduser` | Add user (interactive — Debian/Ubuntu) | `adduser john` |
| `usermod` | Modify a user (e.g., add to group) | `usermod -aG sudo john` (adds user to `sudo`) |
| `userdel` | Delete a user | `userdel -r john` (removes user and home dir) |

### 🔹 Group Management

| **Command** | **Description** | **Example** |
| --- | --- | --- |
| `groupadd` | Add a new group | `groupadd developers` |
| `groupdel` | Delete a group | `groupdel devs` |
| `gpasswd` | Add user to a group | `gpasswd -a john devs` |

## 📦 7. Package Management

### 🔧 Common Package Managers

| **Tool** | **Install** | **Remove** | **Update/Upgrade** |
| --- | --- | --- | --- |
| `apt` *(Debian/Ubuntu)* | `apt install nginx` | `apt remove nginx` | `apt update && apt upgrade` |
| `yum` *(RHEL/CentOS)* | `yum install httpd` | `yum remove httpd` | `yum update` |
| `dnf` *(Fedora)* | `dnf install git` | `dnf erase git` | `dnf upgrade` |
| `pacman` *(Arch)* | `pacman -S firefox` | `pacman -R firefox` | `pacman -Syu` |
| `dpkg` *(Debian .deb)* | `dpkg -i package.deb` | `dpkg -r package-name` | *(Use with `apt` for dependency handling)* |

---

### 🌐 Additional Tools

| **Tool** | **Purpose** | **Example** |
| --- | --- | --- |
| `git clone` | Download code repositories (usually from GitHub) | `git clone https://github.com/user/repo.git` |
| `wine` | Run Windows apps on Linux | `wine app.exe` |
| `playonlinux` | GUI for managing Wine apps | `playonlinux` |

## 💽 8. Disk & Filesystem (Partition, Mount, Unmount)

| **Command** | **Purpose** | **Example** |
| --- | --- | --- |
| `fdisk` | Partition a disk | `sudo fdisk /dev/sdb` |
| `mkfs` | Format a partition | `mkfs.ext4 /dev/sdb1` |
| `mount` | Mount a filesystem | `mount /dev/sdb1 /mnt` |
| `umount` | Unmount a filesystem | `umount /mnt` |
| `lsblk` | List block devices | `lsblk` |

## 📝 9. Text Processing

| **Command** | **Purpose** | **Example** |
| --- | --- | --- |
| `grep` | Search for text patterns | `grep -i "error" /var/log/syslog` |
| `awk` | Advanced field/text processing | `awk '{print $1,$3}' data.txt` |
| `sed` | Stream edit text (search/replace) | `sed 's/foo/bar/g' file.txt` |
| `find` | Find files by name, size, etc. | `find /home -name "*.log" -size +1M` |
| `locate` | Fast file search (uses database) | `locate nginx.conf` |
| `sort` | Sort lines in a file | `sort -u file.txt` |
| `uniq` | Remove or count duplicates | `sort file.txt | uniq -c` |
| `cut` | Cut selected fields from text | `cut -d: -f1 /etc/passwd` |
| `tr` | Translate/transform characters | `echo "hello" | tr '[:lower:]' '[:upper:]'` |
| `wc` | Count lines, words, characters | `wc -l access.log` |

## 📦 10. Archives & Compression

| **Command** | **Purpose** | **Example** |
| --- | --- | --- |
| `tar` | Archive/extract files or directories | `tar -cvf archive.tar dir/` (create) |
|  |  | `tar -xvf archive.tar` (extract) |
| `gzip` | Compress a file | `gzip file.txt` → `file.txt.gz` |
| `gunzip` | Decompress a `.gz` file | `gunzip file.txt.gz` |
| `zip` | Create a `.zip` archive | `zip archive.zip file.txt` |
| `unzip` | Extract a `.zip` archive | `unzip archive.zip` |

## 💻 11. System & Hardware Info

| **Command** | **Purpose** | **Example** |
| --- | --- | --- |
| `lsusb` | List connected USB devices | `lsusb` |
| `lscpu` | Show CPU architecture/info | `lscpu` |
| `lshw -short` | Detailed system hardware overview | `sudo lshw -short` |
| `lspci` | List PCI devices (e.g., GPU, NIC) | `lspci` |
| `lsblk` | Show block devices (disks, partitions) | `lsblk` |
| `df -h` | Show disk usage in human-readable format | `df -h` |
| `du -sh *` | Show size of directories/files (summary) | `du -sh *` |
| `free -h` | Display RAM usage in human-readable format | `free -h` |
| `uptime` | Show how long the system has been running | `uptime` |
| `hostnamectl` | View system hostname and OS details | `hostnamectl` |

## 🔧 12. Service Management (systemd)

### 📦 Install & Configure Common Services

| **Service** | **Install Command** | **Start/Enable** | **Config File** |
| --- | --- | --- | --- |
| **SSH** | `apt install openssh-server` | `systemctl start sshdsystemctl enable sshd` | `/etc/ssh/sshd_config` |
| **Apache2** | `apt install apache2` | `systemctl start apache2systemctl enable apache2` | `/etc/apache2/apache2.conf` |
| **Nginx** | `apt install nginx` | `systemctl start nginxsystemctl enable nginx` | `/etc/nginx/nginx.conf` |
| **Samba** | `apt install samba` | `systemctl start smbdsystemctl enable smbd` | `/etc/samba/smb.conf` |
| **NTFS Support** | `apt install ntfs-3g` | *Mount manually* | `mount -t ntfs-3g /dev/sdb1 /mnt` |
| **MySQL / MariaDB** | `apt install mysql-server` | `systemctl start mysqlsystemctl enable mysql` | `/etc/mysql/my.cnf` |
| **PostgreSQL** | `apt install postgresql` | `systemctl start postgresqlsystemctl enable postgresql` | `/etc/postgresql/` |
| **Docker** | `apt install docker.io` | `systemctl start dockersystemctl enable docker` | `/etc/docker/daemon.json` (if used) |
| **OpenVPN** | `apt install openvpn` | `systemctl start openvpnsystemctl enable openvpn` | `/etc/openvpn/` |

---

## 🔒 13. Security (Firewall & Hardening)

### 🔥 Firewall Tools

| **Tool** | **Install Command** | **Usage Example** | **Purpose** |
| --- | --- | --- | --- |
| `ufw` | `sudo apt install ufw` | `sudo ufw enablesudo ufw allow 22/tcp` | Simple firewall for beginners |
| `iptables` | *(Usually pre-installed)* | `sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPTiptables-save > /etc/iptables/rules.v4` | Advanced packet filtering |
| `nftables` | `sudo apt install nftables` | `sudo nft add rule inet filter input tcp dport 22 accept` | Modern replacement for iptables |

---

### 🛡️ Security Scanners & Hardening Tools

| **Tool** | **Install Command** | **Usage** | **Purpose** |
| --- | --- | --- | --- |
| `chkrootkit` | `sudo apt install chkrootkit` | `sudo chkrootkit` | Scan for known rootkits |
| `rkhunter` | `sudo apt install rkhunter` | `sudo rkhunter --check` | Scan for rootkits and suspicious files |
| `lynis` | `sudo apt install lynis` | `sudo lynis audit system` | Full system audit with hardening suggestions |
| `fail2ban` | `sudo apt install fail2ban` | `sudo fail2ban-client status` | Ban IPs after failed login attempts |
