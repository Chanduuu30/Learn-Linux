# Future Topics

> This file tracks planned topics for future expansion of the Linux Fundamentals documentation.

---

## 📝 Planned Topics

The following topics are planned for future documentation additions:

### 🐚 Shell Scripting

- Bash scripting basics (variables, conditionals, loops)
- Functions and arguments
- Reading user input
- Writing reusable scripts
- Debugging techniques
- Best practices and conventions

### 🔗 Networking Fundamentals

- TCP/IP model overview
- Network interfaces and configuration
- `ip` command (replacing `ifconfig`)
- `ss` command (replacing `netstat`)
- DNS and name resolution
- Firewall basics (`iptables`, `nftables`, `ufw`)
- SSH configuration and key-based authentication
- Network troubleshooting (`ping`, `traceroute`, `mtr`, `dig`, `nslookup`)

### 📦 Package Management

- APT (Debian/Ubuntu): `apt`, `apt-get`, `apt-cache`
- DNF/YUM (Fedora/RHEL): `dnf`, `yum`
- Pacman (Arch Linux)
- Snap and Flatpak (universal packages)
- Building from source
- Repository management

### 👤 User and Group Management

- `/etc/passwd`, `/etc/shadow`, `/etc/group` explained
- `useradd`, `usermod`, `userdel`
- `groupadd`, `groupmod`, `groupdel`
- Password policies
- PAM (Pluggable Authentication Modules) overview
- Sudo configuration (`/etc/sudoers`)

### 🔐 Security Basics

- File permissions and special bits (advanced)
- SELinux and AppArmor introduction
- Fail2ban
- SSH hardening
- Firewall configuration
- Auditing and monitoring
- File integrity checking (AIDE, Tripwire)

### 💾 Storage Management

- Partitions and filesystems (`fdisk`, `parted`, `mkfs`)
- Mounting and `/etc/fstab`
- LVM (Logical Volume Manager)
- RAID basics
- Swap management
- Disk encryption (LUKS)

### 🚀 Process and Service Management

- systemd in depth (`systemctl`, `journalctl`)
- Creating and managing services
- Targets and runlevels
- Timers (replacement for cron)
- Process priorities and cgroups
- Namespaces and containers introduction

### 🐳 Introduction to Containers

- What are containers?
- Docker basics
- Podman
- Container images and registries
- Docker Compose
- Relationship between containers and Linux namespaces/cgroups

### 📊 System Monitoring and Logging

- systemd journal
- Log rotation (`logrotate`)
- Monitoring tools (`vmstat`, `iostat`, `sar`, `dstat`)
- Setting up alerts
- Introduction to centralized logging (rsyslog, ELK stack)

### 🔧 Text Editors

- **nano** — Beginner-friendly editor
- **vim** — Powerful modal editor (modes, navigation, editing)
- **emacs** — Extensible editor overview
- Choosing the right editor

### 📅 Cron and Automation

- Cron syntax and crontab management
- System-wide cron directories
- `at` command for one-time scheduling
- Systemd timers as a cron alternative

### 🌐 Web Server Basics

- Installing and configuring Nginx
- Apache HTTP Server basics
- Virtual hosts
- SSL/TLS certificates (Let's Encrypt)
- Reverse proxy concepts

### 🗄️ Linux for DevOps

- Infrastructure as Code introduction
- Ansible basics
- Git on Linux
- CI/CD pipeline concepts
- Cloud instances and SSH

---

## 🤝 Contribute

Have a topic you'd like to see covered? Feel free to:

1. Open an **issue** suggesting the topic
2. Submit a **pull request** with new documentation
3. Improve existing content with corrections and enhancements

---

## 📋 Topic Status Legend

| Status | Meaning |
|--------|---------|
| 📝 Planned | Topic is planned for future development |
| 🚧 In Progress | Content is being written |
| ✅ Complete | Documentation is available |
| 🔍 Review | Content is under review |

---

> 💡 Tip  
> This file is a living document. Check back periodically for new topics and updates.
