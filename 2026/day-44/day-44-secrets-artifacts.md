# Day 44 – Secrets, Artifacts & Running Real Tests in CI

# Objective

Today I learned how real CI/CD pipelines securely manage:
- Secrets
- Artifacts
- Test execution
- Dependency caching

This was my first time building workflows that actually perform real testing and securely handle sensitive values.

---

# Task 1 – GitHub Secrets

# Secrets Added

GitHub Repository:
```bash
Settings → Secrets and Variables → Actions
```

Secrets created:
```bash
MY_SECRET_MESSAGE
DOCKER_USERNAME
DOCKER_TOKEN
```

---

# Workflow File

```yaml
name: Secrets Workflow

on:
  push:

jobs:
  secret-demo:
    runs-on: ubuntu-latest

    steps:
      - name: Check Secret
        run: |
          if [ -n "${{ secrets.MY_SECRET_MESSAGE }}" ]; then
            echo "The secret is set: true"
          else
            echo "Secret not found"
          fi
```

---

# Observation

✅ GitHub detected the secret correctly.

When trying to print:
```yaml
${{ secrets.MY_SECRET_MESSAGE }}
```

GitHub automatically masked the value:
```bash
***
```

---

# Why You Should Never Print Secrets

Printing secrets in CI logs is dangerous because:
- Logs may be publicly accessible
- Secrets can leak credentials
- Attackers can misuse tokens/passwords
- Production infrastructure could be compromised

---

# Task 2 – Use Secrets as Environment Variables

# Example Workflow

```yaml
name: Secret Env Workflow

on:
  push:

jobs:
  env-secret:
    runs-on: ubuntu-latest

    steps:
      - name: Use Secret as Environment Variable
        env:
          SECRET_MSG: ${{ secrets.MY_SECRET_MESSAGE }}

        run: |
          echo "Secret variable configured successfully"
```

---

# Observation

✅ Secret used securely without hardcoding values.

---

# Task 3 – Upload Artifacts

# Artifact Workflow

```yaml
name: Upload Artifact Workflow

on:
  push:

jobs:
  upload-artifact:
    runs-on: ubuntu-latest

    steps:
      - name: Generate Report
        run: |
          echo "CI Test Report" > report.txt
          cat report.txt

      - name: Upload Artifact
        uses: actions/upload-artifact@v4

        with:
          name: ci-report
          path: report.txt
```

---

# Observation

After workflow completion:
✅ Artifact appeared in GitHub Actions  
✅ File downloadable from Actions tab  

---

# Task 4 – Download Artifacts Between Jobs

# Artifact Sharing Workflow

```yaml
name: Artifact Sharing Workflow

on:
  push:

jobs:
  generate:
    runs-on: ubuntu-latest

    steps:
      - name: Create File
        run: echo "Artifact data from Job 1" > artifact.txt

      - name: Upload Artifact
        uses: actions/upload-artifact@v4

        with:
          name: shared-file
          path: artifact.txt

  consume:
    runs-on: ubuntu-latest
    needs: generate

    steps:
      - name: Download Artifact
        uses: actions/download-artifact@v4

        with:
          name: shared-file

      - name: Print Artifact Content
        run: cat artifact.txt
```

---

# Observation

Job 2 successfully:
✅ Downloaded artifact  
✅ Read file generated in Job 1  

---

# Why Artifacts Matter

Artifacts are useful for:
- Test reports
- Build outputs
- Deployment packages
- Logs
- Sharing files between jobs

---

# Task 5 – Run Real Tests in CI

# Python Test Workflow

# test-script.py

```python
print("CI test executed successfully")
```

---

# Workflow File

```yaml
name: Python CI Test

on:
  push:

jobs:
  test-python:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5

        with:
          python-version: "3.11"

      - name: Run Python Script
        run: python test-script.py
```

---

# Observation

✅ Pipeline turned GREEN when script succeeded.

---

# Intentional Failure Test

Modified script:

```python
raise Exception("Pipeline Failure")
```

---

# Result

❌ Pipeline turned RED.

After fixing:
✅ Pipeline became GREEN again.

---

# Why CI Testing Matters

CI testing helps:
- Catch bugs early
- Prevent broken deployments
- Improve software quality
- Automate validation

---

# Task 6 – Caching

# Cache Workflow Example

```yaml
name: Cache Demo

on:
  push:

jobs:
  cache-job:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Cache Pip Dependencies
        uses: actions/cache@v4

        with:
          path: ~/.cache/pip
          key: ${{ runner.os }}-pip-${{ hashFiles('requirements.txt') }}

      - name: Install Dependencies
        run: pip install -r requirements.txt
```

---

# Observation

Second workflow run:
✅ Faster dependency installation  
✅ Cache reused automatically  

---

# What Is Being Cached?

Cached:
- Downloaded dependency packages
- Build dependencies

Stored:
- On GitHub's cache storage infrastructure

---

# GitHub Actions Concepts Learned

| Concept | Meaning |
|---|---|
| Secrets | Secure sensitive values |
| Artifacts | Share/save files |
| Cache | Speed up workflows |
| upload-artifact | Save files |
| download-artifact | Retrieve files |
| setup-python | Configure Python runner |

---

# Commands & Features Practiced

```bash
actions/upload-artifact
actions/download-artifact
actions/cache
GitHub Secrets
environment variables
pipeline testing
```

---

# Files Created

```bash
.github/workflows/secrets.yml
.github/workflows/artifacts.yml
.github/workflows/test-python.yml
.github/workflows/cache.yml
day-44-secrets-artifacts.md
```

---

# What I Learned

1. Secrets securely store sensitive CI/CD values.
2. Artifacts allow sharing files between jobs.
3. Caching dramatically speeds up pipeline execution.

---

# Why This Matters in DevOps

These concepts are critical in:
- Secure deployments
- CI/CD automation
- Build optimization
- Enterprise pipelines
- Automated testing systems

---

# Final Reflection

Today I successfully learned:
✅ GitHub Secrets management  
✅ Secure environment variables  
✅ Artifact upload/download workflows  
✅ Running real tests in CI  
✅ Debugging failed pipelines  
✅ Dependency caching optimization  

This was one of the most practical GitHub Actions days so far 🚀

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
