# 🧰 Basic Linux Commands & Directory Overview

## 🔧 Basic Commands

- `cd /` — Go to the root directory.
- `ls` — List contents of the current directory.
- `which {binary-name}` — Find the location of a binary.

### 📌 Key Information

> All these binaries are mapped together using the `$PATH` environment variable, allowing them to be executed from anywhere in the terminal.

---

## 🗂️ Key Linux Directories

### `/dev` — Devices

- Contains device files.
- Interfaces with hardware or drivers as if they were regular files (e.g. disk partitions, floppy drives).

### `/boot` — Boot Loader

- Contains files needed to boot the system, such as the Linux kernel.

### `/usr` — User Binaries

- Contains non-essential installed binaries.

#### `/usr/local` — Local Software

- Contains binaries and libraries compiled locally.
- Avoids conflict with software installed via package managers.

##### `/usr/local/bin`

- Stores locally compiled executable binaries.

#### `/usr/bin`

- Stores non-essential command binaries installed by the OS or package manager.

### `/opt` — Optional Software

- Contains third-party or add-on software packages.

### `/bin` — Essential User Binaries

- Core system commands required for all users and basic system operation.

### `/sbin` — System Binaries

- System-level binaries intended to be run by the **superuser (root)**.

### `/home` — User Directories

- Stores individual user data and configuration files.

Example users:
- `/home/isa`
- `/home/jim`

### `/lib` — Shared Libraries

- Contains shared code (libraries) used by binaries in `/bin` and `/sbin`.

### `/tmp` — Temporary Files

- Stores temporary files.
- Contents are typically cleared on system reboot.

### `/var` — Variable Data

- Contains files that change over time, such as:
  - Logs
  - Caches
  - Spools

### `/etc` — Editable Text Configuration

- Houses configuration files, often ending in `.conf`.
- All files are human-readable and editable for system configuration.

### `/proc` — Process Information

- A virtual filesystem.
- Created in-memory by the kernel to track system processes.
- Doesn’t exist on disk.

---

> 🧠 This overview helps demystify common Linux commands and file system structure, especially useful for beginners and DevOps professionals alike.

