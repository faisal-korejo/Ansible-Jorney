
# 🌍 Ansible Galaxy - Complete Guide

## 🔹 What is Ansible Galaxy?
Ansible Galaxy is the **community hub** for sharing, discovering, and reusing Ansible content.  
It provides pre-written **roles** and **collections** that automate common tasks like installing Apache, setting up MySQL, deploying Docker, or configuring WordPress.

Think of it like **GitHub for Ansible automation content**.

---

## 🔹 Key Concepts

### Roles
- A role is a structured way to organize playbooks and tasks.
- Contains tasks, handlers, variables, templates, and files for specific automation (e.g., installing Apache).

### Collections
- A **collection** is a bundle of roles, plugins, and modules packaged together.
- Useful for distributing complex automation in one package.

---

## 🔹 Why Use Ansible Galaxy?
✅ Save time by reusing community roles  
✅ Standardized & well-tested code  
✅ Easy to integrate into playbooks  
✅ Supports enterprise-level automation  

---

## 🔹 Basic Usage

### Search for roles
```bash
ansible-galaxy search apache
```

### Install a role
```bash
ansible-galaxy install geerlingguy.apache
```

### Use it in a playbook
```yaml
---
- hosts: webservers
  become: yes
  roles:
    - geerlingguy.apache
```

---

## 🔹 Real-World Example: Deploy WordPress on AWS EC2

Instead of writing hundreds of lines of YAML, you can combine roles from Galaxy.

### Step 1: Install Roles
```bash
ansible-galaxy install geerlingguy.apache
ansible-galaxy install geerlingguy.mysql
ansible-galaxy install geerlingguy.php
ansible-galaxy install geerlingguy.wordpress
```

### Step 2: Create Playbook (`site.yml`)
```yaml
---
- hosts: webservers
  become: yes

  roles:
    - geerlingguy.apache
    - geerlingguy.mysql
    - geerlingguy.php
    - geerlingguy.wordpress
```

### Step 3: Run Playbook
```bash
ansible-playbook -i inventory.ini site.yml
```

✅ Result: Apache, PHP, and MySQL are installed, and WordPress is deployed automatically at  
`http://<ec2-public-ip>/wordpress`

---

## 🔹 Common Ansible Galaxy Commands

| Command | Description |
|---------|-------------|
| `ansible-galaxy search <name>` | Search roles in Galaxy |
| `ansible-galaxy install <role>` | Install a role |
| `ansible-galaxy remove <role>` | Remove a role |
| `ansible-galaxy list` | List installed roles |
| `ansible-galaxy init <role-name>` | Create a new role skeleton |

---

## 🔹 Summary
- **Ansible Galaxy** helps automate deployments faster by reusing roles and collections.  
- It’s widely used in real-world scenarios like deploying **WordPress, Apache, Docker, Kubernetes, and CI/CD pipelines**.  
- Greatly improves productivity and ensures consistency in automation workflows.

---

# 🚀 Quick One-Liner
*"Deployed a static and dynamic website on AWS EC2 using Apache, MySQL, and PHP with Ansible Galaxy roles for automation."*
