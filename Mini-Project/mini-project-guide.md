
# ✅ Day 14: Final Project – Secure EC2 on Amazon Linux 2023

This guide walks you through securing an Amazon Linux 2023 EC2 instance using Ansible, installing Docker, deploying a container, setting up monitoring with Prometheus and Grafana, and pushing everything to GitHub.

---

## 🔐 Step 1: Launch EC2 Instance

1. Go to the AWS Console > EC2 > Launch Instance.
2. Choose **Amazon Linux 2023** AMI.
3. Select an instance type (e.g., `t2.micro`).
4. Configure key pair (download `.pem` file).
5. Open these ports in **Security Groups**:
   - SSH: 22
   - HTTP: 80
   - Custom TCP: 9090 (Prometheus)
   - Custom TCP: 3000 (Grafana)
6. Launch the instance.

---

## 🧰 Step 2: Connect & Update System

```bash
ssh -i your-key.pem ec2-user@<EC2_PUBLIC_IP>
sudo dnf update -y
```

---

## 🔧 Step 3: Install & Configure Ansible

Amazon Linux 2023 comes with Python 3.11.

```bash
sudo dnf install -y ansible
ansible --version
```

Create Ansible structure:

```bash
mkdir -p ~/ansible-setup/{playbooks,roles,inventory}
cd ~/ansible-setup
```

Create inventory file:

```ini
# inventory/hosts
[local]
localhost ansible_connection=local
```

Create a basic hardening playbook:

```yaml
# playbooks/harden.yml
---
- name: Basic Linux hardening
  hosts: local
  become: yes
  tasks:
    - name: Disable root SSH login
      lineinfile:
        path: /etc/ssh/sshd_config
        regexp: '^PermitRootLogin'
        line: 'PermitRootLogin no'

    - name: Disable password authentication
      lineinfile:
        path: /etc/ssh/sshd_config
        regexp: '^PasswordAuthentication'
        line: 'PasswordAuthentication no'

    - name: Restart SSHD
      service:
        name: sshd
        state: restarted
```

Run the playbook:

```bash
ansible-playbook -i inventory/hosts playbooks/harden.yml
```

---

## 🐳 Step 4: Install Docker

```bash
sudo dnf install -y docker
sudo systemctl enable --now docker
sudo usermod -aG docker ec2-user
newgrp docker
```

Test Docker:

```bash
docker run hello-world
```

---

## 📦 Step 5: Deploy a Container

Example: Nginx

```bash
docker run -d --name nginx -p 80:80 nginx
```

Verify:

```bash
curl http://localhost
```

---

## 📊 Step 6: Install Prometheus

Create config:

```bash
mkdir ~/prometheus
nano ~/prometheus/prometheus.yml
```

Content:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
```

Run Prometheus:

```bash
docker run -d   -p 9090:9090   -v ~/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml   prom/prometheus
```

Visit `http://<EC2_PUBLIC_IP>:9090`

---

## 📈 Step 7: Install Grafana

```bash
docker run -d   -p 3000:3000   --name=grafana   grafana/grafana-oss
```

Visit `http://<EC2_PUBLIC_IP>:3000`  
Login with `admin / admin`

---

## 🚀 Step 8: Push to GitHub

1. Install Git:

```bash
sudo dnf install -y git
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

2. Initialise Git:

```bash
cd ~/ansible-setup
git init
git add .
git commit -m "Initial Ansible setup"
```

3. Push to GitHub:

```bash
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git branch -M main
git push -u origin main
```

---

## 📝 README.md Example

```markdown
# Secure EC2 with Docker and Monitoring

This project provisions a secure EC2 instance using Ansible, installs Docker, deploys Nginx, Prometheus, and Grafana, and is fully version controlled via GitHub.
```

---

✅ You're done! You’ve built a secure, monitored EC2 setup with automation.
