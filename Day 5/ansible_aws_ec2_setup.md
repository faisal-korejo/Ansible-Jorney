
# 🚀 Ansible with AWS EC2 Setup Guide

This guide explains how to configure **Ansible** to manage **AWS EC2 instances** using authentication.

---

## 🔹 1. Prerequisites
- Ansible installed (`ansible --version` to check)
- AWS account with access keys
- `boto3` and `botocore` installed (Python AWS SDK)
- `amazon.aws` Ansible collection installed

```bash
pip install boto3 botocore
ansible-galaxy collection install amazon.aws
```

---

## 🔹 2. Configure AWS Credentials

Ansible uses AWS credentials to authenticate. You can configure them in several ways:

### Option 1: AWS CLI
```bash
aws configure
```
This will save credentials in `~/.aws/credentials`:
```
[default]
aws_access_key_id = YOUR_ACCESS_KEY
aws_secret_access_key = YOUR_SECRET_KEY
region = us-east-1
```

### Option 2: Environment Variables
```bash
export AWS_ACCESS_KEY_ID=YOUR_ACCESS_KEY
export AWS_SECRET_ACCESS_KEY=YOUR_SECRET_KEY
export AWS_DEFAULT_REGION=us-east-1
```

### Option 3: Ansible Vault (Best for security)
Encrypt your keys using Vault:
```bash
ansible-vault create aws_credentials.yml
```
Inside file:
```yaml
aws_access_key: YOUR_ACCESS_KEY
aws_secret_key: YOUR_SECRET_KEY
region: us-east-1
```
Then reference it in playbooks.

---

## 🔹 3. Create an EC2 Instance with Ansible

Example playbook (`ec2_setup.yml`):

```yaml
- name: Launch EC2 instance
  hosts: localhost
  connection: local
  gather_facts: false
  collections:
    - amazon.aws

  tasks:
    - name: Launch a new EC2 instance
      ec2_instance:
        key_name: my-keypair
        instance_type: t2.micro
        image_id: ami-0c55b159cbfafe1f0   # Example Amazon Linux AMI
        region: us-east-1
        wait: yes
        count: 1
        tags:
          Name: Ansible-EC2
      register: ec2

    - name: Show instance info
      debug:
        var: ec2
```

Run it:
```bash
ansible-playbook ec2_setup.yml
```

---

## 🔹 4. Dynamic Inventory for EC2

Instead of hardcoding IPs, use **dynamic inventory** from AWS:

1. Install plugin (already included in `amazon.aws` collection).
2. Create file `aws_ec2.yml`:
```yaml
plugin: amazon.aws.aws_ec2
regions:
  - us-east-1
keyed_groups:
  - key: tags.Name
    prefix: tag
hostnames:
  - public-ip-address
compose:
  ansible_host: public-ip-address
```

3. Run:
```bash
ansible-inventory -i aws_ec2.yml --list
```

Now you can use `-i aws_ec2.yml` in playbooks and Ansible will fetch EC2 IPs automatically.

---

## 🔹 5. SSH Authentication to EC2

Make sure you have an AWS **key pair** (`.pem` file).

```bash
chmod 400 my-keypair.pem
ssh -i my-keypair.pem ec2-user@<PUBLIC_IP>
```

In `ansible.cfg`, configure private key:
```ini
[defaults]
inventory = aws_ec2.yml
remote_user = ec2-user
private_key_file = ~/my-keypair.pem
host_key_checking = False
```

---

## ✅ Summary
- Install `amazon.aws` collection and `boto3`
- Configure AWS credentials (`~/.aws/credentials`, env vars, or Ansible Vault)
- Use `ec2_instance` module to launch EC2
- Use **dynamic inventory** for automatic instance discovery
- Configure Ansible with SSH keys to manage EC2 directly

---
