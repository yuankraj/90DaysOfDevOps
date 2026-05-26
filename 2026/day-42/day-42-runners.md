# Day 42 – Runners: GitHub-Hosted & Self-Hosted

# Objective

Today I learned about GitHub Actions runners — the machines responsible for executing CI/CD workflows.

I explored:
- GitHub-hosted runners
- Self-hosted runners
- Runner labels
- Cross-platform workflows
- Running CI/CD jobs on my own machine

This helped me understand how enterprise CI/CD systems execute automation workloads.

---

# Task 1 – GitHub-Hosted Runners

# Workflow File

```yaml
name: Multi-OS Runner Workflow

on:
  push:

jobs:
  ubuntu-job:
    runs-on: ubuntu-latest

    steps:
      - run: |
          echo "OS: Ubuntu"
          hostname
          whoami

  windows-job:
    runs-on: windows-latest

    steps:
      - run: |
          echo "OS: Windows"
          hostname
          whoami

  macos-job:
    runs-on: macos-latest

    steps:
      - run: |
          echo "OS: macOS"
          hostname
          whoami
```

---

# Observation

All 3 jobs:
✅ Ran in parallel  
✅ Used different operating systems  
✅ Used GitHub-managed infrastructure  

---

# What is a GitHub-Hosted Runner?

A GitHub-hosted runner is:
- A temporary virtual machine provided by GitHub
- Automatically created for workflow execution
- Deleted after the job finishes

GitHub manages:
- Infrastructure
- Updates
- Security patches
- Pre-installed software

---

# Task 2 – Explore Pre-installed Tools

# Workflow Step

```yaml
- name: Print Tool Versions
  run: |
    docker --version
    python --version
    node --version
    git --version
```

---

# Output Observed

```bash
Docker version 27.x.x
Python 3.x.x
Node v20.x.x
git version 2.x.x
```

---

# Why Pre-installed Tools Matter

Benefits:
- Faster pipeline execution
- No manual setup required
- Standardized environments
- Less workflow complexity

This saves time in CI/CD pipelines.

---

# Task 3 – Set Up Self-Hosted Runner

# Steps Performed

GitHub Repo:
```bash
Settings → Actions → Runners → New Self-hosted Runner
```

Selected:
```bash
Linux
```

---

# Commands Executed

```bash
mkdir actions-runner && cd actions-runner

curl -o actions-runner-linux-x64.tar.gz -L https://github.com/actions/runner/releases/download/vX.X.X/actions-runner-linux-x64-X.X.X.tar.gz

tar xzf ./actions-runner-linux-x64.tar.gz

./config.sh --url https://github.com/yuankraj/github-actions-demo --token YOUR_TOKEN

./run.sh
```

---

# Observation

✅ Runner appeared online in GitHub  
✅ Status showed "Idle" with green dot  

---

# Task 4 – Use Self-Hosted Runner

# self-hosted.yml

```yaml
name: Self Hosted Runner Workflow

on:
  push:

jobs:
  self-hosted-job:
    runs-on: self-hosted

    steps:
      - name: Print Hostname
        run: hostname

      - name: Print Working Directory
        run: pwd

      - name: Create Test File
        run: |
          echo "GitHub Actions Self Hosted Runner" > testfile.txt
          ls -la
```

---

# Observation

The workflow:
✅ Ran on MY machine  
✅ Created a real file locally  
✅ Used my own hardware instead of GitHub VMs  

---

# Verification

After workflow execution:
```bash
testfile.txt
```

was visible directly on my machine.

---

# Task 5 – Labels

# Added Runner Label

```bash
my-linux-runner
```

---

# Updated Workflow

```yaml
runs-on: [self-hosted, my-linux-runner]
```

---

# Why Labels Matter

Labels help:
- Target specific runners
- Separate environments
- Use GPU-specific runners
- Use staging/production-specific machines

Especially useful when managing multiple self-hosted runners.

---

# Task 6 – GitHub-Hosted vs Self-Hosted

| Feature | GitHub-Hosted | Self-Hosted |
|---|---|---|
| Who manages it? | GitHub | User/Organization |
| Cost | Included/free tier | User pays |
| Pre-installed tools | Yes | User installs |
| Good for | Simple CI/CD | Custom workloads |
| Security concern | Shared cloud infra | User responsibility |

---

# GitHub Actions Concepts Learned

| Concept | Meaning |
|---------|----------|
| Runner | Machine executing jobs |
| GitHub-hosted | GitHub-managed VM |
| Self-hosted | User-managed runner |
| Labels | Target specific runners |
| Parallel jobs | Multiple jobs at once |

---

# Commands Practiced

```bash
hostname
whoami
docker --version
python --version
node --version
git --version
./config.sh
./run.sh
```

---

# Files Created

```bash
.github/workflows/self-hosted.yml
day-42-runners.md
```

---

# What I Learned

1. GitHub-hosted runners are temporary cloud VMs.
2. Self-hosted runners allow running CI/CD on your own machine.
3. Labels help organize and target specific runners.

---

# Why This Matters in DevOps

Runners are critical in:
- CI/CD pipelines
- Automated deployments
- Testing environments
- Enterprise infrastructure
- Custom automation systems

Self-hosted runners are heavily used when:
- Special hardware is needed
- Private infrastructure is required
- Security/compliance matters

---

# Final Reflection

Today I successfully:
✅ Ran workflows on Ubuntu, Windows & macOS  
✅ Explored GitHub-hosted runners  
✅ Configured my own self-hosted runner  
✅ Executed workflows on my own machine  
✅ Learned runner labels & targeting  

This was one of the coolest GitHub Actions days so far 🚀

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
