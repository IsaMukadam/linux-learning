
# Day 3: Ansible Basics

## ✅ Step 1: Install Ansible and Run Ad-Hoc Commands

### ✅ On Ubuntu/Debian:
```bash
sudo apt update
sudo apt install ansible -y
```

### ✅ On RHEL/CentOS:
```bash
sudo dnf install epel-release -y
sudo dnf install ansible -y
```

### ✅ Verify Installation:
```bash
ansible --version
```

### ✅ Run an Ad-Hoc Command:
Create a basic inventory file:
```ini
# inventory.ini
[web]
192.168.1.100 ansible_user=ubuntu
```

Run a command (e.g., ping all hosts):
```bash
ansible -i inventory.ini all -m ping
```

---

## ✅ Step 2: Write Your First Playbook

Create a YAML file:
```yaml
# playbook.yml
- name: Install Apache
  hosts: web
  become: yes
  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
```

Run the playbook:
```bash
ansible-playbook -i inventory.ini playbook.yml
```

---

## ✅ Step 3: Understand Roles and Inventory

### ✅ Inventory File Structure:
You can define groups and variables:
```ini
[web]
web1 ansible_host=192.168.1.100 ansible_user=ubuntu

[db]
db1 ansible_host=192.168.1.101 ansible_user=ubuntu
```

### ✅ Roles Structure:
Ansible roles follow a directory structure like this:
```
my-role/
├── tasks/
│   └── main.yml
├── handlers/
│   └── main.yml
├── templates/
├── files/
├── vars/
│   └── main.yml
```

To use a role:
```yaml
# playbook.yml
- hosts: web
  roles:
    - my-role
```

---

## ✅ Step 4: Use Variables and Conditionals

### ✅ Variables:
Define in playbook or inventory:
```yaml
vars:
  apache_package: apache2
```

Use in tasks:
```yaml
- name: Install web server
  apt:
    name: "{{ apache_package }}"
    state: present
```

### ✅ Conditionals:
Use `when`:
```yaml
- name: Install Apache on Ubuntu
  apt:
    name: apache2
    state: present
  when: ansible_os_family == "Debian"
```
