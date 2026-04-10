# The Linux Kernel

> The Linux kernel is the core of the operating system. This document explains kernel architecture, responsibilities, and key subsystems: process management, memory management, device drivers, filesystem management, and the system call interface.

---

## 🧠 What Is the Linux Kernel?

The **Linux kernel** is the central component of a GNU/Linux operating system. It is the first program loaded after the bootloader and remains in memory (RAM) for the entire duration the system is running.

The kernel is responsible for:

- Managing the **CPU** and deciding which processes run when
- Managing **memory (RAM)** and allocating it to processes
- Communicating with **hardware** through device drivers
- Providing a **filesystem** to store and retrieve data
- Handling **network** communication
- Enforcing **security** and access control

```
+--------------------------------------------------+
│                  Linux Kernel                      │
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
│  │ Handling    │  │  Subsystem  │                 │
│  └─────────────┘  └────────────┘                 │
+--------------------------------------------------+
```

---

## 🏛️ Monolithic Kernel Architecture

Linux uses a **monolithic kernel** architecture.

### What Does "Monolithic" Mean?

In a monolithic kernel, **all core operating system services run in kernel space** as part of a single large program. This includes:

- Process management
- Memory management
- Device drivers
- Filesystem drivers
- Network stack
- System call handling

```
+--------------------------------------------------+
│            Monolithic Kernel                      │
│                                                   │
│  ┌──────────────────────────────────────────────┐ │
│  │          All services in kernel space         │ │
│  │                                               │ │
│  │  ┌───────────┐ ┌───────────┐ ┌───────────┐  │ │
│  │  │ Process   │ │  Memory   │ │  Device   │  │ │
│  │  │ Mgmt      │ │  Mgmt     │ │  Drivers  │  │ │
│  │  └───────────┘ └───────────┘ └───────────┘  │ │
│  │  ┌───────────┐ ┌───────────┐ ┌───────────┐  │ │
│  │  │Filesystem │ │  Network  │ │   Syscall │  │ │
│  │  │  Drivers  │ │   Stack   │ │  Handler  │  │ │
│  │  └───────────┘ └───────────┘ └───────────┘  │ │
│  └──────────────────────────────────────────────┘ │
+--------------------------------------------------+
```

### Monolithic vs Microkernel

| Feature | Monolithic (Linux) | Microkernel (Mach, QNX) |
|---------|-------------------|------------------------|
| **Design** | All services in kernel space | Minimal kernel; services in user space |
| **Performance** | Faster — direct function calls | Slower — inter-process communication overhead |
| **Stability** | A buggy driver can crash the whole system | A buggy driver only crashes its own process |
| **Size** | Larger kernel image | Smaller kernel core |
| **Modularity** | Achieved via loadable kernel modules | Built into the design |
| **Examples** | Linux, traditional Unix | Minix, QNX, L4 |

### Linux: Monolithic + Modular

While Linux is monolithic, it achieves modularity through **Loadable Kernel Modules (LKMs)**. Modules can be loaded and unloaded at runtime without rebooting.

```bash
# View loaded kernel modules
lsmod

# Load a module
sudo modprobe <module_name>

# Unload a module
sudo rmmod <module_name>

# List all available modules
ls /lib/modules/$(uname -r)/
```

> 💡 Tip  
> Linux combines the **performance** of a monolithic kernel with the **flexibility** of modularity through LKMs. This gives it the best of both worlds.

---

## ⚙️ Kernel Subsystems

### 1. Process Management

The kernel manages all running processes — creating, scheduling, and terminating them.

#### Process Creation

When you run a command in the terminal, the kernel creates a new process through the `fork()` and `exec()` system calls:

```bash
$ ls
```

What happens internally:

```
1. Bash calls fork()
   → Kernel creates a copy of the bash process (child process)

2. Child process calls exec("/bin/ls")
   → Kernel replaces child's memory with the ls program

3. ls runs and produces output
   → Output goes to the terminal

4. ls exits
   → Kernel cleans up the process

5. Bash calls wait()
   → Bash learns that ls finished and gets its exit status
```

```
Parent Process (bash)                Child Process
      │                                   │
      ├── fork() ──────────────────────────→│
      │                                   │
      │                          ┌────────┴────────┐
      │                          │  Copy of bash   │
      │                          └────────┬────────┘
      │                                   │
      │                          exec("/bin/ls")
      │                                   │
      │                          ┌────────┴────────┐
      │                          │     ls runs     │
      │                          │  Produces output│
      │                          └────────┬────────┘
      │                                   │
      │                          exit() ──→│
      │                                   │
      ├── wait() ──────────────────────────→│
      │     ←── exit status returned ───────│
      │                                   │
```

#### Process States

Every process in Linux is in one of these states:

| State | Code | Description |
|-------|------|-------------|
| **Running** | `R` | Currently executing or ready to execute |
| **Sleeping (interruptible)** | `S` | Waiting for an event (I/O, signal) |
| **Sleeping (uninterruptible)** | `D` | Waiting for I/O — cannot be interrupted |
| **Stopped** | `T` | Process has been stopped (e.g., by Ctrl+Z) |
| **Zombie** | `Z` | Process finished but parent hasn't collected its status |

```bash
# View process states
ps aux

# View process tree
pstree

# Real-time process monitoring
top
# or
htop
```

#### CPU Scheduling

The kernel's **scheduler** decides which process runs on the CPU and for how long.

Linux uses the **Completely Fair Scheduler (CFS)**, introduced in kernel 2.6.23 (2007).

**How CFS Works:**

```
+--------------------------------------------------+
│             Completely Fair Scheduler (CFS)       │
│                                                   │
│  ┌──────────────────────────────────────────────┐ │
│  │          Red-Black Tree                      │ │
│  │  (sorted by virtual runtime)                  │ │
│  │                                               │ │
│  │    Process A (vruntime: 120ms)               │ │
│  │         \                                    │ │
│  │      Process B (vruntime: 150ms)             │ │
│  │         /                                    │ │
│  │    Process C (vruntime: 180ms)               │ │
│  │                                               │ │
│  │  ← Always pick the leftmost (lowest vruntime) │ │
│  └──────────────────────────────────────────────┘ │
│                                                   │
│  • Each process gets "fair" CPU time              │
│  • No process starves                            │
│  • Nice values adjust priority                    │
+--------------------------------------------------+
```

**Key CFS Concepts:**

| Concept | Description |
|---------|-------------|
| **Virtual Runtime (vruntime)** | Tracks how much CPU time a process has received |
| **Red-Black Tree** | Data structure used to efficiently find the next process to run |
| **Fairness** | Process with lowest vruntime gets scheduled next |
| **Nice Values** | Range from -20 (highest priority) to +19 (lowest priority) |
| **Time Slicing** | Each process gets a portion of CPU time before another runs |

```bash
# View nice value of processes
ps -eo pid,ni,comm

# Run a command with a different nice value
nice -n 10 ./my_program

# Change nice value of a running process
renice -n 5 -p <PID>
```

> ⚠️ Important  
> Only the **root** user can set negative nice values (increase priority). Regular users can only decrease priority (positive nice values).

---

### 2. Memory Management

The kernel manages the system's **RAM**, allocating memory to processes and ensuring efficient use of this finite resource.

#### Virtual Memory

Linux uses **virtual memory** — each process sees its own contiguous address space, even though the actual data may be scattered across physical RAM or partially on disk.

```
+--------------------------------------------------+
│              Virtual Memory Model                  │
│                                                   │
│  Process A's View          Process B's View       │
│  ┌──────────────┐          ┌──────────────┐      │
│  │ 0xFFFFFFFF   │          │ 0xFFFFFFFF   │      │
│  │     ...      │          │     ...      │      │
│  │ 0x08048000   │          │ 0x08048000   │      │
│  │  (code)      │          │  (code)      │      │
│  │ 0x00000000   │          │ 0x00000000   │      │
│  └──────────────┘          └──────────────┘      │
│         │                          │              │
│         ▼                          ▼              │
│  ┌──────────────────────────────────────┐        │
│  │         Memory Management Unit       │        │
│  │            (MMU — hardware)           │        │
│  │  Maps virtual → physical addresses    │        │
│  └──────────────────────────────────────┘        │
│         │                          │              │
│         ▼                          ▼              │
│  ┌──────────────────────────────────────┐        │
│  │          Physical RAM                │        │
│  │  [Page frames allocated to processes]│        │
│  └──────────────────────────────────────┘        │
+--------------------------------------------------+
```

**Benefits of Virtual Memory:**

| Benefit | Explanation |
|---------|-------------|
| **Isolation** | Each process has its own address space — one process cannot corrupt another |
| **Abstraction** | Processes see a simple, contiguous address space |
| **Efficiency** | Only actually used pages need physical RAM |
| **Swapping** | Unused pages can be moved to disk to free RAM |

#### Paging

Memory is managed in fixed-size blocks called **pages** (typically 4 KB on x86).

```
Virtual Address Space                    Physical RAM
┌──────────────────────┐              ┌──────────────────────┐
│  Page 0  │ 0x0000    │              │  Frame 5  │ 0x5000    │
├──────────┼───────────┤              ├──────────┼───────────┤
│  Page 1  │ 0x1000    │──────┐       ├──────────┼───────────┤
├──────────┼───────────┤      │       │  Frame 2  │ 0x2000    │
│  Page 2  │ 0x2000    │      │       ├──────────┼───────────┤
├──────────┼───────────┤      │  ┌───→│  Frame 8  │ 0x8000    │
│  Page 3  │ 0x3000    │      │  │    ├──────────┼───────────┤
├──────────┼───────────┤      │  │    │  Frame 1  │ 0x1000    │
│   ...    │   ...     │      │  │    └──────────┴───────────┘
└──────────────────────┘      │  │
                              │  └── Page Table (managed by kernel)
                              │      Maps virtual pages to physical frames
                              │
                              └── Some pages may be on disk (swapped out)
```

#### Swap Space

When physical RAM runs low, the kernel can move inactive pages to a dedicated area on disk called **swap space**.

```
+--------------------------------------------------+
│                   Memory Hierarchy                 │
│                                                   │
│  ┌──────────────────────────────────────────────┐ │
│  │             CPU Registers                     │ │
│  │  (fastest, smallest, per-core)                │ │
│  └────────────────────┬─────────────────────────┘ │
│                       ▼                            │
│  ┌──────────────────────────────────────────────┐ │
│  │             CPU Cache (L1/L2/L3)             │ │
│  │  (very fast, small)                          │ │
│  └────────────────────┬─────────────────────────┘ │
│                       ▼                            │
│  ┌──────────────────────────────────────────────┐ │
│  │              RAM (Physical Memory)            │ │
│  │  (fast, moderate size — 4GB, 8GB, 16GB...)   │ │
│  └────────────────────┬─────────────────────────┘ │
│                       ▼                            │
│  ┌──────────────────────────────────────────────┐ │
│  │           Swap Space (On Disk)                │ │
│  │  (slow, large — used when RAM is full)        │ │
│  └──────────────────────────────────────────────┘ │
+--------------------------------------------------+
```

```bash
# Check memory and swap usage
free -h

# Check swap configuration
swapon --show

# View detailed memory info
cat /proc/meminfo
```

> ⚠️ Important  
> Swap is much slower than RAM. If your system is heavily using swap ("thrashing"), consider adding more physical RAM.

---

### 3. Device Drivers

**Device drivers** are kernel modules that allow the kernel to communicate with hardware devices.

```
+--------------------------------------------------+
│               Device Driver Layer                 │
│                                                   │
│  Application (user space)                        │
│         │                                         │
│         ▼                                         │
│  ┌──────────────────────────────────────────────┐ │
│  │  Character Device │ Block Device │ Network   │ │
│  │  (keyboards,      │ (disks,      │ (NICs,    │ │
│  │   serial ports)   │  USB drives) │  Wi-Fi)   │ │
│  └──────────────────────────────────────────────┘ │
│         │                                         │
│         ▼                                         │
│  ┌──────────────────────────────────────────────┐ │
│  │          Specific Device Drivers              │ │
│  │  • e1000 (Intel Ethernet)                    │ │
│  │  • ahci (SATA controller)                    │ │
│  │  • nvidia (GPU)                              │ │
│  │  • snd_hda_intel (Audio)                     │ │
│  │  • usbhid (USB input devices)                │ │
│  └──────────────────────────────────────────────┘ │
│         │                                         │
│         ▼                                         │
│  Hardware (physical devices)                      │
+--------------------------------------------------+
```

**Device Types:**

| Type | Description | Examples |
|------|-------------|----------|
| **Character Devices** | Stream of bytes, sequential access | Keyboard, mouse, serial ports, `/dev/tty` |
| **Block Devices** | Random access in fixed-size blocks | Hard drives, SSDs, USB drives, `/dev/sda` |
| **Network Devices** | Packet-based communication | Ethernet cards, Wi-Fi adapters, `eth0`, `wlan0` |

```bash
# List loaded drivers
lsmod

# View hardware information
lspci          # PCI devices
lsusb          # USB devices
lscpu          # CPU information

# View device files
ls -l /dev/
```

---

### 4. File System Management

The kernel supports multiple filesystem types and provides a **Virtual File System (VFS)** layer that abstracts them all into a unified interface.

```
+--------------------------------------------------+
│            File System Architecture                │
│                                                   │
│  Applications (open, read, write, close)          │
│         │                                         │
│         ▼                                         │
│  ┌──────────────────────────────────────────────┐ │
│  │      Virtual File System (VFS) Layer         │ │
│  │  (Unified interface for all filesystems)      │ │
│  └────────────────────┬─────────────────────────┘ │
│                       │                            │
│         ┌─────────────┼─────────────┐             │
│         ▼             ▼             ▼              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐        │
│  │  ext4    │  │   xfs    │  │  btrfs   │        │
│  │  driver  │  │  driver  │  │  driver  │        │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘        │
│       │              │             │               │
│       ▼              ▼             ▼               │
│  ┌──────────────────────────────────────┐         │
│  │        Block Device Layer             │         │
│  │        (/dev/sda1, /dev/sda2)        │         │
│  └──────────────────────────────────────┘         │
│                       │                            │
│                       ▼                            │
│              Physical Storage                      │
│              (Hard Drive / SSD)                    │
+--------------------------------------------------+
```

**Common Linux Filesystems:**

| Filesystem | Description | Use Case |
|------------|-------------|----------|
| **ext4** | Fourth Extended Filesystem — mature, reliable, default on many distros | General purpose, desktops, servers |
| **XFS** | High-performance, 64-bit journaling filesystem | Large files, servers, enterprise |
| **Btrfs** | Copy-on-write, snapshots, compression, RAID support | Advanced features, backups |
| **FAT32/NTFS** | Windows-compatible filesystems | USB drives, dual-boot systems |
| **tmpfs** | RAM-based filesystem | `/tmp`, `/run` |
| **procfs** | Process information filesystem | `/proc` |
| **sysfs** | Kernel/hardware info filesystem | `/sys` |

```bash
# Check filesystem types on your system
df -T

# View mounted filesystems
mount

# View filesystem details
lsblk -f
```

---

### 5. System Call Interface

The **System Call Interface (SCI)** is the gateway through which user-space applications request services from the kernel.

```
User Space:

  Application
       │
       ▼
  glibc wrapper function
       │  (e.g., fopen(), malloc())
       ▼
  System call instruction
       │  (syscall on x86_64, int 0x80 on x86)
       │
═══════╪═══════════════════════════════════════════ Boundary
       │
Kernel Space:
       ▼
  System Call Table (sys_call_table)
       │
       ├─→ sys_read()
       ├─→ sys_write()
       ├─→ sys_open()
       ├─→ sys_close()
       ├─→ sys_fork()
       ├─→ sys_exec()
       ├─→ sys_exit()
       └─→ ... (300+ more)
```

#### Example System Calls

| System Call | Number (x86_64) | Purpose |
|-------------|:--------------:|---------|
| `read` | 0 | Read from a file descriptor |
| `write` | 1 | Write to a file descriptor |
| `open` | 2 | Open a file |
| `close` | 3 | Close a file descriptor |
| `stat` | 4 | Get file status |
| `fstat` | 5 | Get file status (via fd) |
| `mmap` | 9 | Map memory |
| `mprotect` | 10 | Change memory protection |
| `brk` | 12 | Change data segment size |
| `fork` | 57 | Create a child process |
| `execve` | 59 | Execute a program |
| `exit` | 60 | Terminate the calling process |
| `wait4` | 61 | Wait for process to change state |
| `kill` | 62 | Send signal to a process |
| `getcwd` | 79 | Get current working directory |
| `chmod` | 90 | Change file permissions |
| `socket` | 41 | Create a socket |
| `connect` | 42 | Connect a socket |

#### Example: What `cat file.txt` Does Internally

```c
// What the programmer writes:
FILE *f = fopen("file.txt", "r");
char buf[1024];
fread(buf, 1, 1024, f);
fwrite(buf, 1, bytes_read, stdout);
fclose(f);

// What actually happens (system calls):
fd = open("file.txt", O_RDONLY);       // sys_open
bytes_read = read(fd, buf, 1024);      // sys_read
write(STDOUT_FILENO, buf, bytes_read); // sys_write
close(fd);                              // sys_close
```

> 💡 Tip  
> Use `strace` to observe system calls in real time:
> ```bash
> strace cat /etc/hosts
> ```
> This shows every system call `cat` makes to read and display the file.

---

## 🔀 Context Switching

**Context switching** is the process by which the kernel saves the state of one process and loads the state of another, enabling multitasking.

```
Process A is running          Context Switch          Process B runs
┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│ CPU Registers:    │     │                  │     │ CPU Registers:    │
│   RAX = 0x42      │     │ 1. Save A's regs │     │   RAX = 0x100    │
│   RBX = 0x100     │────→│ 2. Save A's PC   │────→│   RBX = 0x200    │
│   RIP = 0x8000    │     │ 3. Load B's regs │     │   RIP = 0x5000   │
│                  │     │ 4. Load B's PC   │     │                  │
└──────────────────┘     └──────────────────┘     └──────────────────┘

Kernel saves/restores:
• General-purpose registers
• Stack pointer
• Instruction pointer (PC)
• Memory management info (page tables)
• CPU flags
```

**When does context switching occur?**

| Trigger | Example |
|---------|---------|
| **Timer interrupt** | Scheduler switches processes periodically (every few ms) |
| **I/O wait** | Process waits for disk/network — kernel switches to another |
| **System call** | Some syscalls cause the process to sleep |
| **Higher priority process** | A high-priority process becomes ready |
| **Voluntary yield** | Process calls `sched_yield()` |

Context switching has overhead. Too many switches can degrade performance.

---

## 🔔 Interrupt Handling

**Interrupts** are signals sent to the CPU by hardware or software that require immediate attention.

```
+--------------------------------------------------+
│              Interrupt Handling                    │
│                                                   │
│  Hardware Device ──────────────────────────┐      │
│  (Keyboard, NIC, Disk Timer, etc.)          │      │
│         │                                    │      │
│         ▼                                    │      │
│  ┌──────────────────────────────────────┐    │      │
│  │        Interrupt Controller           │    │      │
│  │        (APIC / PIC)                   │    │      │
│  └────────────────┬─────────────────────┘    │      │
│                   │                            │      │
│                   ▼                            │      │
│  ┌──────────────────────────────────────┐    │      │
│  │              CPU                      │    │      │
│  │                                       │    │      │
│  │  1. Current process paused            │    │      │
│  │  2. Interrupt handler (ISR) runs      │◄───┘      │
│  │  3. Handler processes the event       │            │
│  │  4. Original process resumes          │            │
│  └──────────────────────────────────────┘            │
+--------------------------------------------------+
```

**Types of Interrupts:**

| Type | Source | Example |
|------|--------|---------|
| **Hardware Interrupt** | External device | Keyboard press, network packet, disk I/O complete |
| **Software Interrupt** | Program instruction | System calls (`int 0x80`), `syscall` instruction |
| **Exception** | CPU itself | Division by zero, page fault, invalid instruction |

**Interrupt Service Routine (ISR):**

When an interrupt occurs:
1. CPU saves current process state
2. CPU looks up the interrupt handler in the **Interrupt Descriptor Table (IDT)**
3. The ISR (kernel code) handles the event
4. CPU restores the previous process state and continues

---

## 🔑 Key Kernel Files and Commands

### Kernel Version

```bash
# Check your kernel version
uname -r

# Detailed kernel information
uname -a

# Kernel build information
cat /proc/version
```

### Kernel Parameters

```bash
# View all kernel parameters
sysctl -a

# View a specific parameter
sysctl kernel.hostname

# Change a parameter temporarily
sudo sysctl -w kernel.hostname=myhost

# Make changes permanent (edit /etc/sysctl.conf)
```

### Kernel Messages (dmesg)

```bash
# View kernel ring buffer (boot messages, driver info, errors)
dmesg

# View last 20 kernel messages
dmesg | tail -20

# View only error messages
dmesg --level=err

# Real-time kernel messages
dmesg -w
```

### Kernel Configuration

```bash
# View kernel config (if available)
cat /boot/config-$(uname -r)

# Check if a specific feature is enabled
zcat /proc/config.gz | grep CONFIG_EXT4
```

---

## ✅ Summary

| Subsystem | Responsibility |
|-----------|---------------|
| **Process Management** | Creating, scheduling, and terminating processes (CFS scheduler) |
| **Memory Management** | Virtual memory, paging, swap, allocation |
| **Device Drivers** | Hardware abstraction — keyboards, disks, networks, GPUs |
| **Filesystem Management** | VFS layer supporting ext4, XFS, Btrfs, and more |
| **System Call Interface** | Gateway between user space and kernel space |
| **Context Switching** | Saving/restoring process state for multitasking |
| **Interrupt Handling** | Responding to hardware and software events |

**Kernel Architecture:**

- **Monolithic** — all core services run in kernel space
- **Modular** — loadable kernel modules provide flexibility
- **Preemptible** — kernel can be interrupted and rescheduled
- **Multi-user** — supports multiple users with isolation

---

## 📚 What's Next?

Continue your Linux learning journey:

1. **[Filesystem Hierarchy](filesystem-hierarchy.md)** — Understand the Linux directory structure
2. **[Key Concepts](key-concepts.md)** — Permissions, everything-is-a-file, multitasking
3. **[Basic Commands](basic-commands.md)** — Practice essential Linux commands
4. **[Architecture](architecture.md)** — Review user space vs kernel space concepts

<details>
<summary>🔍 Click for additional resources</summary>

### Recommended Further Reading

- **`man 2 syscalls`** — System call manual
- **`/proc` and `/sys` filesystems** — Runtime kernel interfaces
- **Linux Kernel Source**: [kernel.org](https://www.kernel.org/)
- **"Linux Kernel Development"** by Robert Love
- **"Understanding the Linux Kernel"** by Daniel P. Bovet and Marco Cesati
- **Kernel Newbies**: [kernelnewbies.org](https://kernelnewbies.org/)

</details>

---

> 💡 Tip  
> You don't need to memorize kernel internals to use Linux effectively. But understanding these concepts will make you a better Linux user, developer, and system administrator.
