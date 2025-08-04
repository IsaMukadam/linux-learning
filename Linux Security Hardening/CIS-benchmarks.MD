# Explore CIS Benchmarks for Ubuntu/RHEL

# CIS Benchmarks for Ubuntu & RHEL

## 🔐 Why Use CIS Benchmarks?
- Meet compliance standards (NIST, ISO 27001, HIPAA, PCI-DSS)
- Harden servers against attacks
- Establish a secure baseline configuration

---

## ✅ Key Areas Covered

# ✅ Key CIS Hardening Steps (Ubuntu/RHEL)

---

## 1. 🔧 Initial Setup

### Set GRUB Password (Ubuntu/RHEL):
```bash
sudo grub2-setpassword
```
> For RHEL, this creates `/boot/grub2/user.cfg`.  
> For Ubuntu (GRUB 2), manually edit `/etc/grub.d/40_custom` to include:
```bash
set superusers="admin"
password_pbkdf2 admin [hashed-password]
```
Then update GRUB:
```bash
sudo update-grub
```

### Disable Unused Filesystems:
Edit `/etc/modprobe.d/CIS.conf`:
```bash
install cramfs /bin/true
install squashfs /bin/true
install udf /bin/true
```

### Secure Boot Parameters:
Edit `/etc/default/grub`:
```bash
GRUB_CMDLINE_LINUX="audit=1 ipv6.disable=1 selinux=1"
```
Then:
```bash
sudo update-grub  # Ubuntu
sudo grub2-mkconfig -o /boot/grub2/grub.cfg  # RHEL
```

---

## 2. 🗂 Filesystem Configuration

### Mount Options:
Edit `/etc/fstab`:
```bash
tmpfs /tmp tmpfs defaults,noexec,nosuid,nodev 0 0
```

### Secure Temporary Partitions:
```bash
sudo mount -o remount,noexec,nosuid,nodev /tmp
sudo mount -o remount,noexec,nosuid,nodev /var/tmp
sudo mount -o remount,noexec,nosuid,nodev /dev/shm
```

---

## 3. 🔄 Software Updates

### Ubuntu:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

### RHEL:
```bash
sudo yum update -y
sudo yum install dnf-automatic
sudo systemctl enable --now dnf-automatic.timer
```

### Remove Unneeded Packages:
```bash
sudo apt purge telnet rsh-server xinetd -y  # Ubuntu
sudo yum remove telnet rsh-server xinetd -y  # RHEL
```

---

## 4. 👥 User Accounts & Authentication

### Password Policy:
Edit `/etc/login.defs` and `/etc/pam.d/common-password` (Ubuntu):
```bash
PASS_MAX_DAYS 90
PASS_MIN_DAYS 7
PASS_WARN_AGE 7
```

Use `pam_pwquality`:
```bash
sudo apt install libpam-pwquality  # Ubuntu
sudo yum install libpwquality  # RHEL
```

Edit `/etc/security/pwquality.conf`:
```bash
minlen = 14
dcredit = -1
ucredit = -1
ocredit = -1
lcredit = -1
```

### SSH Hardening:
Edit `/etc/ssh/sshd_config`:
```bash
PermitRootLogin no
PasswordAuthentication no
PermitEmptyPasswords no
AllowUsers youruser
```
Then restart SSH:
```bash
sudo systemctl restart sshd
```

---

## 5. 🔐 Permissions & Access Control

### Audit World-Writable Files:
```bash
find / -xdev -type d \( -perm -0002 -a ! -perm -1000 \) -print
```

### Restrict `su`:
```bash
sudo groupadd sugroup
sudo usermod -a -G sugroup youruser
```

Edit `/etc/pam.d/su`:
```bash
auth required pam_wheel.so use_uid group=sugroup
```

---

## 6. 🔄 System Services

### Disable Unused Services:
```bash
sudo systemctl list-unit-files | grep enabled
sudo systemctl disable avahi-daemon
sudo systemctl disable cups
```

### Harden systemd Units:
Add:
```ini
[Service]
PrivateTmp=true
ProtectSystem=full
ProtectHome=yes
```
To service overrides:  
`/etc/systemd/system/your-service.service.d/hardening.conf`

---

## 7. 📑 Logging & Auditing

### Enable Auditd:
```bash
sudo apt install auditd -y  # Ubuntu
sudo yum install audit -y   # RHEL
sudo systemctl enable --now auditd
```

Example audit rule:
```bash
echo "-w /etc/passwd -p wa -k passwd_changes" | sudo tee -a /etc/audit/rules.d/audit.rules
```

### Configure Logging:
Ensure `rsyslog` or `journald` is enabled:
```bash
sudo systemctl enable --now rsyslog
```

---

## 8. 🌐 Networking

### Disable IPv6:
Edit `/etc/sysctl.conf`:
```bash
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
```
Then:
```bash
sudo sysctl -p
```

### Kernel Parameters:
Add to `/etc/sysctl.conf`:
```bash
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.log_martians = 1
```
Apply changes:
```bash
sudo sysctl -p
```

### Firewall (UFW for Ubuntu):
```bash
sudo apt install ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw enable
```

Or for RHEL with `firewalld`:
```bash
sudo systemctl enable --now firewalld
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload
```

---

## 9. 🛡 AppArmor / SELinux

### AppArmor (Ubuntu):
```bash
sudo apt install apparmor-utils
sudo aa-enforce /etc/apparmor.d/*
```

### SELinux (RHEL):
Ensure `/etc/selinux/config` contains:
```bash
SELINUX=enforcing
SELINUXTYPE=targeted
```
Then:
```bash
sudo setenforce 1
```

---

## 10. ⏰ Time Synchronization

### Ubuntu with `chrony`:
```bash
sudo apt install chrony
sudo systemctl enable --now chrony
```

### RHEL with `chrony`:
```bash
sudo yum install chrony
sudo systemctl enable --now chronyd
```

Check sync status:
```bash
chronyc tracking
```

---

## 📘 How to Access Official Benchmarks
1. Sign up at: [https://www.cisecurity.org/cis-benchmarks](https://www.cisecurity.org/cis-benchmarks)
2. Download PDF or Excel benchmarks for:
   - Ubuntu (e.g., 20.04, 22.04)
   - RHEL (e.g., 7, 8, 9)

---

## 🔧 Tools for Auditing and Automation

| Tool            | Description                                           |
|-----------------|-------------------------------------------------------|
| **CIS-CAT Pro** | Official CIS assessment tool (free for members)      |
| **Lynis**       | Open-source auditing tool with CIS-related checks     |
| **OpenSCAP**    | SCAP/CIS compliance scanning, especially for RHEL     |
| **Ansible**     | Automate CIS hardening with playbooks and roles       |

---

## 🧪 Example: SSH Hardening

```bash
# Disable root login
sudo sed -i 's/^PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config

# Disable password auth
sudo sed -i 's/^PasswordAuthentication.*/PasswordAuthentication no/' /etc/ssh/sshd_config

# Restart SSH
sudo systemctl restart sshd
