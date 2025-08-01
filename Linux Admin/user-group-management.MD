# 👥 Linux User & Group Management Cheatsheet

Manage users, groups, permissions, and ownership like a pro on any Linux system.

---

## 👤 User Management

### ➕ Create a New User

```bash
sudo adduser <username>       # Debian/Ubuntu (interactive)
sudo useradd -m <username>    # Universal (manual setup)
```

### 🔁 Modify an Existing User

```bash
sudo usermod -aG <group> <user>     # Add user to group
sudo usermod -l <new> <old>         # Rename user
sudo usermod -d /new/home <user>    # Change home directory
```

### ❌ Delete a User

```bash
sudo deluser <username>            # Debian/Ubuntu
sudo userdel -r <username>         # Removes user & home directory
```

### 🔒 Set or Change Password

```bash
sudo passwd <username>
```

### 🔍 View User Info

```bash
id <username>             # UID, GID, and groups
getent passwd <username>  # Full user entry
```

---

## 👪 Group Management

### ➕ Create a Group

```bash
sudo addgroup <groupname>         # Debian/Ubuntu
sudo groupadd <groupname>         # Universal
```

### ❌ Delete a Group

```bash
sudo delgroup <groupname>         # Debian/Ubuntu
sudo groupdel <groupname>         # Universal
```

### 🔄 Add/Remove User from Group

```bash
sudo usermod -aG <group> <user>    # Add to group
sudo gpasswd -d <user> <group>     # Remove from group
```

### 🔍 View Group Info

```bash
getent group <groupname>
groups <username>
```

---

## 🛡️ Permissions Overview

### 🔠 Symbols

- `r` — read
- `w` — write
- `x` — execute

### 🧾 Format

```text
-rwxr-xr--  1  owner  group  file
```

### 🔧 Change Permissions

```bash
chmod u+x script.sh       # Add execute for user
chmod 755 file            # rwx for user, rx for group and others
```

### 👤 Change Ownership

```bash
sudo chown user file
sudo chown user:group file
```

---

## 🔐 Useful Files

| File                  | Description                          |
|-----------------------|------------------------------------|
| `/etc/passwd`         | Basic user account information     |
| `/etc/shadow`         | Encrypted user passwords            |
| `/etc/group`          | Group account information           |
| `/etc/sudoers`        | Sudo privileges configuration       |

> 🛑 Use `visudo` to safely edit `/etc/sudoers`!

---

## 🚀 Example Use Cases

```bash
# Add new user 'isa' with a home directory
sudo adduser isa

# Add user 'isa' to the 'docker' group
sudo usermod -aG docker isa

# Change owner of 'project' to user:group
sudo chown isa:developers project

# Set file to read-only for others
chmod o-w project.txt

# View all groups for current user
groups
```

---

## 🧠 Pro Tips

- Use `sudo su - <user>` to switch users with full environment.
- Run `compgen -u` to list all users and `compgen -g` for groups.
- `newgrp <group>` activates new group in current shell.

---

> 🔧 This cheat sheet covers the essentials of managing Linux users, groups, and file access control — whether you're scripting or administering systems.
