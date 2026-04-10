# Basic Linux Commands

> This document provides a hands-on introduction to essential Linux commands. Every command includes descriptions, examples, and exercises to help you build practical skills.

---

## 🖥️ Getting Started: The Terminal

The **terminal** (also called shell, console, or command line) is your primary interface for interacting with Linux.

```bash
# Open a terminal
# On desktop Linux: Ctrl+Alt+T (usually)
# On a server: You're already in one after SSH login
```

**Common shells:**

| Shell | Description |
|-------|-------------|
| `bash` | Bourne Again SHell — default on most distros |
| `zsh` | Z Shell — popular alternative, default on macOS |
| `fish` | Friendly Interactive SHell — user-friendly, colorful |
| `sh` | Bourne Shell — minimal, POSIX-compliant |

```bash
# Check your current shell
echo $SHELL

# List available shells
cat /etc/shells
```

---

## 📋 Command Reference Table

| Command | Description |
|---------|-------------|
| `pwd` | Print working directory |
| `ls` | List directory contents |
| `cd` | Change directory |
| `cp` | Copy files or directories |
| `mv` | Move or rename files or directories |
| `rm` | Remove files or directories |
| `mkdir` | Create a new directory |
| `rmdir` | Remove an empty directory |
| `touch` | Create an empty file or update timestamp |
| `cat` | Display file contents |
| `less` | View file contents page by page |
| `head` | Display the first lines of a file |
| `tail` | Display the last lines of a file |
| `echo` | Print text to the terminal |
| `man` | Display the manual page for a command |
| `whoami` | Display the current username |
| `uname` | Display system information |
| `date` | Display or set the date and time |
| `wc` | Count lines, words, and characters |
| `grep` | Search for patterns in text |
| `find` | Search for files and directories |
| `chmod` | Change file permissions |
| `chown` | Change file owner and group |
| `sudo` | Execute a command as another user (usually root) |
| `ps` | Report running processes |
| `kill` | Send a signal to a process |
| `df` | Report filesystem disk space usage |
| `du` | Estimate file and directory space usage |
| `free` | Display memory usage |
| `top` / `htop` | Display and manage processes interactively |

---

## 🔧 Command Examples

### 1. `pwd` — Print Working Directory

Displays the full path of your current location in the filesystem.

```bash
$ pwd
/home/alice/projects
```

---

### 2. `ls` — List Directory Contents

Lists files and directories in the current (or specified) location.

```bash
# List current directory
ls

# Long format (permissions, owner, size, date)
ls -l

# Show hidden files (dotfiles)
ls -a

# Long format + hidden files
ls -la

# Human-readable sizes
ls -lh

# Combine multiple flags
ls -lah

# List a specific directory
ls /etc/
ls -la /var/log/

# Sort by modification time (newest first)
ls -lt

# Sort by size (largest first)
ls -lS
```

**Understanding `ls -la` output:**

```bash
$ ls -la
total 48
drwxr-xr-x  5 alice developers 4096 Apr 10 12:00 .
drwxr-xr-x 18 alice developers 4096 Apr  9 08:30 ..
-rw-r--r--  1 alice developers  220 Apr  9 08:30 .bashrc
-rwxr-xr-x  1 alice developers  102 Apr 10 12:00 script.sh
drwxr-xr-x  2 alice developers 4096 Apr 10 11:00 documents
-rw-r--r--  1 alice developers 1024 Apr 10 10:00 notes.txt
││││││││││  │    │        │       │     │           │
││││││││││  │    │        │       │     │           └── File name
││││││││││  │    │        │       │     └────────────── Modification date
││││││││││  │    │        │       └──────────────────── File size (bytes)
││││││││││  │    │        └──────────────────────────── Group
││││││││││  │    └───────────────────────────────────── Owner
││││││││││  └────────────────────────────────────────── Hard link count
│││││││││└───────────────────────────────────────────── Others: r-- (read)
│││││││└─────────────────────────────────────────────── Group:  r-x (read+execute)
││││││└──────────────────────────────────────────────── Owner:  rwx (read+write+execute)
│││││└───────────────────────────────────────────────── Special bits
││││└────────────────────────────────────────────────── File type: d=dir, -=file, l=link
```

---

### 3. `cd` — Change Directory

Navigates to a different directory.

```bash
# Go to home directory
cd ~
cd

# Go to root of filesystem
cd /

# Go to /etc directory
cd /etc

# Go up one level
cd ..

# Go up two levels
cd ../..

# Go to previous directory (like "back" button)
cd -

# Use relative paths
cd documents/
cd ../projects/

# Use absolute paths
cd /home/alice/documents
```

> 💡 Tip  
> `.` means "current directory" and `..` means "parent directory."

---

### 4. `cp` — Copy Files and Directories

Copies files or directories from one location to another.

```bash
# Copy a file
cp file.txt backup.txt

# Copy to a directory
cp file.txt /tmp/

# Copy with full path
cp /home/alice/file.txt /home/alice/backup/file.txt

# Copy a directory (recursive)
cp -r documents/ documents_backup/

# Copy and preserve attributes (timestamps, permissions)
cp -a file.txt backup.txt

# Interactive (ask before overwriting)
cp -i file.txt /tmp/

# Copy multiple files
cp file1.txt file2.txt file3.txt /tmp/

# Copy all .txt files
cp *.txt /tmp/
```

---

### 5. `mv` — Move or Rename Files

Moves files to a new location or renames them.

```bash
# Rename a file (move within same directory)
mv oldname.txt newname.txt

# Move a file to another directory
mv file.txt /tmp/

# Move and rename at the same time
mv file.txt /tmp/renamed.txt

# Move a directory
mv old_dir/ new_location/

# Interactive (ask before overwriting)
mv -i file.txt /tmp/

# Move multiple files
mv file1.txt file2.txt file3.txt /tmp/
```

> ⚠️ Important  
> `mv` does not create a copy — the original is removed from its source location.

---

### 6. `rm` — Remove Files and Directories

Deletes files or directories. **This action is irreversible!**

```bash
# Remove a file
rm file.txt

# Remove multiple files
rm file1.txt file2.txt

# Remove with confirmation
rm -i file.txt

# Remove a directory and its contents (recursive)
rm -r directory/

# Force removal (no prompts, ignore nonexistent files)
rm -rf directory/

# Remove all .log files
rm *.log

# Remove all files starting with 'temp'
rm temp*
```

> ⚠️ Important  
> **NEVER** run `rm -rf /` or `rm -rf /*`. This will attempt to delete your entire filesystem. There is no "Recycle Bin" in the terminal — deleted files are gone.

<details>
<summary>🔍 Click to learn about safer deletion practices</summary>

### Safer Altern to `rm`

Instead of permanently deleting files, consider:

```bash
# Move to a trash directory instead
mkdir -p ~/.trash
mv unwanted_file.txt ~/.trash/

# Empty trash when you're sure
rm -rf ~/.trash/*
```

Or use the `trash-cli` tool:

```bash
sudo apt install trash-cli
trash-put file.txt        # Move to trash
trash-list                # List trashed files
trash-restore             # Restore a file
trash-empty               # Empty trash
```

</details>

---

### 7. `mkdir` — Create Directories

Creates new directories.

```bash
# Create a single directory
mkdir new_folder

# Create multiple directories
mkdir folder1 folder2 folder3

# Create nested directories (create parents as needed)
mkdir -p projects/2024/april/reports

# Set permissions while creating
mkdir -m 700 private_folder
```

---

### 8. `touch` — Create Empty Files

Creates an empty file or updates the timestamp of an existing file.

```bash
# Create an empty file
touch newfile.txt

# Create multiple files
touch file1.txt file2.txt file3.txt

# Update timestamp of existing file
touch existing_file.txt

# Create a file in a specific location
touch /tmp/testfile.txt
```

---

### 9. `cat` — Display File Contents

Concatenates and prints file contents to the terminal.

```bash
# Display a file
cat /etc/hostname

# Display multiple files
cat file1.txt file2.txt

# Display with line numbers
cat -n file.txt

# Create a file by typing (Ctrl+D to save)
cat > newfile.txt
Hello World
Ctrl+D

# Append to a file
cat >> existing.txt
Additional text
Ctrl+D
```

---

### 10. `less` — View File Page by Page

Like `cat`, but lets you scroll through large files.

```bash
# View a file with scrolling
less /var/log/syslog

# Navigate in less:
#   Space/Page Down  — Next page
#   b/Page Up        — Previous page
#   Arrow keys       — Line by line
#   /pattern         — Search forward
#   n                — Next search result
#   q                — Quit

# View with line numbers
less -N /var/log/syslog
```

---

### 11. `head` and `tail` — First and Last Lines

```bash
# First 10 lines of a file
head file.txt

# First 20 lines
head -n 20 file.txt

# Last 10 lines
tail file.txt

# Last 20 lines
tail -n 20 file.txt

# Follow a file in real time (great for logs)
tail -f /var/log/syslog

# Follow multiple log files
tail -f /var/log/syslog /var/log/auth.log
```

> 💡 Tip  
> `tail -f` is essential for monitoring log files in real time. Press `Ctrl+C` to stop.

---

### 12. `echo` — Print Text

Prints text to the terminal or writes to a file.

```bash
# Print text
echo "Hello, World!"

# Print a variable
echo $HOME
echo $USER
echo $PATH

# Write to a file (overwrites)
echo "Hello" > file.txt

# Append to a file
echo "World" >> file.txt

# Print with special characters
echo -e "Line 1\nLine 2\tTabbed"

# Print without newline
echo -n "No newline here"
```

---

### 13. `man` — Manual Pages

Displays the documentation for any command.

```bash
# Open manual for a command
man ls
man cp
man grep

# Navigate in man pages:
#   Space/Page Down  — Next page
#   b/Page Up        — Previous page
#   /pattern         — Search
#   q                — Quit

# Search for commands by keyword
man -k "copy files"
man -k "network"

# View manual section (e.g., section 5 for file formats)
man 5 passwd
man 2 open
```

**Manual sections:**

| Section | Content |
|---------|---------|
| 1 | User commands |
| 2 | System calls |
| 3 | Library functions |
| 4 | Special files (devices) |
| 5 | File formats and conventions |
| 6 | Games |
| 7 | Miscellaneous |
| 8 | System administration commands |

---

### 14. `whoami` and `uname` — System Information

```bash
# Current username
whoami

# System information
uname
uname -a          # All information
uname -r          # Kernel version
uname -m          # Machine architecture
uname -n          # Hostname

# Distribution info
cat /etc/os-release
lsb_release -a
```

---

### 15. `date` — Date and Time

```bash
# Current date and time
date

# Custom format
date "+%Y-%m-%d %H:%M:%S"
date "+%A, %B %d, %Y"

# UTC time
date -u
```

---

### 16. `wc` — Word, Line, and Character Count

```bash
# Count lines, words, characters
wc file.txt

# Count only lines
wc -l file.txt

# Count only words
wc -w file.txt

# Count only characters
wc -c file.txt

# Count lines of a command output
cat /etc/passwd | wc -l
```

---

### 17. `grep` — Search Text Patterns

One of the most powerful and frequently used Linux commands.

```bash
# Search for a word in a file
grep "root" /etc/passwd

# Case-insensitive search
grep -i "error" /var/log/syslog

# Show line numbers
grep -n "error" /var/log/syslog

# Invert match (show lines NOT containing pattern)
grep -v "nologin" /etc/passwd

# Count matching lines
grep -c "error" /var/log/syslog

# Recursive search in directory
grep -r "TODO" ./projects/

# Show context (lines before and after match)
grep -C 3 "error" /var/log/syslog

# Use regular expressions
grep -E "^[A-Z]" file.txt

# Search multiple patterns
grep -E "error|warning|critical" /var/log/syslog

# Show only filenames with matches
grep -rl "pattern" ./projects/
```

---

### 18. `find` — Search for Files

Locates files and directories based on various criteria.

```bash
# Find by name
find /home -name "report.txt"

# Find by name (case-insensitive)
find /home -iname "Report.txt"

# Find all .log files
find /var/log -name "*.log"

# Find directories
find / -type d -name "config"

# Find files larger than 100MB
find / -type f -size +100M

# Find files modified in the last 7 days
find /home -type f -mtime -7

# Find files modified more than 30 days ago
find /tmp -type f -mtime +30

# Find and delete (BE CAREFUL!)
find /tmp -name "*.tmp" -delete

# Find and execute a command on results
find . -name "*.txt" -exec cat {} \;

# Find files by permissions
find / -perm 777
```

---

### 19. `chmod` — Change Permissions

Modifies file and directory permissions.

```bash
# Add execute permission for owner
chmod u+x script.sh

# Remove write permission for group and others
chmod go-w file.txt

# Set permissions using octal notation
chmod 755 script.sh       # rwxr-xr-x
chmod 644 file.txt        # rw-r--r--
chmod 600 secret.key      # rw-------
chmod 700 private_dir/    # rwx------

# Recursive (directory and contents)
chmod -R 755 /var/www/html/
```

For a detailed explanation of permissions, see [Key Concepts](key-concepts.md).

---

### 20. `chown` — Change Ownership

Changes the owner and/or group of a file.

```bash
# Change owner
sudo chown bob file.txt

# Change owner and group
sudo chown alice:developers file.txt

# Change only group
sudo chgrp admins file.txt

# Recursive (directory and contents)
sudo chown -R www-data:www-data /var/www/html/
```

---

### 21. `sudo` — Execute as Root

Runs a command with superuser (root) privileges.

```bash
# Run a single command as root
sudo apt update

# Switch to root shell
sudo -i

# Run command as another user
sudo -u postgres psql

# Repeat last command with sudo
sudo !!

# View your sudo privileges
sudo -l
```

> ⚠️ Important  
> Only use `sudo` when necessary. Running everyday commands with root privileges is dangerous and can accidentally damage your system.

---

### 22. `ps` — Process Status

Displays information about running processes.

```bash
# Basic process list (current terminal)
ps

# All processes (detailed)
ps aux

# All processes (tree format)
ps -ef

# Search for a specific process
ps aux | grep nginx

# Show process tree
pstree

# Show top processes by CPU
ps aux --sort=-%cpu | head
```

---

### 23. `kill` — Send Signals to Processes

Sends signals (usually to terminate) to running processes.

```bash
# Gracefully terminate a process
kill 1234

# Force kill (cannot be ignored)
kill -9 1234

# Kill by name
killall nginx

# Send reload signal to nginx
kill -SIGHUP $(cat /var/run/nginx.pid)

# List all signal names
kill -l
```

---

### 24. `df` — Disk Free

Reports filesystem disk space usage.

```bash
# Disk usage (human-readable)
df -h

# Show filesystem types
df -T

# Show inodes instead of blocks
df -i
```

---

### 25. `du` — Disk Usage

Estimates file and directory space usage.

```bash
# Size of current directory
du -sh

# Size of a specific directory
du -sh /var/log/

# Top-level directories only
du -sh /*

# Top 10 largest directories
du -sh /* | sort -hr | head -10
```

---

### 26. `free` — Memory Usage

Displays system memory usage.

```bash
# Memory usage (human-readable)
free -h

# Continuous monitoring
free -h -s 2

# Total memory only
free -h | grep Mem
```

---

### 27. `top` / `htop` — Interactive Process Viewer

```bash
# Open top (built-in)
top

# Open htop (install first: sudo apt install htop)
htop

# In top/htop:
#   k    — Kill a process
#   r    — Renice a process
#   q    — Quit
#   M    — Sort by memory (htop)
#   P    — Sort by CPU (htop)
```

---

## 🎯 Practice Exercises

### Exercise 1: Navigation

```bash
# 1. Check where you are
pwd

# 2. List contents
ls -la

# 3. Go to /tmp
cd /tmp

# 4. Create a directory
mkdir linux-practice

# 5. Enter it
cd linux-practice

# 6. Verify your location
pwd
```

### Exercise 2: File Operations

```bash
# 1. Create files
touch file1.txt file2.txt file3.txt

# 2. Write to a file
echo "Hello Linux!" > file1.txt

# 3. Copy a file
cp file1.txt file1-backup.txt

# 4. Rename a file
mv file3.txt renamed.txt

# 5. List to verify
ls -la

# 6. View content
cat file1.txt

# 7. Clean up
cd ..
rm -rf linux-practice
```

### Exercise 3: Search and Filter

```bash
# 1. Search for your username in /etc/passwd
grep $(whoami) /etc/passwd

# 2. Find all .conf files in /etc
find /etc -name "*.conf" -type f | head -10

# 3. Count lines in /etc/passwd
wc -l /etc/passwd

# 4. View the last 5 lines of /etc/passwd
tail -n 5 /etc/passwd
```

### Exercise 4: Permissions

```bash
# 1. Create a script
echo '#!/bin/bash' > myscript.sh
echo 'echo "Hello from my script!"' >> myscript.sh

# 2. Check current permissions
ls -l myscript.sh

# 3. Add execute permission
chmod +x myscript.sh

# 4. Run the script
./myscript.sh

# 5. Clean up
rm myscript.sh
```

---

## ⌨️ Essential Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Cancel/kill current command |
| `Ctrl+D` | Exit shell / End of input |
| `Ctrl+L` | Clear screen |
| `Ctrl+A` | Move to beginning of line |
| `Ctrl+E` | Move to end of line |
| `Ctrl+U` | Delete from cursor to beginning |
| `Ctrl+K` | Delete from cursor to end |
| `Ctrl+R` | Search command history |
| `Ctrl+Z` | Suspend current process |
| `Tab` | Auto-complete command/filename |
| `Tab+Tab` | List all possible completions |
| `Up/Down` | Browse command history |
| `!!` | Repeat last command |
| `!$` | Last argument of previous command |
| `Clear` | Clear the terminal screen |

---

## 🔗 Pipes and Redirection

### Pipes (`|`)

Connect the output of one command as input to another.

```bash
# Count number of files in /etc
ls /etc | wc -l

# Find and filter
ps aux | grep nginx

# Sort and show top 5
du -sh /* | sort -hr | head -5

# Chain multiple commands
cat /etc/passwd | grep bash | cut -d: -f1 | sort
```

### Redirection

| Operator | Description |
|----------|-------------|
| `>` | Redirect output (overwrite) |
| `>>` | Redirect output (append) |
| `<` | Redirect input |
| `2>` | Redirect stderr |
| `&>` | Redirect stdout and stderr |
| `2>&1` | Redirect stderr to stdout |

```bash
# Save output to file (overwrite)
ls -la > files.txt

# Append to file
echo "new line" >> files.txt

# Discard output
command > /dev/null

# Save errors only
command 2> errors.log

# Save both output and errors
command &> output.log

# Use file as input
sort < unsorted.txt
```

---

## ✅ Summary: Most Common Commands Cheat Sheet

```
┌─────────────────────────────────────────────────────────┐
│              Essential Linux Commands                     │
├──────────────────┬──────────────────────────────────────┤
│  Navigation      │  pwd, cd, ls, tree                   │
│  Files           │  touch, cat, less, head, tail, cp,   │
│                  │  mv, rm, mkdir, rmdir                │
│  Search          │  grep, find, locate, which, whereis  │
│  Permissions     │  chmod, chown, chgrp, umask          │
│  System Info     │  uname, whoami, date, uptime, df,    │
│                  │  du, free, top, htop                 │
│  Processes       │  ps, kill, killall, jobs, fg, bg     │
│  Help            │  man, --help, info, whatis           │
│  Network         │  ping, curl, wget, ssh, ip, ss       │
│  Archives        │  tar, gzip, zip, unzip               │
│  Text Editing    │  nano, vim, sed, awk                 │
│  Package Mgmt    │  apt, dnf, pacman, snap, flatpak     │
└──────────────────┴──────────────────────────────────────┘
```

---

## 📚 What's Next?

You now know the essential Linux commands! Continue building your skills:

1. **[Introduction](introduction.md)** — Learn what Linux is and its history
2. **[Architecture](architecture.md)** — Understand user space vs kernel space
3. **[Linux Kernel](linux-kernel.md)** — Explore kernel internals
4. **[Filesystem Hierarchy](filesystem-hierarchy.md)** — Master the directory structure
5. **[Key Concepts](key-concepts.md)** — Permissions, multi-user design, modularity

<details>
<summary>🔍 Click for additional resources</summary>

### Recommended Practice Resources

- **OverTheWire Bandit** — Gamified Linux command-line challenges
- **Linux Journey** — Free interactive Linux tutorials
- **`tldr`** — Simplified, community-driven man pages (`sudo apt install tldr`)
- **`cheat`** — Command-line cheat sheets (`sudo apt install cheat`)
- **"The Linux Command Line"** by William Shotts — Free book available online

</details>

---

> 💡 Tip  
> The best way to learn Linux is by **using it every day**. Replace your daily GUI tasks with terminal commands. The more you use these commands, the more natural they'll become.
