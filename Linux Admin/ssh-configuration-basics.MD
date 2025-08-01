# 🔐 SSH Configuration Basics Cheatsheet

Learn the essentials of setting up and managing SSH connections securely and efficiently.

---

## ⚙️ SSH Client Basics

### Connect to a Remote Server

    ssh user@hostname_or_ip

### Use a Specific Port

    ssh -p 2222 user@hostname

### Enable Verbose Mode (debugging)

    ssh -v user@hostname

### Copy Public Key to Server (for passwordless login)

    ssh-copy-id user@hostname

---

## 🗂 SSH Configuration File (`~/.ssh/config`)

Customize SSH client settings per host.

### Example Config

    Host myserver
        HostName 192.168.1.100
        User isa
        Port 2222
        IdentityFile ~/.ssh/id_rsa_myserver
        ForwardAgent yes

### Key Options

| Option         | Description                                      |
|----------------|------------------------------------------------|
| `Host`         | Alias to use with `ssh <alias>`                 |
| `HostName`     | Actual hostname or IP address                    |
| `User`         | Default username for this host                   |
| `Port`         | SSH port                                         |
| `IdentityFile` | Private key file to use                           |
| `ForwardAgent` | Enables SSH agent forwarding                      |

---

## 🔑 SSH Key Management

### Generate a New Key Pair

    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

### Copy Public Key to Remote Server

    ssh-copy-id user@hostname

### Manually Copy Public Key

    cat ~/.ssh/id_rsa.pub | ssh user@hostname "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

---

## 🛡️ SSH Daemon Configuration (`/etc/ssh/sshd_config`)

### Common Settings to Secure SSH Server

| Setting           | Description                                  | Example                  |
|-------------------|----------------------------------------------|--------------------------|
| `Port`            | Change default SSH port                       | `Port 2222`              |
| `PermitRootLogin` | Allow or deny root login                       | `PermitRootLogin no`     |
| `PasswordAuthentication` | Enable/disable password login              | `PasswordAuthentication no` |
| `PubkeyAuthentication`  | Enable public key authentication             | `PubkeyAuthentication yes`|
| `AllowUsers`       | Restrict login to specific users               | `AllowUsers isa jim`     |

### Restart SSH Service After Changes

    sudo systemctl restart sshd

---

## 🔄 Useful SSH Commands

| Task                         | Command                                     |
|------------------------------|---------------------------------------------|
| Check SSH version             | `ssh -V`                                    |
| Test SSH connection           | `ssh -vvv user@hostname`                     |
| List authorized keys          | `cat ~/.ssh/authorized_keys`                 |
| Generate new SSH key pair     | `ssh-keygen -t rsa -b 4096`                  |
| Copy key to server            | `ssh-copy-id user@hostname`                   |

---

> 💡 **Tips:**

- Always keep your private keys secure and never share them.
- Use SSH agent forwarding only when necessary.
- Disable root login and password authentication on production servers.
- Regularly update your SSH server software to patch vulnerabilities.
