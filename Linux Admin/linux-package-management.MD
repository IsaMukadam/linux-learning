# 📦 Linux Package Management Cheatsheet (apt, dnf, yum)

Learning how to install, remove, search, and manage software packages on popular Linux distributions.

---

## 📍 Quick Summary

| Tool  | Used by                            | Notes                                |
|-------|------------------------------------|--------------------------------------|
| `apt` | Debian, Ubuntu                     | Frontend for `dpkg`                  |
| `dnf` | Fedora, RHEL ≥ 8, CentOS ≥ 8       | Successor to `yum`                   |
| `yum` | RHEL ≤ 7, CentOS ≤ 7, Amazon Linux | Older package manager (uses RPM)     |

---

## 🛠 Common Commands

### 🔍 Search for a Package

```bash
apt search <package>
dnf search <package>
yum search <package>
```

### 📥 Install a Package

```bash
sudo apt install <package>
sudo dnf install <package>
sudo yum install <package>
```

### 🧹 Remove a Package

```bash
sudo apt remove <package>
sudo dnf remove <package>
sudo yum remove <package>
```

### 🔄 Update System Packages

```bash
sudo apt update         # Refresh package index
sudo apt upgrade        # Upgrade packages

sudo dnf upgrade
sudo yum update         # yum combines update & upgrade
```

### 🔍 View Installed Packages

```bash
apt list --installed
dnf list installed
yum list installed
```

### 🧪 Show Package Info

```bash
apt show <package>
dnf info <package>
yum info <package>
```

---

## 🧰 Extra Tools

| Task                          | `apt`                        | `dnf` / `yum`                 |
|-------------------------------|-------------------------------|-------------------------------|
| Clean cache                   | `sudo apt clean`              | `sudo dnf clean all` / `yum`  |
| Check dependencies            | `apt depends <package>`       | `dnf repoquery --requires`    |
| List files in a package       | `dpkg -L <package>`           | `rpm -ql <package>`           |
| What pkg provides a file      | `apt-file search <filename>`  | `dnf provides <file>`         |

> 💡 For `apt-file`, install it first with `sudo apt install apt-file` and run `apt-file update`.

---

## 🧠 Best Practices

- Run `sudo apt update` or `sudo dnf check-update` regularly.
- Always review dependencies before removing packages.
- Use `apt autoremove` or `dnf autoremove` to clean unused packages.
- Prefer `apt` over `apt-get` for modern Debian-based systems.

---

## 🚀 Example Use Cases

```bash
# Install NGINX
sudo apt install nginx
sudo dnf install nginx
sudo yum install nginx

# Remove Python 2
sudo apt remove python2
sudo dnf remove python2
sudo yum remove python2

# Clean up
sudo apt autoremove && sudo apt clean
sudo dnf autoremove && sudo dnf clean all
```

---

> 📚 This cheatsheet helps you master the core tools used in system package management across major Linux distributions.