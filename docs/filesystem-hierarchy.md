# Linux Filesystem Hierarchy Standard (FHS)

> The Linux filesystem is a unified tree structure starting from the root directory `/`. This document explains the Filesystem Hierarchy Standard (FHS) and describes every important directory in a Linux system.

---

## 🌳 The Linux Filesystem Tree

In Linux, **everything starts from a single root directory** represented by `/`. Unlike Windows, which uses drive letters (C:, D:, etc.), Linux has a **single unified tree** where all storage devices are mounted at various points within this tree.

```
                    /
          (Root Directory)
         ╱      │      ╲      ╲
       bin     etc     home    var     usr     dev     proc    tmp
       │       │       │       │       │       │       │       │
   Essential  System  User   Logs &   User   Device  Process  Temp
  Binaries  Config  Homes  Variable Installed Files  Info    Files
                          Data    Software
```

> ⚠️ Important  
> The root directory `/` is the top of the Linux filesystem hierarchy. It is **not** the same as the home directory of the root user, which is `/root`.

---

## 📂 Directory-by-Directory Guide

### `/` — Root Directory

The top-level directory of the entire filesystem. Everything else is contained within it.

```bash
# List contents of root directory
ls /
```

> ⚠️ Important  
> Do not confuse `/` (root of filesystem) with `~` or `/root` (root user's home directory) or `/home/username` (regular user's home directory).

---

### `/home` — User Home Directories

Contains personal directories for each user on the system.

```
/home/
├── alice/          ← Alice's personal files
│   ├── Documents/
│   ├── Downloads/
│   ├── .bashrc     ← Alice's shell configuration
│   └── .config/
├── bob/            ← Bob's personal files
│   ├── Documents/
│   └── .bashrc
└── charlie/        ← Charlie's personal files
    └── ...
```

**Common subdirectories in a user's home:**

| Directory | Purpose |
|-----------|---------|
| `~/Documents` | Personal documents |
| `~/Downloads` | Downloaded files |
| `~/Desktop` | Files on the desktop (GUI) |
| `~/Pictures` | Images and photos |
| `~/Music` | Audio files |
| `~/Videos` | Video files |
| `~/.bashrc` | Bash shell configuration |
| `~/.ssh/` | SSH keys and configuration |
| `~/.config/` | Application configurations |

```bash
# Go to your home directory
cd ~
# or simply
cd

# List your home directory
ls -la ~

# Check current user's home
echo $HOME
```

---

### `/bin` — Essential User Binaries

Contains **essential command binaries** (executables) that are needed for basic system operation, available to all users.

**Common commands found in `/bin`:**

| Command | Purpose |
|---------|---------|
| `ls` | List directory contents |
| `cp` | Copy files |
| `mv` | Move/rename files |
| `rm` | Remove files |
| `cat` | Display file contents |
| `echo` | Print text to terminal |
| `bash` | Bourne Again SHell |
| `chmod` | Change file permissions |
| `date` | Display/set date and time |
| `grep` | Search text patterns |

```bash
# List contents of /bin
ls /bin

# Check where a command is located
which ls
# Output: /bin/ls (or /usr/bin/ls on some systems)
```

> 💡 Tip  
> On modern systems (with merged `/usr`), `/bin` is often a symbolic link to `/usr/bin`. This is part of the **UsrMerge** initiative.

---

### `/sbin` — System Administration Binaries

Contains **essential system administration binaries**, typically used by the root user.

**Common commands found in `/sbin`:**

| Command | Purpose |
|---------|---------|
| `fdisk` | Partition table manipulator |
| `ifconfig` | Network interface configuration |
| `reboot` | Reboot the system |
| `shutdown` | Shut down the system |
| `mkfs` | Build a Linux filesystem |
| `iptables` | Firewall administration |
| `init` / `systemctl` | System/service management |

```bash
# List contents of /sbin
ls /sbin

# These typically require root privileges
sudo fdisk -l
sudo shutdown -h now
```

> 💡 Tip  
> Like `/bin`, on modern systems `/sbin` is often a symlink to `/usr/sbin`.

---

### `/etc` — System Configuration Files

The central location for **system-wide configuration files**. Almost every service and application stores its config here.

```
/etc/
├── passwd              ← User account information
├── shadow              ← Encrypted user passwords
├── group               ← Group definitions
├── hostname            ← System hostname
├── fstab               ← Filesystem table (auto-mount config)
├── resolv.conf         ← DNS configuration
├── bash.bashrc        ← System-wide bash config
├── ssh/               ← SSH server configuration
│   ├── sshd_config
│   └── ssh_config
├── nginx/             ← Nginx web server config
│   ├── nginx.conf
│   └── sites-available/
├── apt/               ← APT package manager config (Debian/Ubuntu)
│   ├── sources.list
│   └── apt.conf.d/
├── systemd/           ← Systemd system manager config
│   └── system/
└── cron.d/            ← Scheduled task definitions
```

**Key configuration files:**

| File | Purpose |
|------|---------|
| `/etc/passwd` | User account database |
| `/etc/shadow` | Encrypted password database |
| `/etc/group` | Group database |
| `/etc/fstab` | Filesystem mount table |
| `/etc/hostname` | System hostname |
| `/etc/resolv.conf` | DNS resolver configuration |
| `/etc/hosts` | Static hostname-to-IP mapping |
| `/etc/sudoers` | Sudo privileges configuration |
| `/etc/profile` | System-wide environment settings |

```bash
# View your system's hostname
cat /etc/hostname

# View user accounts
cat /etc/passwd

# View filesystem mount config
cat /etc/fstab

# View DNS settings
cat /etc/resolv.conf
```

> ⚠️ Important  
> Many files in `/etc` require root privileges to modify. Always backup config files before editing:
> ```bash
> sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup
> ```

---

### `/var` — Variable Data

Contains **files that change frequently** during system operation — logs, caches, spools, and runtime data.

```
/var/
├── log/               ← System and application logs
│   ├── syslog         ← System log
│   ├── auth.log       ← Authentication log
│   ├── kern.log       ← Kernel log
│   ├── dpkg.log       ← Package manager log
│   └── nginx/         ← Nginx web server logs
│       ├── access.log
│       └── error.log
├── cache/             ← Application cache
│   └── apt/           ← APT package cache
├── lib/               ← State information (databases, package info)
│   └── dpkg/          ← Installed package database
├── spool/             ← Print queues, mail queues
│   └── cron/          ← Cron job spools
├── tmp/               ← Temporary files (sometimes separate from /tmp)
├── run/               ← Runtime data (PIDs, sockets) — symlinked to /run
└── www/               ← Web server content (Debian/Ubuntu)
    └── html/          ← Default web pages
```

**Common log files:**

| Log File | What It Records |
|----------|-----------------|
| `/var/log/syslog` | General system messages |
| `/var/log/auth.log` | Authentication attempts, sudo usage |
| `/var/log/kern.log` | Kernel messages |
| `/var/log/dpkg.log` | Package installation/removal |
| `/var/log/boot.log` | Boot process messages |
| `/var/log/journal/` | Systemd journal logs |

```bash
# View recent system logs
tail /var/log/syslog

# View authentication log
tail /var/log/auth.log

# View systemd journal
journalctl

# View journal for current boot
journalctl -b
```

---

### `/usr` — User Software and Libraries

The largest directory, containing **user-installed applications, shared libraries, documentation, and source code**. Think of it as "Unix System Resources."

```
/usr/
├── bin/               ← Non-essential user binaries (most commands)
│   ├── python3
│   ├── gcc
│   ├── git
│   ├── vim
│   └── docker
├── sbin/              ← Non-essential system admin binaries
├── lib/               ← Shared libraries for /usr/bin and /usr/sbin
│   └── python3/
├── lib64/             ← 64-bit libraries
├── share/             ← Architecture-independent data
│   ├── man/           ← Manual pages
│   ├── doc/           ← Documentation
│   ├── icons/         ← GUI icons
│   └── applications/  ← Desktop application entries
├── include/           ← C/C++ header files (for development)
│   └── stdio.h
├── local/             ← Software installed locally (not by package manager)
│   ├── bin/
│   ├── lib/
│   └── share/
├── src/               ← Kernel source code (sometimes)
└── games/             ← Games
```

**Key difference: `/bin` vs `/usr/bin`**

| Directory | Contents |
|-----------|----------|
| `/bin` | Essential binaries needed for basic system boot and repair |
| `/usr/bin` | Non-essential binaries — most user applications and tools |

> 💡 Tip  
> On many modern distributions, `/bin` → `/usr/bin` (symlink). This is called the **UsrMerge**. The distinction is becoming less relevant.

```bash
# Check if /bin is a symlink
ls -ld /bin

# See how much space /usr uses
du -sh /usr
```

---

### `/tmp` — Temporary Files

A directory for **temporary files** created by applications and users. Files here may be deleted on reboot.

```bash
# Create a temporary file
touch /tmp/mytempfile.txt

# List temporary files
ls -l /tmp/
```

**Properties of `/tmp`:**

| Property | Description |
|----------|-------------|
| **World-writable** | Any user can create files here |
| **Sticky bit set** | Users can only delete their own files |
| **May be cleared on reboot** | Many systems clean `/tmp` at startup |
| **Often mounted as tmpfs** | Stored in RAM for speed |

```bash
# Check permissions of /tmp
ls -ld /tmp
# Output: drwxrwxrwt ... /tmp
#                         ↑
#                    't' = sticky bit

# Sticky bit means: you can only delete your own files
```

---

### `/dev` — Device Files

Contains **special device files** that represent hardware devices. In Linux, **everything is a file**, and hardware devices are no exception.

```
/dev/
├── sda                ← First SATA/SCSI hard drive
├── sda1               ← First partition on first drive
├── sda2               ← Second partition
├── nvme0n1            ← NVMe SSD
├── nvme0n1p1          ← First partition on NVMe
├── tty0               ← Virtual terminal
├── tty1               ← First virtual console
├── null               ← Null device (discards all input)
├── zero               ← Infinite stream of zeros
├── random             ← Random number generator
├── urandom            ← Non-blocking random number generator
├── sda                ← Block device (hard drive)
├── loop0              ← Loop device (for mounting image files)
├── cpu                ← CPU information
└── input/             ← Input devices (keyboard, mouse)
    ├── event0
    └── mice
```

**Device Types:**

| Type | Prefix | Description | Example |
|------|--------|-------------|---------|
| **Block devices** | `sd`, `nvme`, `loop` | Storage devices, accessed in blocks | `/dev/sda`, `/dev/nvme0n1` |
| **Character devices** | `tty`, `null`, `random` | Stream-based devices | `/dev/tty1`, `/dev/null` |

```bash
# List device files
ls -l /dev/

# View block devices
lsblk

# Write zeros to a file from /dev/zero
dd if=/dev/zero of=testfile bs=1M count=10

# Discard output using /dev/null
echo "test" > /dev/null
```

> 💡 Tip  
> `/dev/null` is known as the "bit bucket" or "black hole." Anything written to it disappears forever. It's commonly used to suppress output:
> ```bash
> command > /dev/null 2>&1
> ```

---

### `/proc` — Process Information Interface

A **virtual filesystem** (procfs) that provides an interface to kernel data structures. It presents information about processes and system information as files.

```
/proc/
├── 1/                 ← Process ID 1 (usually init/systemd)
│   ├── cmdline        ← Command line used to start
│   ├── environ        ← Environment variables
│   ├── fd/            ← File descriptors
│   ├── status         ← Process status
│   └── maps           ← Memory maps
├── 1234/              ← Process ID 1234
├── 5678/              ← Process ID 5678
├── cpuinfo            ← CPU information
├── meminfo            ← Memory information
├── version            ← Kernel version
├── mounts             ← Mounted filesystems
├── partitions         ← Disk partitions
├── uptime             ← System uptime
├── loadavg            ← Load average
└── net/               ← Network information
    ├── tcp
    └── dev
```

```bash
# View CPU information
cat /proc/cpuinfo

# View memory information
cat /proc/meminfo

# View kernel version
cat /proc/version

# View system uptime
cat /proc/uptime

# View load average
cat /proc/loadavg

# View info about process ID 1
cat /proc/1/status
```

> 💡 Tip  
> Files in `/proc` are not real files on disk — they are **virtual** representations of kernel data. They exist only in memory and are generated on-the-fly when you read them.

---

### `/sys` — Kernel and Hardware Interface

A **virtual filesystem** (sysfs) that exposes information about kernel subsystems, device drivers, and hardware configuration.

```
/sys/
├── block/             ← Block devices
│   └── sda/
├── bus/               ← Hardware buses
│   ├── pci/
│   └── usb/
├── class/             ← Device classes
│   ├── net/           ← Network interfaces
│   │   └── eth0/
│   └── input/
├── devices/           ← All devices in the system
│   ├── pci0000:00/
│   └── platform/
├── fs/                ← Filesystem information
├── kernel/            ← Kernel parameters
│   └── mm/            ← Memory management
├── module/            ← Loaded kernel modules
│   └── ext4/
└── power/             ← Power management
```

```bash
# View network interface information
ls /sys/class/net/

# View CPU temperature (if available)
cat /sys/class/thermal/thermal_zone0/temp

# List loaded kernel modules
ls /sys/module/
```

> 💡 Tip  
> Like `/proc`, `/sys` is a virtual filesystem. Some files in `/sys` can be **written to** in order to change kernel behavior at runtime.

---

### Other Important Directories

| Directory | Purpose |
|-----------|---------|
| `/boot` | Bootloader files (kernel image, initrd, GRUB config) |
| `/lib` | Essential shared libraries for binaries in `/bin` and `/sbin` |
| `/lib64` | 64-bit essential shared libraries |
| `/opt` | Optional/add-on software (third-party applications) |
| `/srv` | Data for services provided by the system (web content, FTP) |
| `/run` | Runtime variable data (replaces `/var/run`) — PIDs, sockets |
| `/mnt` | Temporary mount point for administrators |
| `/media` | Auto-mount point for removable media (USB drives, CDs) |
| `/root` | Home directory of the **root** user |
| `/sbin` | System administration binaries |
| `/selinux` | SELinux security policy files |

```bash
# View boot files
ls /boot/

# Check mounted filesystems
df -h

# View /run contents
ls /run/
```

---

## 🌲 Complete Directory Tree Diagram

```
/
├── bin        → Essential user binaries (commands)
├── boot       → Bootloader files
├── dev        → Device files
├── etc        → System configuration files
├── home       → User home directories
├── lib        → Essential shared libraries
├── lib64      → 64-bit libraries
├── media      → Removable media mount points
├── mnt        → Temporary mount point
├── opt        → Optional third-party software
├── proc       → Process information (virtual)
├── root       → Root user's home directory
├── run        → Runtime data
├── sbin       → System administration binaries
├── srv        → Service data
├── sys        → Kernel/hardware interface (virtual)
├── tmp        → Temporary files
├── usr        → User software and libraries
│   ├── bin    → Most user commands
│   ├── lib    → Libraries
│   ├── local  → Locally installed software
│   └── share  → Architecture-independent data
└── var        → Variable data (logs, caches, spools)
```

---

## 📊 Quick Reference Table

| Directory | Description | Writable By |
|-----------|-------------|-------------|
| `/` | Root of filesystem | root |
| `/bin` | Essential binaries | root |
| `/boot` | Boot files | root |
| `/dev` | Device files | root (udev) |
| `/etc` | Configuration files | root |
| `/home` | User directories | Users (own dir) |
| `/lib` | Shared libraries | root |
| `/media` | Removable media | root |
| `/mnt` | Temporary mount | root |
| `/opt` | Optional software | root |
| `/proc` | Process info (virtual) | kernel |
| `/root` | Root user's home | root |
| `/run` | Runtime data | root |
| `/sbin` | Admin binaries | root |
| `/sys` | Kernel info (virtual) | root (some) |
| `/tmp` | Temporary files | Everyone |
| `/usr` | User software | root |
| `/var` | Variable data | root (some subdirs) |

---

## 🔍 Useful Commands for Navigation

```bash
# Print current directory
pwd

# List directory contents
ls
ls -l          # Long format
ls -a          # Show hidden files
ls -la         # Long + hidden files
ls -lh         # Human-readable sizes

# Change directory
cd /etc
cd ~           # Go home
cd -           # Go to previous directory
cd ..          # Go up one level

# Show directory tree
tree           # May need: sudo apt install tree
tree -L 2 /    # Show 2 levels deep

# Find files
find /etc -name "*.conf"
locate filename

# Check disk usage
df -h          # Filesystem disk space
du -sh /var/log/  # Directory size
```

---

## ✅ Summary

| Concept | Key Takeaway |
|---------|--------------|
| **`/`** | Single root — everything starts here |
| **Unified tree** | No drive letters; all devices mounted within one tree |
| **`/etc`** | Configuration — the "control center" |
| **`/var`** | Changing data — logs, caches |
| **`/usr`** | User software — applications and libraries |
| **`/home`** | User personal directories |
| **`/dev`** | Hardware as files |
| **`/proc`** | Kernel/process info as files (virtual) |
| **`/sys`** | Kernel/hardware interface (virtual) |
| **`/tmp`** | Temporary files — may be cleared on reboot |
| **`/bin` vs `/usr/bin`** | Essential vs non-essential binaries |

---

## 📚 What's Next?

Continue your Linux learning journey:

1. **[Key Concepts](key-concepts.md)** — Permissions, everything-is-a-file, multi-user design
2. **[Basic Commands](basic-commands.md)** — Practice essential Linux commands
3. **[Linux Kernel](linux-kernel.md)** — Dive deeper into kernel subsystems
4. **[Architecture](architecture.md)** — Review user space vs kernel space

<details>
<summary>🔍 Click for additional resources</summary>

### Recommended Further Reading

- **FHS Specification**: [refspecs.linuxfoundation.org/fhs](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)
- **`man hier`** — Manual page describing filesystem hierarchy
- **`man filesystems`** — Linux filesystem types
- **"The Linux Command Line"** by William Shotts

</details>

---

> 💡 Tip  
> Spend time navigating the filesystem with `ls`, `cd`, and `tree`. The best way to learn the directory structure is by exploring it yourself on a live system.
