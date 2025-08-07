# 🔧 DevOps Hands-On: Linux, Ansible & Security

Welcome to a quick collection of notes and playbooks based on the areas I’ve recently worked through — mainly Linux basics, security hardening, Ansible automation, and a small mini-project to bring it all together.

---

## ✅ What I Covered

### 🐧 Linux Admin Basics (Ubuntu / Amazon Linux)
- Got familiar with the Linux directory structure (`/etc`, `/var`, `/home`, etc.)
- Installed and managed packages using `apt` (Ubuntu) and `dnf` (Amazon Linux 2023).
- Managed system services with `systemctl`.
- Created and managed users and groups.
- Set up basic SSH configuration for remote access.

### 🔐 Linux Security Hardening
- Explored CIS benchmarks to understand what a secure baseline looks like.
- Set up firewall rules using `ufw` or `firewalld`.
- Learned the basics of AppArmor and how it enforces access control.
- Disabled root SSH login and configured key-based authentication.

### ⚙️ Ansible Basics
- Installed Ansible and ran some ad-hoc commands (`ansible -m ping`, `ansible -m shell`).
- Wrote a basic playbook to automate package installs and user creation.
- Used variables and simple conditionals.
- Learned how inventories and YAML syntax work.

---

## 🛠 Mini Project: Harden & Automate an EC2 Instance

I brought everything together in a mini-project where I:

- Launched an EC2 instance running Amazon Linux 2023.
- Used Ansible to:
  - Add a non-root admin user.
  - Disable password-based SSH access.
  - Install common packages.
  - Apply some CIS-level hardening steps.
- Installed Docker and deployed a test container.

---

## 📁 Repo Structure
