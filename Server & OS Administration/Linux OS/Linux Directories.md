### 1. `/` — Root Directory

- The top-level directory; contains all other directories and files.

---

### 2. `/home` — User Home Directories

- Contains personal directories for users (e.g., `/home/alice`).
- Key files:
    - Hidden config files like `.bashrc` (shell settings and command history)
    - User documents and downloads folders

---

### 3. `/bin` — Essential User Binaries

- Contains essential command programs needed by all users.
- Key files:
    - `ls` — lists files and directories
    - `cp` — copies files
    - `mv` — moves or renames files
    - `echo` — outputs text
    - `sh` — the basic shell interpreter

---

### 4. `/etc` — System Configuration Files

- Contains system-wide configuration files controlling how the system behaves.
- Key files:
    - `passwd` — user account info (usernames, home dirs)
    - `shadow` — encrypted user passwords
    - `hostname` — system’s hostname (computer name)
    - `ssh/sshd_config` — SSH server settings
    - `network/interfaces` — network device configs
    - `apt/sources.list` — software package repository lists (Debian-based systems)

---

### 5. `/lib` — Essential Shared Libraries

- Contains shared libraries needed by system binaries and programs.
- Key files:
    - `libc.so.6` — standard C library used by many programs
    - Kernel modules (`/lib/modules/`), which extend kernel capabilities

---

### 6. `/sbin` — System Administration Binaries

- Contains essential programs used for system maintenance by the administrator (root).
- Key files:
    - `reboot` — reboot the system
    - `shutdown` — shut down the system
    - `mount` — mount filesystems
    - `fsck` — file system check and repair utility

---

### 7. `/usr` — User Applications and Data

- Contains installed user applications, libraries, and documentation.
- Key files:
    - `bin/apt-get` — package manager executable (Debian/Ubuntu)
    - `bin/python3` — Python interpreter
    - `lib/libgtk-3.so` — graphical toolkit library
    - `share/doc` — documentation for installed software

---

### 8. `/var` — Variable Data Files

- Holds files that change frequently during system operation.
- Key files:
    - `log/syslog` — general system activity logs
    - `log/auth.log` — authentication and security logs
    - `mail/username` — user mailboxes (if mail server is used)
    - `cache/apt/archives` — cached software packages

---

### 9. `/tmp` — Temporary Files

- Contains temporary files created by programs and users; usually cleared on reboot.
- Key files:
    - Temporary files like `tmpfile.txt` created during program operation
    - Directories for system processes like `.X11-unix` used by the graphical system

---

### 10. `/dev` — Device Files

- Special files that represent hardware devices or virtual devices.
- Key files:
    - `sda` — main hard drive or SSD
    - `tty1` — terminal device for first console
    - `null` — special file that discards data (used to discard unwanted output)
    - `random` — source of random data for cryptographic and other uses

---

### 11. `/mnt` — Mount Points

- Temporary directories where external filesystems like USB drives or network shares are mounted.
- Key files:
    - `usbdrive` — example mount point for a USB drive
    - `nfs_share` — example mount point for a network share

---

### 12. `/proc` — Process and Kernel Information (Virtual Filesystem)

- Provides real-time info about running processes and kernel status as files.
- Key files:
    - `cpuinfo` — details about the CPU (model, speed, cores)
    - `meminfo` — current memory and swap usage
    - `uptime` — how long system has been running
    - Directories like `[pid]/` with info about each running process

---

### 13. `/sys` — System and Device Info (Virtual Filesystem)

- Contains information about devices, drivers, and kernel settings.
- Key files:
    - `class/net/eth0` — info about the network interface
    - `block/sda` — info about disk devices
    - `fs/cgroup` — control groups for resource management

---
