
# Ansible Roles

## What is a Role?
An **Ansible Role** is a way of automatically loading related vars, files, tasks, templates, and handlers.  
Roles help you **organize playbooks** into reusable components.

---

## Directory Structure of a Role
When you create a role (e.g., `ansible-galaxy role init myrole`), Ansible generates a standard structure:

```
myrole/
├── README.md        # Documentation about the role
├── defaults/        # Default variables (lowest priority)
│   └── main.yml
├── files/           # Static files that can be copied by this role
├── handlers/        # Handlers (e.g., restart services)
│   └── main.yml
├── meta/            # Role metadata (dependencies, author, license)
│   └── main.yml
├── tasks/           # Main list of tasks to execute
│   └── main.yml
├── templates/       # Jinja2 templates for config files
├── tests/           # Test playbooks and inventory for role testing
│   ├── inventory
│   └── test.yml
└── vars/            # Variables with higher precedence than defaults
    └── main.yml
```

---

## Role Components Explained

- **tasks/**  
  Contains the main list of tasks executed by the role. Example: install packages, configure services.

- **handlers/**  
  Contains handlers triggered by `notify`. Example: restart Apache after config change.

- **defaults/**  
  Default variables for the role. These can be overridden by playbooks or inventory.

- **vars/**  
  Variables with higher priority than defaults. Good for constants that usually should not change.

- **files/**  
  Static files that can be copied directly to managed nodes.

- **templates/**  
  Jinja2 templates that allow dynamic generation of configuration files.

- **meta/**  
  Defines role dependencies and metadata (author, license, supported platforms).

- **tests/**  
  Contains example playbooks and inventory to test the role.

---

## Using Roles in a Playbook

You can use a role in a playbook like this:

```yaml
- hosts: webservers
  become: true
  roles:
    - role: myrole
```

---

## Benefits of Roles
- Reusability → Roles can be shared across projects.
- Readability → Keeps playbooks clean and structured.
- Scalability → Easier to manage large infrastructures.
- Community support → Many prebuilt roles are available on [Ansible Galaxy](https://galaxy.ansible.com/).

---

## Summary
- Roles are a way to **organize and reuse automation code** in Ansible.  
- Each role has a standard structure (tasks, handlers, vars, templates, etc.).  
- You can easily include roles in playbooks to simplify configuration management.
