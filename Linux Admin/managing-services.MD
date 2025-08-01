# 🧯 Linux Services Management Cheatsheet (`systemctl` + `journalctl`)

Master the basics and advanced usage of managing system services using `systemctl` and `journalctl` on modern Linux systems (Systemd-based).

---

## 📌 What is systemd?

**Systemd** is the init system used by most modern Linux distributions. It manages:
- System boot process
- Services (aka daemons)
- Logging via `journald`
- Resource control for processes

---

## 🚦 Basic Service Control

| Action       | Command                             |
|--------------|--------------------------------------|
| Start        | `sudo systemctl start <service>`     |
| Stop         | `sudo systemctl stop <service>`      |
| Restart      | `sudo systemctl restart <service>`   |
| Reload       | `sudo systemctl reload <service>`    |
| Enable (on boot) | `sudo systemctl enable <service>` |
| Disable (on boot) | `sudo systemctl disable <service>` |
| Mask (disable completely) | `sudo systemctl mask <service>` |
| Unmask       | `sudo systemctl unmask <service>`    |
| Status       | `systemctl status <service>`         |

---

## 🧼 Check, View, and Monitor Services

### 🔍 View All Services

```bash
systemctl list-units --type=service
```

### 📌 Filter Active or Failed Services

```bash
systemctl --failed
systemctl list-units --state=active
```

### 🧭 View Services by Startup Target

```bash
systemctl list-dependencies multi-user.target
```

---

## 📖 Using `journalctl` for Logs

| Task                     | Command                                |
|--------------------------|----------------------------------------|
| View logs for a service  | `journalctl -u <service>`              |
| Follow logs live         | `journalctl -u <service> -f`           |
| Show recent logs         | `journalctl -u <service> -n 50`        |
| View logs since reboot   | `journalctl -b -u <service>`           |
| Filter by time           | `journalctl --since "10 min ago"`      |
| Filter by priority       | `journalctl -p err..alert`             |

---

## ⚙️ Service Unit Files

Systemd unit files are stored in:
- `/etc/systemd/system/` → Custom unit files
- `/lib/systemd/system/` → Default service files

### 📄 View Unit File

```bash
systemctl cat <service>
```

### ✏️ Edit & Reload

```bash
sudo systemctl edit <service>         # Creates an override file
sudo systemctl daemon-reexec          # Reexec systemd itself
sudo systemctl daemon-reload          # Reload unit files
```

---

## 🚀 Example Use Cases

```bash
# Start and enable Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Check SSH service status
systemctl status ssh

# Restart NGINX and view live logs
sudo systemctl restart nginx
journalctl -u nginx -f

# Mask a service so it can't be started (e.g. Bluetooth)
sudo systemctl mask bluetooth

# List all failed services on system
systemctl --failed
```

---

## 🧠 Pro Tips

- Unit files can be of other types too: `socket`, `timer`, `mount`, etc.
- Use `--user` flag for managing user services: `systemctl --user start <service>`
- Always reload with `sudo systemctl daemon-reload` after editing a unit file manually.
- Use `systemd-analyze blame` to debug boot time delays.

---

> 🔧 This guide helps you confidently manage services, debug issues, and optimize startup processes using systemd.