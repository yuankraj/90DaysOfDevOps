# Day 38 – YAML Basics

# Objective

Today I learned the fundamentals of YAML, which is widely used in:
- CI/CD pipelines
- Docker Compose
- Kubernetes manifests
- GitHub Actions
- Ansible playbooks

I practiced writing YAML files manually, validating them, and understanding indentation rules.

---

# Task 1 – Key-Value Pairs

# person.yaml

```yaml
name: Vishal Singh
role: DevOps Engineer
experience_years: 1
learning: true
```

---

# Observation

- YAML uses key-value format
- Spaces are important
- No tabs allowed

---

# Task 2 – Lists

# Updated person.yaml

```yaml
name: Vishal Singh
role: DevOps Engineer
experience_years: 1
learning: true

tools:
  - Docker
  - Kubernetes
  - Git
  - Linux
  - Python

hobbies: [coding, gaming, learning]
```

---

# Two Ways to Write Lists in YAML

## Block Style
```yaml
tools:
  - Docker
  - Kubernetes
```

---

## Inline Style
```yaml
hobbies: [coding, gaming, learning]
```

---

# Task 3 – Nested Objects

# server.yaml

```yaml
server:
  name: production-server
  ip: 192.168.1.10
  port: 8080

database:
  host: localhost
  name: appdb

  credentials:
    user: postgres
    password: secret123
```

---

# Observation

Nested indentation creates parent-child relationships.

Example:
```yaml
database:
  credentials:
```

means:
- credentials belongs to database

---

# Task 4 – Multi-line Strings

# Updated server.yaml

```yaml
server:
  name: production-server
  ip: 192.168.1.10
  port: 8080

database:
  host: localhost
  name: appdb

  credentials:
    user: postgres
    password: secret123

startup_script_pipe: |
  sudo apt update
  sudo apt install nginx -y
  systemctl start nginx

startup_script_fold: >
  This is a startup script
  written using folded style.
  All lines become one line.
```

---

# Difference Between | and >

| Symbol | Behavior |
|--------|-----------|
| `|` | Preserves line breaks |
| `>` | Converts multiple lines into one |

---

# When to Use Them?

## Use `|`
When:
- Writing shell scripts
- Config files
- Multi-line commands

---

## Use `>`
When:
- Writing long paragraphs
- Descriptions
- Documentation text

---

# Task 5 – Validate YAML

# Install yamllint

## Input
```bash
sudo apt install yamllint
```

---

# Validate YAML Files

## Input
```bash
yamllint person.yaml

yamllint server.yaml
```

---

# Intentional Indentation Error

## Broken YAML

```yaml
tools:
- Docker
  - Kubernetes
```

---

# Error Message

```bash
syntax error: expected <block end>
```

---

# Fix

```yaml
tools:
  - Docker
  - Kubernetes
```

---

# Task 6 – Spot the Difference

# Correct YAML

```yaml
name: devops
tools:
  - docker
  - kubernetes
```

---

# Broken YAML

```yaml
name: devops
tools:
- docker
  - kubernetes
```

---

# What's Wrong?

The indentation is inconsistent.

Correct YAML requires proper spacing:
```yaml
tools:
  - docker
```

The broken YAML confuses the parser because list items are not aligned properly.

---

# YAML Rules I Learned

| Rule | Explanation |
|------|-------------|
| Spaces only | Never use tabs |
| Indentation matters | Usually 2 spaces |
| Lists use `-` | One item per line |
| Booleans use true/false | Without quotes |
| Quotes optional | Unless special characters exist |

---

# Commands Used

```bash
cat person.yaml
cat server.yaml
yamllint person.yaml
yamllint server.yaml
```

---

# Files Created

```bash
person.yaml
server.yaml
day-38-yaml.md
```

---

# What I Learned

1. YAML is indentation-sensitive.
2. YAML is heavily used in DevOps tools like Kubernetes and Docker Compose.
3. Tabs can break YAML parsing completely.

---

# Why YAML Matters in DevOps

YAML is used in:
- Kubernetes manifests
- Docker Compose
- GitHub Actions
- GitLab CI/CD
- Ansible
- Terraform configurations

Without YAML knowledge, modern DevOps workflows become difficult.

---

# Final Reflection

Today I practiced:
✅ YAML syntax  
✅ Key-value pairs  
✅ Lists  
✅ Nested objects  
✅ Multi-line strings  
✅ YAML validation  
✅ Indentation troubleshooting  

This gave me a strong foundation before starting CI/CD pipelines 🚀

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
