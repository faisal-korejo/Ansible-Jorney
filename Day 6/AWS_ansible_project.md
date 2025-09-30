# AWS Ansible Project - Automating EC2 Instances with Ansible and Boto3

## 📌 Project Goal
Automate the creation of **2 Ubuntu instances** and **1 Debian instance** on AWS using **Ansible, Boto3, and IAM credentials**.

---

## 1️⃣ Prerequisites
- AWS account with IAM user (Access Key + Secret Key)
- Ansible installed on local machine
- Python3 installed
- AWS Ansible Collection installed

---

## 2️⃣ Setup Virtual Environment (to manage boto3 dependencies)
```bash
# Create virtual environment
python3 -m venv ~/myenv

# Activate environment
source ~/myenv/bin/activate

# Upgrade pip
pip install --upgrade pip

# Install boto3 and botocore
pip install boto3 botocore
```

---

## 3️⃣ Install Ansible AWS Collection
```bash
ansible-galaxy collection install amazon.aws --force
```

---

## 4️⃣ Project Structure
```
AWS_Ansible_Project/
│── ec2_create.yml        # Main playbook
│── inventory.ini         # Inventory file
│── group_vars/
│   └── all/
│       └── aws_keys.yml  # Encrypted IAM keys with Ansible Vault
│── ec2/
│   └── tasks/
│       └── main.yml      # Role tasks to launch EC2
```

---

## 5️⃣ Secure AWS IAM Keys using Ansible Vault
```bash
# Generate vault password file
openssl rand --base64 2048 > vault.pass

# Create group_vars directory
mkdir -p group_vars/all

# Store IAM credentials securely
ansible-vault create group_vars/all/aws_keys.yml --vault-password-file vault.pass
```

Inside `aws_keys.yml` (encrypted):
```yaml
ec2_access_key: YOUR_AWS_ACCESS_KEY
ec2_secret_key: YOUR_AWS_SECRET_KEY
```

---

## 6️⃣ Create Role for EC2
```bash
ansible-galaxy role init ec2
```

---

## 7️⃣ Define EC2 Task (ec2/tasks/main.yml)
```yaml
---
- name: Launch EC2 Instances (2 Ubuntu + 1 Debian)
  amazon.aws.ec2_instance:
    name: "{{ item.name }}"
    key_name: "YOUR_KEY_PAIR"
    instance_type: t3.micro
    security_group: "default"
    region: us-east-1
    aws_access_key: "{{ ec2_access_key }}"
    aws_secret_key: "{{ ec2_secret_key }}"
    network:
      assign_public_ip: true
    image_id: "{{ item.image }}"
  loop:
    - { image: "ami-0b0012dad04fbe3d7", name: "ubuntu-instance-1" }  # Ubuntu
    - { image: "ami-0b0012dad04fbe3d7", name: "ubuntu-instance-2" }  # Ubuntu
    - { image: "ami-0e86e20dae9224db8", name: "debian-instance" }    # Debian
```

> ⚠️ Replace AMI IDs with the latest Ubuntu & Debian AMIs for your region.

---

## 8️⃣ Main Playbook (ec2_create.yml)
```yaml
---
- hosts: localhost
  connection: local
  vars:
    ansible_python_interpreter: ~/myenv/bin/python
  roles:
    - ec2
```

---

## 9️⃣ Run Playbook
```bash
ansible-playbook -i inventory.ini ec2_create.yml --vault-password-file vault.pass
```

---

## 🔑 Key Learnings
- Automated AWS EC2 provisioning with Ansible
- Managed IAM credentials securely using Ansible Vault
- Created **2 Ubuntu** and **1 Debian** instances with Boto3
- Used **virtual environment** to manage dependencies cleanly
- Practiced **Infrastructure as Code (IaC)** with reusable Ansible roles

---

## ✅ Outcome
This project successfully deployed multiple EC2 instances (Ubuntu + Debian) in AWS with full automation, security, and idempotency.

---

## 📌 Tags
#Ansible #AWS #Boto3 #IAM #Automation #DevOps #InfrastructureAsCode #Ubuntu #Debian
