
# Ansible Project

## 📌 Overview
This repository contains Ansible playbooks and roles for automating server configuration and application deployment.

## 📂 Project Structure
```
.
├── ansible.cfg        # Ansible configuration file
├── inventory/         # Inventory files (hosts definitions)
├── playbooks/         # Main playbooks
├── roles/             # Custom roles
│   └── example-role/  # Example role structure
├── group_vars/        # Group-specific variables
├── host_vars/         # Host-specific variables
├── .gitignore         # Ignored files for git
└── README.md          # Project documentation
```

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/yourusername/your-ansible-project.git
cd your-ansible-project
```

### 2. Define your inventory
Edit the `inventory/hosts.ini` (or YAML) file to define your target servers.

Example (`inventory/hosts.ini`):
```ini
[webservers]
192.168.1.10
192.168.1.11

[dbservers]
192.168.1.20
```

### 3. Run a playbook
```bash
ansible-playbook -i inventory/hosts.ini playbooks/site.yml
```

## 🛠 Using Roles
Roles help organize playbooks into reusable units.

Example usage in a playbook (`playbooks/site.yml`):
```yaml
- hosts: webservers
  become: true
  roles:
    - role: example-role
```

## 🔐 Ansible Vault
If you have sensitive variables (passwords, keys), use **Ansible Vault**:
```bash
ansible-vault create secrets.yml
ansible-vault edit secrets.yml
ansible-playbook -i inventory/hosts.ini playbooks/site.yml --ask-vault-pass
```

## ✅ Best Practices
- Keep **secrets encrypted** with Vault.
- Use **roles** for modular and reusable code.
- Store **environment-specific vars** in `group_vars` and `host_vars`.
- Ignore sensitive files with `.gitignore`.

## 🤝 Contributing
1. Fork the repo
2. Create a feature branch (`git checkout -b feature/my-feature`)
3. Commit your changes (`git commit -m "Add my feature"`)
4. Push to the branch (`git push origin feature/my-feature`)
5. Create a Pull Request

## 📜 License
This project is licensed under the MIT License.
