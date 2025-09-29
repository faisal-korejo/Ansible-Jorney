# EC2 Ansible Connection Setup

This document explains step by step how to configure **Ansible with AWS EC2** using `boto3`, `ansible-vault`, and roles.

---

## 1. Install boto3
`boto3` is the AWS SDK for Python. Ansible uses it to interact with AWS services.

```bash
pip install boto3
```

---

## 2. Install Amazon AWS Ansible Collection
The `amazon.aws` collection provides Ansible modules to manage AWS resources.

```bash
ansible-galaxy collection install amazon.aws --force
```

---

## 3. Create Ansible Inventory File
The inventory file tells Ansible which hosts to manage. Here we use `localhost` since AWS is managed via API.

Create `inventory.ini`:
```ini
[local]
localhost ansible_connection=local
```

---

## 4. Create Role for EC2
Initialize a role structure for EC2:

```bash
ansible-galaxy role init ec2
```

This creates a folder `ec2/` with subfolders like `tasks/`, `vars/`, `handlers/`, etc.

---

## 5. Write EC2 Task File
Inside `ec2/tasks/main.yml`, add your EC2 instance requirements. Example:

```yaml
- name: Launch EC2 instance
  amazon.aws.ec2_instance:
    name: ansible-instance
    instance_type: t3.micro
    image_id: ami-04b70fa74e45c3917
    region: us-east-1
    key_name: my-keypair
    security_group: default
    aws_access_key: "{{ ec2_access_key }}"
    aws_secret_key: "{{ ec2_secret_key }}"
    network_interfaces:
      - assign_public_ip: true
    tags:
      Environment: Testing
```

---

## 6. Create Vault Password File
We use a random password file for Ansible Vault (so we don’t type the password manually).

```bash
openssl rand --base64 2048 > vault.pass
```

This generates a secure random password and saves it to `vault.pass`.

---

## 7. Store Secrets in Vault
Create an encrypted file to store AWS credentials:

```bash
ansible-vault create group_vars/all/pass.yml --vault-password-file vault.pass
```

Inside `pass.yml`, add your secrets:
```yaml
ec2_access_key: YOUR_AWS_ACCESS_KEY
ec2_secret_key: YOUR_AWS_SECRET_KEY
```

Now this file is encrypted and safe.

---

## 8. Create Playbook (requirements.yml)
Define your playbook to use the `ec2` role and specify the Python interpreter from the virtual environment:

```yaml
---
- hosts: localhost
  connection: local
  vars:
    ansible_python_interpreter: /root/myenv/bin/python
  roles:
    - ec2
```

---

## 9. Run Playbook
Finally, run the playbook with:

```bash
ansible-playbook -i inventory.ini requirements.yml --vault-password-file vault.pass
```

This will:
- Use your vault password file to decrypt secrets.
- Run the EC2 role tasks.
- Launch your instance on AWS.

---

## ✅ Summary
1. Installed dependencies (`boto3`, `amazon.aws` collection).  
2. Created inventory and role.  
3. Wrote EC2 instance requirements in `tasks/main.yml`.  
4. Secured AWS credentials with `ansible-vault`.  
5. Ran playbook to launch EC2 successfully.  

Now you can commit this documentation and your role files to GitHub 🚀
