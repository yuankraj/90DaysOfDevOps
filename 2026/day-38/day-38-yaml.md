# Day 38 – YAML Basics

## Task Summary
Today I learned the basics of **YAML syntax**, which is widely used in DevOps tools like:

- Kubernetes
- GitHub Actions
- GitLab CI/CD
- Docker Compose
- Ansible

YAML stands for **YAML Ain't Markup Language** and is designed to be human-readable.

---

# Task 1 – Key Value Pairs

Example YAML structure:

```yaml
name: Vishal
role: DevOps Engineer
experience_years: 0
learning: true
```

Key observations:

- YAML uses **key: value** format
- Booleans use `true` or `false`
- YAML uses **spaces instead of tabs**

---

# Task 2 – Lists

### Block List Format

```yaml
tools:
  - docker
  - kubernetes
  - git
  - github
  - linux
```

### Inline List Format

```yaml
hobbies: [coding, reading, gaming]
```

### Two Ways to Write Lists

1. **Block list**

```
- item1
- item2
```

2. **Inline list**

```
[item1, item2]
```

---

# Task 3 – Nested Objects

Example nested YAML structure:

```yaml
server:
  name: app-server
  ip: 192.168.1.10
  port: 8080
```

Nested database configuration:

```yaml
database:
  host: db-server
  name: devopsdb
  credentials:
    user: admin
    password: securepassword
```

### Important Rule

Indentation must be consistent.

Standard practice:

```
2 spaces
```

Tabs will break YAML parsing.

---

# Task 4 – Multi-line Strings

### Block Style (`|`)

```yaml
startup_script: |
  echo "Start server"
  systemctl start nginx
```

Behavior:

- Preserves line breaks
- Useful for scripts or commands

---

### Folded Style (`>`)

```yaml
startup_script: >
  echo "Start server"
  systemctl start nginx
```

Behavior:

- Converts multiple lines into a **single line**

---

### When to Use Each

| Style | Use Case |
|------|-----------|
| `|` | Scripts, commands, logs |
| `>` | Long text paragraphs |

---

# Task 5 – YAML Validation

Installed yamllint:

```
sudo apt install yamllint
```

Validate file:

```
yamllint person.yaml
yamllint server.yaml
```

Intentional error example:

Using **tab indentation** causes:

```
syntax error: found character '\t'
```

Fix:

Replace tabs with spaces.

---

# Task 6 – Spot the Difference

Correct YAML:

```yaml
name: devops
tools:
  - docker
  - kubernetes
```

Broken YAML:

```yaml
name: devops
tools:
- docker
  - kubernetes
```

### Problem

Indentation is inconsistent.

Correct format requires:

```
tools:
  - item
```

Not:

```
tools:
- item
```

---

# Key YAML Rules

1. YAML uses **spaces only (no tabs)**
2. Indentation defines structure
3. Lists use `-`
4. Nested objects use indentation
5. YAML is very sensitive to formatting

---

# What I Learned

1. YAML is the configuration language used in many DevOps tools.
2. Correct indentation is critical in YAML files.
3. YAML supports lists, nested objects, and multi-line strings.

---

# Summary

Today I learned the fundamentals of YAML including key-value pairs, lists, nested objects, and multi-line strings. YAML is essential for writing configuration files used in CI/CD pipelines, Kubernetes manifests, and infrastructure automation tools.
