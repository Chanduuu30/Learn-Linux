# Introduction to Linux

> Welcome to Linux! This guide will help you understand what Linux is, its origins, and why it powers the vast majority of the world's servers and cloud infrastructure.

---

## 🐧 What Is Linux?

**Linux** is a free, open-source, Unix-like operating system kernel created by **Linus Torvalds** in 1991. It is the core component that manages hardware resources and provides essential services for all other software.

However, when people say "Linux," they are usually referring to a complete operating system — which is more accurately called **GNU/Linux**. This is because the operating system consists of:

- The **Linux kernel** (created by Linus Torvalds)
- **GNU tools and utilities** (created by the GNU Project, initiated by Richard Stallman)
- Additional open-source software from thousands of contributors worldwide

---

## 🔓 Linux as an Open-Source Operating System

Linux is **open-source**, which means:

| Feature | Description |
|---------|-------------|
| **Free to use** | No licensing fees. Anyone can download, install, and use Linux. |
| **Free to study** | The source code is publicly available. You can read and learn from it. |
| **Free to modify** | You can change the code to suit your needs. |
| **Free to distribute** | You can share the original or modified versions with anyone. |

This open development model has led to one of the most collaborative and innovative software ecosystems in history.

---

## 🤝 What Does GNU/Linux Mean?

The term **GNU/Linux** acknowledges that Linux is more than just a kernel. The complete operating system combines:

### The GNU Project

The **GNU Project** was launched in **1983** by **Richard Stallman** with the goal of creating a complete, free, Unix-compatible operating system. By the early 1990s, the GNU Project had developed:

- The **GCC compiler** — essential for building software
- The **Bash shell** — the standard command-line interpreter
- Core utilities (`ls`, `cp`, `mv`, `cat`, `grep`, etc.)
- The **GNU C Library (glibc)** — the standard C library
- Text editors (`nano`, `emacs`)
- Build tools (`make`)

> ⚠️ Important  
> By the time Linus Torvalds released the Linux kernel in 1991, the GNU Project had almost everything needed for a complete OS — except a working kernel. Linux filled that gap.

### The Linux Kernel

**Linus Torvalds**, a Finnish computer science student, began developing a free kernel as a personal project in 1991. He announced it on the `comp.os.minix` Usenet newsgroup:

> *"I'm doing a (free) operating system (just a hobby, won't be big and professional like GNU)..."*  
> — Linus Torvalds, August 25, 1991

That "hobby" went on to power the modern internet.

### The Combined System

```
+-------------------------------------------+
|              GNU/Linux OS                 |
+-------------------------------------------+
|  GNU Tools & Utilities  │  Linux Kernel   |
|  (ls, cp, bash, gcc)    │  (core OS)      |
+-------------------------------------------+
|        Hardware (CPU, RAM, Disk)          |
+-------------------------------------------+
```

The GNU tools provide the user-facing utilities and development tools, while the Linux kernel handles hardware management, memory, processes, and system calls.

---

## 🏗️ A Short Explanation of the GNU Project

The **GNU Project** is not just about software — it's about **freedom**.

### The Four Essential Freedoms

| Freedom | What It Means |
|---------|---------------|
| **Freedom 0** | The freedom to run the program as you wish, for any purpose |
| **Freedom 1** | The freedom to study how the program works and change it |
| **Freedom 2** | The freedom to redistribute copies |
| **Freedom 3** | The freedom to distribute copies of your modified versions |

These freedoms are enshrined in the **GNU General Public License (GPL)**, which ensures that software remains free and open for everyone.

### Key GNU Software Components

| Component | Purpose |
|-----------|---------|
| `gcc` | GNU Compiler Collection — compiles C, C++, and other languages |
| `glibc` | GNU C Library — standard C library used by most Linux programs |
| `bash` | Bourne Again SHell — the default command-line shell |
| `coreutils` | Essential commands: `ls`, `cp`, `mv`, `rm`, `cat`, `echo`, etc. |
| `binutils` | Binary utilities: `ld`, `as`, `objdump`, `nm` |
| `make` | Build automation tool |
| `grep` | Pattern matching and text searching |
| `findutils` | File searching utilities (`find`, `locate`) |

---

## 🌍 Why Linux Is Everywhere

Linux is the most widely used operating system in several critical domains:

### 🖥️ Servers and Cloud Infrastructure

- **Over 96% of the world's top 1 million web servers** run Linux
- **All of the top 500 supercomputers** run Linux
- **The vast majority of cloud instances** (AWS, Azure, Google Cloud) run Linux

**Why?**

| Reason | Explanation |
|--------|-------------|
| **Stability** | Linux servers can run for years without needing a reboot |
| **Performance** | Highly optimized for server workloads |
| **Security** | Strong permission model and rapid security patching |
| **Cost** | No licensing fees — free to deploy at any scale |
| **Flexibility** | Customize every aspect of the system |
| **Open-source** | Transparent code, auditable, community-driven improvements |

### 📱 Mobile

- **Android** is built on the Linux kernel
- Over **3 billion Android devices** worldwide run Linux

### 🚀 Embedded Systems & IoT

- Routers, smart TVs, cars, smart home devices
- Raspberry Pi, Arduino (some models), and more

### 🛠️ Development & DevOps

- Primary development platform for software engineers
- Powers Docker, Kubernetes, Jenkins, Ansible, and all major DevOps tools

---

## 📊 Linux Market Share

| Domain | Linux Market Share |
|--------|--------------------|
| Web servers | ~96%+ |
| Supercomputers | 100% |
| Smartphones (Android) | ~70%+ |
| Cloud infrastructure | ~90%+ |
| Stock exchange trading | ~80%+ |

---

## 🐧 Linux Distributions (Distros)

Because Linux is open-source, many organizations and communities have created their own **distributions** (or "distros") — complete operating systems built around the Linux kernel and GNU tools.

### Popular Distributions

| Distribution | Based On | Best For |
|--------------|----------|----------|
| **Ubuntu** | Debian | Beginners, desktops, servers |
| **Debian** | — | Stability, servers |
| **Fedora** | — | Cutting-edge features, development |
| **CentOS Stream / Rocky Linux / AlmaLinux** | RHEL | Enterprise servers |
| **Arch Linux** | — | Advanced users, customization |
| **openSUSE** | — | Enterprise, desktops |
| **Linux Mint** | Ubuntu | Beginners transitioning from Windows |
| **Kali Linux** | Debian | Security testing, penetration testing |

<details>
<summary>🔍 Click to learn more about choosing a distro</summary>

### How to Choose Your First Distro

If you're new to Linux, here are some recommendations:

- **Absolute beginner?** → Start with **Ubuntu** or **Linux Mint**
- **Want to learn servers?** → Use **Ubuntu Server** or **Debian**
- **Interested in enterprise Linux?** → Try **Rocky Linux** or **AlmaLinux**
- **Want cutting-edge software?** → Try **Fedora** or **Arch Linux**
- **Interested in security?** → Try **Kali Linux** (for ethical hacking only)

All distros share the same fundamental concepts covered in this documentation. The skills you learn on one distro transfer to all others.

</details>

---

## 🔄 The Relationship: GNU Tools ↔ Linux Kernel

To understand how GNU/Linux works, it's essential to know how the pieces interact:

```
+----------------------+
|   User Applications  |   (Firefox, GCC, Bash, etc.)
+----------------------+
           │
           ▼
+----------------------+
|   System Libraries   |   (glibc — provides standard functions)
+----------------------+
           │
           ▼
+----------------------+
|     GNU Utilities    |   (coreutils: ls, cp, cat, grep, etc.)
+----------------------+
           │
           ▼
+----------------------+
|      Linux Kernel    |   (Manages CPU, RAM, devices, filesystems)
+----------------------+
           │
           ▼
+----------------------+
|       Hardware       |   (CPU, Memory, Disk, Network, GPU)
+----------------------+
```

**Flow of a typical operation:**

1. A user types `ls` in the terminal
2. The **Bash shell** (GNU utility) receives the command
3. Bash calls the **glibc** library
4. glibc makes a **system call** (`getdents`) to the **Linux kernel**
5. The kernel reads the directory from the **hard drive** (hardware)
6. The result travels back up the chain and is displayed on your screen

---

## ✅ Summary

| Concept | Key Takeaway |
|---------|--------------|
| **Linux** | An open-source kernel created by Linus Torvalds in 1991 |
| **GNU/Linux** | The complete OS: GNU tools + Linux kernel |
| **GNU Project** | Started by Richard Stallman in 1983 to create a free OS |
| **Open-source** | Free to use, study, modify, and distribute |
| **Distributions** | Ubuntu, Debian, Fedora, Arch, and hundreds more |
| **Dominance** | Powers servers, cloud, supercomputers, Android, and IoT |

---

## 📚 What's Next?

Now that you understand what Linux is and its history, continue your learning journey:

1. **[Architecture](architecture.md)** — Learn how a GNU/Linux system is structured
2. **[Linux Kernel](linux-kernel.md)** — Dive into kernel internals
3. **[Filesystem Hierarchy](filesystem-hierarchy.md)** — Understand the Linux directory structure
4. **[Key Concepts](key-concepts.md)** — Permissions, multi-user design, and more
5. **[Basic Commands](basic-commands.md)** — Start using Linux hands-on

---

> 💡 Tip  
> You don't need to memorize everything in this guide. Use it as a reference. The best way to learn Linux is by using it — open a terminal and start exploring!
