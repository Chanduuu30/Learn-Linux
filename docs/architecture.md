# GNU/Linux System Architecture

> This document explains the architecture of a GNU/Linux system, covering the layers from user applications down to hardware, and the crucial distinction between user space and kernel space.

---

## 🏗️ Overview

A GNU/Linux system is structured in clearly defined **layers**, each with specific responsibilities. Understanding this architecture is fundamental to understanding how Linux works.

```
+--------------------------------------------------+
|              User Applications                    |
|  (Web browsers, editors, compilers, scripts)      |
+--------------------------------------------------+
                      │
                      ▼
+--------------------------------------------------+
|              System Libraries                     |
|  (glibc — GNU C Library, pthreads, libm, etc.)    |
+--------------------------------------------------+
                      │
                      ▼
+--------------------------------------------------+
|              GNU Utilities                        |
|  (coreutils: ls, cp, mv, cat, grep, bash, etc.)   |
+--------------------------------------------------+
                      │
                      ▼
+--------------------------------------------------+
|                 Linux Kernel                      |
|  (Process mgmt, memory mgmt, drivers, syscalls)   |
+--------------------------------------------------+
                      │
                      ▼
+--------------------------------------------------+
|                   Hardware                        |
|  (CPU, RAM, hard drives, network cards, GPU)      |
+--------------------------------------------------+
```

---

## 🧩 The Two Worlds: User Space vs Kernel Space

The most important architectural concept in Linux is the separation between **user space** and **kernel space**.

### What Is Kernel Space?

**Kernel space** is a protected area of memory where the operating system kernel runs. It has:

- **Full, unrestricted access** to all hardware and system resources
- **Privileged CPU mode** (Ring 0 on x86 architecture)
- Direct access to memory, devices, and CPU instructions
- The ability to execute any instruction

```
+--------------------------------------------------+
│               KERNEL SPACE                        │
│                                                   │
│  ┌──────────────────────────────────────────────┐ │
│  │          Linux Kernel                        │ │
│  │  • Process Management                        │ │
│  │  • Memory Management                         │ │
│  │  • Device Drivers                            │ │
│  │  • Filesystem Management                     │ │
│  │  • Network Stack                             │ │
│  │  • System Call Interface                     │ │
│  └──────────────────────────────────────────────┘ │
│                                                   │
│  Privileged Mode (Ring 0)                        │
│  Full hardware access                            │
+--------------------------------------------------+
```

### What Is User Space?

**User space** is where all normal applications and utilities run. It has:

- **Restricted access** — cannot directly access hardware or kernel memory
- **Unprivileged CPU mode** (Ring 3 on x86 architecture)
- Must request kernel services through **system calls**
- Isolated from other processes for security and stability

```
+--------------------------------------------------+
│               USER SPACE                          │
│                                                   │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│  │ Application │ │ Application │ │ Application │ │
│  │   (Firefox) │ │   (Vim)     │ │   (bash)    │ │
│  └─────────────┘ └─────────────┘ └─────────────┘ │
│                                                   │
│  ┌──────────────────────────────────────────────┐ │
│  │         System Libraries (glibc)             │ │
│  └──────────────────────────────────────────────┘ │
│                                                   │
│  ┌──────────────────────────────────────────────┐ │
│  │           GNU Utilities                      │ │
│  │   (ls, cp, mv, cat, grep, find, etc.)        │ │
│  └──────────────────────────────────────────────┘ │
│                                                   │
│  Unprivileged Mode (Ring 3)                      │
│  Restricted access, isolated processes           │
+--------------------------------------------------+
```

### Why Separate User Space and Kernel Space?

| Benefit | Explanation |
|---------|-------------|
| **Security** | Applications cannot access hardware or other processes' memory directly |
| **Stability** | A crashing application won't crash the entire system |
| **Isolation** | Each process runs in its own protected memory space |
| **Control** | The kernel mediates all access to shared resources |
| **Abstraction** | Applications don't need to know hardware details |

> ⚠️ Important  
> If an application in user space crashes, only that application is affected. If something crashes in kernel space, the entire system can go down (kernel panic).

---

## 📊 Detailed Architecture: Layer by Layer

### Layer 1: User Applications

This is the top layer — the programs you interact with directly.

**Examples:**

| Type | Examples |
|------|----------|
| **GUI Applications** | Firefox, LibreOffice, GIMP, VLC |
| **CLI Applications** | `vim`, `nano`, `htop`, `git`, `python3` |
| **Servers** | `nginx`, `apache2`, `sshd`, `mysqld` |
| **Development Tools** | `gcc`, `make`, `cmake`, `docker` |
| **Shell** | `bash`, `zsh`, `fish` |

User applications **never** talk to hardware directly. They must go through system libraries and the kernel.

---

### Layer 2: System Libraries (glibc)

**System libraries** provide standardized functions that applications use to request services from the kernel.

The most important library is the **GNU C Library (glibc)**. It provides:

- Standard C library functions (`printf`, `malloc`, `strcpy`, etc.)
- **System call wrappers** — convenient functions that make kernel requests
- Threading support (`pthreads`)
- Dynamic linker/loader (`ld.so`)

**How it works:**

When a programmer writes:
```c
FILE *f = fopen("/etc/hosts", "r");
```

Behind the scenes, `fopen()` (from glibc) eventually makes a system call:
```c
open("/etc/hosts", O_RDONLY)
```

This system call transitions from **user space → kernel space**.

**Common System Libraries:**

| Library | Purpose |
|---------|---------|
| `glibc` | GNU C Library — standard C functions and syscalls |
| `libpthread` | POSIX threading library |
| `libm` | Mathematical functions |
| `libdl` | Dynamic linking interface |
| `librt` | Real-time extensions |

---

### Layer 3: GNU Utilities

GNU utilities are the essential command-line tools that come with every Linux system. They are part of the **GNU coreutils** package and many others.

**Categories:**

| Category | Commands |
|----------|----------|
| **File operations** | `ls`, `cp`, `mv`, `rm`, `cat`, `touch`, `mkdir` |
| **Text processing** | `grep`, `awk`, `sed`, `sort`, `uniq`, `wc` |
| **Shell** | `bash`, command history, pipes, redirections |
| **File search** | `find`, `locate`, `which` |
| **System info** | `uname`, `uptime`, `free`, `df`, `du` |
| **User management** | `useradd`, `passwd`, `su`, `sudo` |
| **Process management** | `ps`, `kill`, `top`, `nice` |

These utilities are technically user-space applications, but they are considered "core" because they are essential for system operation.

---

### Layer 4: The Linux Kernel

The **Linux kernel** is the heart of the operating system. It manages all hardware resources and provides services to user-space applications.

**Kernel responsibilities:**

```
+--------------------------------------------------+
│                  LINUX KERNEL                     │
│                                                   │
│  ┌─────────────┐  ┌─────────────┐ ┌────────────┐ │
│  │  Process    │  │   Memory    │ │   Device   │ │
│  │ Management  │  │ Management  │ │  Drivers   │ │
│  └─────────────┘  └─────────────┘ └────────────┘ │
│                                                   │
│  ┌─────────────┐  ┌─────────────┐ ┌────────────┐ │
│  │  Filesystem │  │   Network   │ │   System   │ │
│  │ Management  │  │    Stack    │ │   Calls    │ │
│  └─────────────┘  └─────────────┘ └────────────┘ │
│                                                   │
│  ┌─────────────┐  ┌─────────────┐                 │
│  │ Interrupt   │  │   Security  │                 │
│  │ Handling    │  │  (SELinux,  │                 │
│  │             │  │  AppArmor)  │                 │
│  └─────────────┘  └─────────────┘                 │
+--------------------------------------------------+
```

Each of these subsystems is covered in detail in the [Linux Kernel document](linux-kernel.md).

---

### Layer 5: Hardware

The **hardware layer** consists of all physical (or virtual) components:

| Component | Role |
|-----------|------|
| **CPU** | Executes instructions, managed by kernel scheduler |
| **RAM** | Temporary memory, managed by kernel memory manager |
| **Hard Drive / SSD** | Persistent storage, accessed via filesystem drivers |
| **Network Interface Card (NIC)** | Network communication, managed by network stack |
| **GPU** | Graphics processing, accessed via device drivers |
| **USB Controller** | Peripheral device connections |
| **Sound Card** | Audio input/output |

The kernel **abstracts** this hardware so applications don't need to know specific hardware details.

---

## 🔄 Interaction Flow: How a Command Works

Let's trace what happens when you run a simple command like `ls`:

```
User types:  ls
     │
     ▼
┌─────────────────────────────────────────────────┐
│  1. Bash (shell) parses the command             │
│     → Identifies "ls" as an executable          │
└─────────────────────────────────────────────────┘
     │
     ▼
┌─────────────────────────────────────────────────┐
│  2. Bash calls exec() system call               │
│     → User space → Kernel space transition      │
└─────────────────────────────────────────────────┘
     │
     ▼
┌─────────────────────────────────────────────────┐
│  3. Kernel loads /bin/ls into memory            │
│     → Sets up process, allocates resources      │
└─────────────────────────────────────────────────┘
     │
     ▼
┌─────────────────────────────────────────────────┐
│  4. ls calls glibc functions                     │
│     → readdir() → getdents64() syscall          │
└─────────────────────────────────────────────────┘
     │
     ▼
┌─────────────────────────────────────────────────┐
│  5. Kernel filesystem driver reads directory    │
│     → Ext4/XFS driver reads from disk           │
└─────────────────────────────────────────────────┘
     │
     ▼
┌─────────────────────────────────────────────────┐
│  6. Kernel returns data to ls                   │
│     → Kernel space → User space transition      │
└─────────────────────────────────────────────────┘
     │
     ▼
┌─────────────────────────────────────────────────┐
│  7. ls formats and prints output to terminal    │
│     → You see the file listing                  │
└─────────────────────────────────────────────────┘
```

### Simplified Flow Diagram

```
Application (ls)
     │
     ▼
System Libraries (glibc — readdir())
     │
     ▼
System Call (getdents64)  ←── User Space / Kernel Space Boundary
     │
     ▼
Linux Kernel (VFS → Ext4 Driver → Disk Controller)
     │
     ▼
Hardware (Hard Drive / SSD)
     │
     ▼
Data returns back up the chain to your terminal
```

---

## 🔌 System Calls: The Bridge Between Spaces

**System calls** are the only way user-space applications can request services from the kernel. They are the **bridge** between user space and kernel space.

```
┌──────────────────────────────────────────────────────┐
│                    USER SPACE                         │
│                                                       │
│  Application calls: open(), read(), write(), fork()  │
│         │                                             │
│         ▼                                             │
│  ┌──────────────────────────────────────────┐        │
│  │       glibc (system call wrappers)       │        │
│  └──────────────────────────────────────────┘        │
│         │                                             │
│         ▼                                             │
│  ┌──────────────────────────────────────────┐        │
│  │     CPU instruction: syscall / int 0x80  │        │
│  └──────────────────────────────────────────┘        │
└──────────────────────────────────────────────────────┘
                     │
          ═══════════╪═══════════════  BOUNDARY
                     │
┌──────────────────────────────────────────────────────┐
│                   KERNEL SPACE                         │
│         │                                             │
│         ▼                                             │
│  ┌──────────────────────────────────────────┐        │
│  │    System Call Table (sys_call_table)    │        │
│  │    → sys_open(), sys_read(), sys_write() │        │
│  └──────────────────────────────────────────┘        │
│         │                                             │
│         ▼                                             │
│  Kernel performs requested operation                 │
│         │                                             │
│         ▼                                             │
│  Result returned to user space                       │
└──────────────────────────────────────────────────────┘
```

### Common System Calls

| System Call | Purpose | Used By |
|-------------|---------|---------|
| `open()` | Open a file | `cat`, `vim`, all file operations |
| `read()` | Read from a file descriptor | Almost every program |
| `write()` | Write to a file descriptor | Almost every program |
| `fork()` | Create a new process | Shell executing commands |
| `exec()` | Replace process with new program | Shell running commands |
| `exit()` | Terminate a process | Program completion |
| `wait()` | Wait for child process | Parent processes |
| `brk()` / `mmap()` | Memory allocation | `malloc()`, dynamic memory |
| `chmod()` | Change file permissions | `chmod` command |
| `chdir()` | Change working directory | `cd` command |

> 💡 Tip  
> You can view all system calls made by a program using `strace`. Try: `strace ls` to see every system call the `ls` command makes.

---

## 🛡️ Protection Rings (x86 Architecture)

On x86 processors, the CPU implements **protection rings** to enforce the separation between user space and kernel space:

```
+------------------------------------------+
│  Ring 0 — Kernel Mode                    │
│  • Full hardware access                  │
│  • Unrestricted instructions              │
│  • Linux kernel runs here                │
│  • Device drivers run here               │
+------------------------------------------+
│  Ring 1 — (Unused in Linux)              │
+------------------------------------------+
│  Ring 2 — (Unused in Linux)              │
+------------------------------------------+
│  Ring 3 — User Mode                      │
│  • Restricted instructions only          │
│  • No direct hardware access             │
│  • Applications run here                 │
│  • Must use syscalls for kernel services │
+------------------------------------------+
```

> ⚠️ Important  
> Linux only uses **Ring 0** (kernel) and **Ring 3** (user). Rings 1 and 2 exist in the x86 architecture but are not used by Linux.

---

## 📦 Kernel Modules: Extending the Kernel

Linux supports **loadable kernel modules (LKMs)** — pieces of code that can be loaded into and unloaded from the kernel at runtime.

```
+--------------------------------------------------+
│                 Linux Kernel                      │
│                                                   │
│  ┌──────────────────────────────────────────────┐ │
│  │              Core Kernel                      │ │
│  │  (Always loaded, always running)              │ │
│  └──────────────────────────────────────────────┘ │
│                       │                           │
│         ┌─────────────┼─────────────┐            │
│         ▼             ▼             ▼             │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐        │
│  │ Module:  │  │ Module:  │  │ Module:  │        │
│  │ ext4.ko  │  │ nvidia   │  │ snd_hda  │        │
│  │          │  │ .ko      │  │ _intel.ko│        │
│  └──────────┘  └──────────┘  └──────────┘        │
│                                                   │
│  Modules can be loaded/removed dynamically        │
+--------------------------------------------------+
```

**Common kernel modules:**

| Module | Purpose |
|--------|---------|
| `ext4.ko` | Ext4 filesystem driver |
| `nvidia.ko` | NVIDIA GPU driver |
| `snd_hda_intel.ko` | Intel HD audio driver |
| `e1000.ko` | Intel network card driver |
| `usb_storage.ko` | USB mass storage driver |

**Useful commands:**

```bash
# List currently loaded kernel modules
lsmod

# Load a kernel module
sudo modprobe ext4

# Remove a kernel module
sudo rmmod ext4

# Show module information
modinfo ext4
```

---

## 🌐 User Space vs Kernel Space: Comparison Table

| Feature | User Space | Kernel Space |
|---------|------------|--------------|
| **Privilege level** | Ring 3 (unprivileged) | Ring 0 (privileged) |
| **Hardware access** | Indirect (via syscalls) | Direct |
| **Memory access** | Own process only | All memory |
| **Crash impact** | Only the application | Entire system (kernel panic) |
| **Examples** | Firefox, bash, ls, python | Kernel core, device drivers |
| **Context switching** | Between processes | Between user/kernel mode |
| **Debugging tools** | `gdb`, `strace` | `kgdb`, `crash`, `ftrace` |

---

## ✅ Summary

| Concept | Key Takeaway |
|---------|--------------|
| **User Space** | Where applications run — restricted, isolated, safe |
| **Kernel Space** | Where the kernel runs — full access, privileged, critical |
| **System Calls** | The only bridge between user space and kernel space |
| **glibc** | Provides convenient wrappers for system calls |
| **GNU Utilities** | Core command-line tools in user space |
| **Kernel Modules** | Dynamically loadable kernel code (drivers, filesystems) |
| **Protection Rings** | CPU-level enforcement of user/kernel separation |
| **Interaction Flow** | App → Library → Syscall → Kernel → Hardware |

---

## 📚 What's Next?

Now that you understand Linux architecture, continue your learning:

1. **[Linux Kernel](linux-kernel.md)** — Deep dive into kernel internals
2. **[Filesystem Hierarchy](filesystem-hierarchy.md)** — Understand the Linux directory structure
3. **[Key Concepts](key-concepts.md)** — Permissions, everything-is-a-file, and more
4. **[Basic Commands](basic-commands.md)** — Practice with essential Linux commands

<details>
<summary>🔍 Click for additional resources</summary>

### Recommended Further Reading

- **`man 2 syscalls`** — Manual page listing all system calls
- **`strace`** — Trace system calls and signals
- **`ltrace`** — Trace library calls
- **"Understanding the Linux Kernel"** by Daniel P. Bovet and Marco Cesati
- **"Linux System Programming"** by Robert Love

</details>

---

> 💡 Tip  
> Understanding the user space / kernel space boundary is one of the most important concepts in Linux. Whenever something goes wrong, ask yourself: "Did this fail in user space or kernel space?" This question will guide your troubleshooting.
