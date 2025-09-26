# ⚙️ Ansible Playbook Components

An **Ansible Playbook** is a YAML file that defines automation tasks such as configuration management, application deployment, or orchestration.  

---

## 🔑 Main Components of a Playbook

### 1️⃣ Hosts
Defines which target systems (inventory group or host) the playbook will run on.  
```yaml
- hosts: webservers
```

---

### 2️⃣ Remote User
Specifies which user account to use on the remote machine.  
```yaml
- hosts: webservers
  remote_user: ubuntu
```

---

### 3️⃣ Tasks
A list of actions Ansible will execute on the hosts.  
```yaml
tasks:
  - name: Install Nginx
    apt:
      name: nginx
      state: present
```

---

### 4️⃣ Variables
Custom values you can define and use inside playbooks.  
```yaml
vars:
  http_port: 80
```

Usage in task:  
```yaml
tasks:
  - name: Open HTTP port
    ufw:
      rule: allow
      port: "{{ http_port }}"
```

---

### 5️⃣ Handlers
Triggered only when a task reports a "changed" status. Commonly used for restarting services.  
```yaml
tasks:
  - name: Update configuration
    template:
      src: nginx.conf.j2
      dest: /etc/nginx/nginx.conf
    notify:
      - Restart Nginx

handlers:
  - name: Restart Nginx
    service:
      name: nginx
      state: restarted
```

---

### 6️⃣ Roles
A structured way of organizing playbooks (tasks, handlers, vars, files, templates).  
```yaml
- hosts: webservers
  roles:
    - nginx
```

---

### 7️⃣ Tags
Allow selective running of tasks.  
```yaml
tasks:
  - name: Install Git
    apt:
      name: git
      state: present
    tags: ['git', 'packages']
```

Run only tagged tasks:  
```bash
ansible-playbook site.yml --tags "git"
```

---

## 🚀 Example: Complete Playbook
```yaml
- hosts: webservers
  remote_user: ubuntu
  vars:
    http_port: 80

  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Configure firewall
      ufw:
        rule: allow
        port: "{{ http_port }}"

    - name: Update configuration
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
      notify:
        - Restart Nginx

  handlers:
    - name: Restart Nginx
      service:
        name: nginx
        state: restarted
```

---

## ✅ Summary
- **hosts** → Target systems  
- **remote_user** → User for connection  
- **tasks** → List of actions  
- **vars** → Variables for reuse  
- **handlers** → Triggered tasks on change  
- **roles** → Structured playbook organization  
- **tags** → Run selected tasks  

---
