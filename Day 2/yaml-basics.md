# 📘 YAML Basics

YAML (**YAML Ain't Markup Language**) is a human-friendly data serialization format often used for configuration files, Kubernetes manifests, CI/CD pipelines, and more.

---

## 🔑 Key Features
- Easy to **read & write** (compared to JSON/XML)  
- Uses **indentation** for structure (no braces `{}` or brackets `[]`)  
- Supports **scalars, lists, and mappings**  
- File extension: `.yaml` or `.yml`  

---

## 📝 Syntax Rules
1. Indentation with **spaces only** (❌ no tabs).  
2. Key-value pairs use `:`  
   ```yaml
   key: value
   ```
3. Lists use `-`  
   ```yaml
   fruits:
     - apple
     - banana
     - mango
   ```
4. Nesting is done with indentation:  
   ```yaml
   person:
     name: Faisal
     age: 25
   ```

---

## 🔹 Scalars (Basic Values)
```yaml
string: "Hello YAML"
integer: 42
float: 3.14
boolean_true: true
boolean_false: false
null_value: null
```

---

## 🔹 Lists
```yaml
languages:
  - Python
  - JavaScript
  - Go
```

---

## 🔹 Dictionaries (Mappings)
```yaml
server:
  host: "localhost"
  port: 8080
```

---

## 🔹 Nested Structures
```yaml
app:
  name: recipe-generator
  version: 1.0
  maintainers:
    - name: Ali
      email: ali@example.com
    - name: Sara
      email: sara@example.com
```

---

## 🔹 Inline Notation
```yaml
colors: [red, green, blue]
person: {name: "Faisal", age: 25}
```

---

## 🔹 Comments
```yaml
# This is a comment
environment: production
```

---

## 🔹 Multi-line Strings
Preserve formatting (`|`):
```yaml
description: |
  This is a multi-line text block.
  It preserves newlines and formatting.
```

Folded style (`>`):
```yaml
short_description: >
  This is a folded
  multi-line string (becomes single line).
```

---

## 🚀 Example: Kubernetes Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: myapp
      image: nginx:latest
      ports:
        - containerPort: 80
```

---

## ✅ Summary
- Use **spaces for indentation** (never tabs).  
- Supports **scalars, lists, mappings**.  
- Allows **nested structures** and **inline syntax**.  
- Widely used in **config files** (Kubernetes, Docker, Ansible, GitHub Actions, etc.).  

---
