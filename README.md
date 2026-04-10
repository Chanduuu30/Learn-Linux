# Linux Fundamentals: GNU/Linux, Kernel, and File System Hierarchy

> A beginner-friendly yet technically comprehensive handbook to understanding Linux architecture, kernel concepts, filesystem structure, and essential commands.

---

## 📘 About This Project

This repository serves as a **mini interactive Linux handbook** designed for:

- **Beginners** who are just starting their Linux journey
- **Students** studying operating systems and system administration
- **Developers** who want to understand the platform they deploy on
- **IT Professionals** preparing for certifications like Linux+, RHCSA, or LFCS

Every document is written with clean Markdown formatting, code examples, ASCII diagrams, and interactive collapsible sections to make learning engaging and effective.

---

## 📂 Documentation Structure

| Document | Description |
|----------|-------------|
| [Introduction](docs/introduction.md) | What is Linux? GNU/Linux explained. The GNU Project and open-source philosophy. |
| [Architecture](docs/architecture.md) | GNU/Linux system architecture: user space, kernel space, libraries, and hardware interaction. |
| [Linux Kernel](docs/linux-kernel.md) | Kernel internals: process management, memory management, device drivers, filesystems, and system calls. |
| [Filesystem Hierarchy](docs/filesystem-hierarchy.md) | The Linux Filesystem Hierarchy Standard (FHS) — understanding `/`, `/etc`, `/var`, `/usr`, and more. |
| [Key Concepts](docs/key-concepts.md) | Everything is a file, permissions, multi-user design, multitasking, and modular architecture. |
| [Basic Commands](docs/basic-commands.md) | Essential Linux commands every beginner should know, with examples and exercises. |

---

## 🚀 Quick Start

1. Start with **[Introduction](docs/introduction.md)** to understand what Linux is and why it matters.
2. Move to **[Architecture](docs/architecture.md)** to learn how a GNU/Linux system is structured.
3. Dive into the **[Linux Kernel](docs/linux-kernel.md)** to understand the core of the operating system.
4. Explore the **[Filesystem Hierarchy](docs/filesystem-hierarchy.md)** to navigate Linux with confidence.
5. Master **[Key Concepts](docs/key-concepts.md)** like permissions, multi-user design, and modularity.
6. Practice with **[Basic Commands](docs/basic-commands.md)** to build hands-on skills.

---

## 📐 Repository Layout

```
linux-fundamentals/
│
├── README.md                        ← You are here
├── docs/
│   ├── introduction.md              ← What is Linux? GNU/Linux explained
│   ├── architecture.md              ← System architecture: user space & kernel space
│   ├── linux-kernel.md              ← Kernel internals & system calls
│   ├── filesystem-hierarchy.md      ← The FHS standard: /, /etc, /var, /usr, etc.
│   ├── key-concepts.md              ← Permissions, everything-is-a-file, multitasking
│   └── basic-commands.md            ← Essential Linux commands with examples
│
├── assets/
│   └── diagrams/                    ← ASCII diagrams and future visual assets
│
└── extras/
    └── future-topics.md             ← Planned topics for future expansion
```

---

## ✨ Features

- ✅ **Beginner-friendly** — No prior Linux experience required
- ✅ **Technically accurate** — Written with engineering precision
- ✅ **ASCII diagrams** — Visual representations of architecture and concepts
- ✅ **Command examples** — Copy-paste-ready bash commands
- ✅ **Collapsible sections** — Interactive learning blocks
- ✅ **Tables & cheat sheets** — Quick reference guides
- ✅ **Tip & warning boxes** — Important callouts at a glance

---

## 📖 Example: What You'll Learn

### The GNU/Linux Architecture

```
+----------------------+
|   User Applications  |
+----------------------+
           |
           v
+----------------------+
|   System Libraries   |
|       (glibc)        |
+----------------------+
           |
           v
+----------------------+
|     GNU Utilities    |
+----------------------+
           |
           v
+----------------------+
|      Linux Kernel    |
+----------------------+
           |
           v
+----------------------+
|       Hardware       |
+----------------------+
```

### Key System Calls

| System Call | Purpose |
|-------------|---------|
| `read()` | Read data from a file descriptor |
| `write()` | Write data to a file descriptor |
| `fork()` | Create a new process |
| `exec()` | Replace current process with a new program |

---

## 🛠 Prerequisites

- A Linux terminal (any distribution — Ubuntu, Fedora, Debian, Arch, etc.)
- Basic familiarity with opening a terminal and typing commands
- Curiosity and willingness to experiment

> 💡 Tip  
> Don't have Linux installed? You can use a free virtual machine (VirtualBox, VMware) or a cloud instance to practice. Many online platforms also offer browser-based Linux terminals.

---

## 🤝 Contributing

Contributions are welcome! If you find errors, have suggestions for improvements, or want to add new sections:

1. **Fork** this repository
2. Create a **feature branch** (`git checkout -b feature/amazing-content`)
3. **Commit** your changes (`git commit -m 'Add amazing content'`)
4. **Push** to the branch (`git push origin feature/amazing-content`)
5. Open a **Pull Request**

Please follow the existing Markdown style and formatting conventions used throughout the documentation.

---

## 📜 License

This project is open-source and available under the [MIT License](LICENSE).

---

## 🙏 Acknowledgments

- The **GNU Project** (Free Software Foundation) — for the tools and philosophy
- **Linus Torvalds** — for creating the Linux kernel
- The **Linux Documentation Project** (TLDP) — for decades of community knowledge
- All open-source contributors who make Linux what it is today

---

> 💡 Tip  
> Bookmark this repository and use it as a reference guide. Come back whenever you need a refresher on Linux fundamentals.

---

*Happy learning! 🐧*
