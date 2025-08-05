
# 🛡 AppArmor Basics (Ubuntu)

AppArmor is a **Mandatory Access Control (MAC)** system that restricts what programs can do based on security profiles. It’s simpler than SELinux and tightly integrated with Ubuntu.

---

## ✅ 1. Install AppArmor

Most Ubuntu systems have AppArmor installed and enabled by default. But to be sure:
```bash
sudo apt update
sudo apt install apparmor apparmor-utils
```

---

## ✅ 2. Check AppArmor Status

```bash
sudo aa-status
```

This will show:
- Whether AppArmor is enabled
- Which profiles are loaded
- What mode each profile is in (enforce or complain)

---

## ✅ 3. Enforce All Profiles

```bash
sudo aa-enforce /etc/apparmor.d/*
```

You can also switch specific profiles:
```bash
sudo aa-enforce /etc/apparmor.d/usr.sbin.apache2
```

---

## ✅ 4. Complain Mode (for testing)

Put a profile in **complain mode** to log violations without enforcing:
```bash
sudo aa-complain /etc/apparmor.d/usr.sbin.apache2
```

---

## ✅ 5. View or Edit Profiles

Profiles are stored in:
```
/etc/apparmor.d/
```

To create or edit a profile:
```bash
sudo nano /etc/apparmor.d/usr.sbin.nginx
```

---

## ✅ 6. Generate New Profiles (Easy Mode)

Run a program in learning mode:
```bash
sudo aa-genprof nginx
```

Then follow the prompts to create a new profile based on what the service actually does.

---

## ✅ 7. Reload a Modified Profile

After editing:
```bash
sudo apparmor_parser -r /etc/apparmor.d/usr.sbin.nginx
```

---

## ✅ 8. Disable a Profile (not recommended for production)

```bash
sudo ln -s /etc/apparmor.d/usr.sbin.nginx /etc/apparmor.d/disable/
sudo apparmor_parser -R /etc/apparmor.d/usr.sbin.nginx
```
