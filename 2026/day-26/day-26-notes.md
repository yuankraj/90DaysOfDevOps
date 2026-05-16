# Day 26 – GitHub CLI: Manage GitHub from Your Terminal

# Objective

Today I learned how to manage GitHub directly from the terminal using the GitHub CLI (`gh`).

I practiced:
- Installing and authenticating GitHub CLI
- Managing repositories
- Creating and handling issues
- Managing pull requests from terminal
- Exploring GitHub Actions commands
- Using advanced `gh` features

This challenge helped me understand how DevOps engineers automate GitHub workflows directly from the command line.

---

# Task 1 – Install and Authenticate

# Install GitHub CLI

## Ubuntu/Debian

### Input
```bash
sudo apt install gh
```

---

# Verify Installation

### Input
```bash
gh --version
```

### Output
```bash
gh version 2.45.0
```

---

# Authenticate GitHub Account

### Input
```bash
gh auth login
```

### Observation
- Logged in successfully using browser authentication.

---

# Verify Logged-In Account

### Input
```bash
gh auth status
```

### Output
```bash
Logged in to github.com as yuankraj
```

---

# Authentication Methods Supported

GitHub CLI supports:
- Browser authentication
- Personal Access Token (PAT)
- SSH authentication

---

# Task 2 – Working with Repositories

# Create Repository from Terminal

### Input
```bash
gh repo create devops-gh-cli-demo --public --clone --add-readme
```

### Observation
- Repository created and cloned automatically.

---

# Clone Repository Using gh

### Input
```bash
gh repo clone cli/cli
```

### Observation
- Repository cloned successfully.

---

# View Repository Details

### Input
```bash
gh repo view
```

### Output
```bash
Repository details displayed
```

---

# List All Repositories

### Input
```bash
gh repo list
```

---

# Open Repository in Browser

### Input
```bash
gh repo view --web
```

### Observation
- Opened GitHub repository directly in browser.

---

# Delete Test Repository

### Input
```bash
gh repo delete devops-gh-cli-demo
```

### Warning
- Repository permanently deleted.

---

# Task 3 – GitHub Issues

# Create Issue

### Input
```bash
gh issue create --title "Bug in login page" --body "Fix login validation issue" --label "bug"
```

### Observation
- Issue created successfully from terminal.

---

# List Open Issues

### Input
```bash
gh issue list
```

---

# View Specific Issue

### Input
```bash
gh issue view 1
```

---

# Close Issue

### Input
```bash
gh issue close 1
```

### Observation
- Issue closed successfully.

---

# Real-World Use of gh issue

Useful for:
- Automated bug reporting
- CI/CD notifications
- Scripting issue management
- DevOps automation workflows

---

# Task 4 – Pull Requests

# Create Branch

### Input
```bash
git switch -c feature-gh-cli
```

---

# Make Change & Push

### Input
```bash
echo "GitHub CLI Practice" >> notes.txt

git add .
git commit -m "Add GitHub CLI notes"

git push -u origin feature-gh-cli
```

---

# Create Pull Request

### Input
```bash
gh pr create --fill
```

### Observation
- Pull request created entirely from terminal.

---

# List Pull Requests

### Input
```bash
gh pr list
```

---

# View PR Details

### Input
```bash
gh pr view
```

### Observation
- Shows reviewers, status, checks, and commits.

---

# Merge PR

### Input
```bash
gh pr merge
```

### Observation
- Pull request merged successfully.

---

# Merge Methods Supported

```bash
--merge
--squash
--rebase
```

---

# Reviewing Someone Else's PR

Commands:
```bash
gh pr checkout <pr-number>

gh pr review
```

---

# Task 5 – GitHub Actions Preview

# List Workflow Runs

### Input
```bash
gh run list
```

---

# View Workflow Status

### Input
```bash
gh run view
```

### Observation
- Displays workflow execution details and logs.

---

# Usefulness in CI/CD

`gh run` and `gh workflow` help:
- Monitor pipeline status
- Trigger workflows
- Debug failed deployments
- Automate CI/CD management

---

# Task 6 – Useful gh Tricks

# Raw GitHub API Calls

### Input
```bash
gh api user
```

---

# Create Gist

### Input
```bash
gh gist create notes.txt
```

---

# Create Release

### Input
```bash
gh release create v1.0
```

---

# Create Alias

### Input
```bash
gh alias set co "pr checkout"
```

---

# Search GitHub Repositories

### Input
```bash
gh search repos devops
```

---

# Commands Added to git-commands.md

```bash
gh auth login
gh auth status
gh repo create
gh repo clone
gh repo view
gh repo list
gh issue create
gh issue list
gh issue close
gh pr create
gh pr list
gh pr merge
gh run list
gh api
gh gist create
gh release create
```

---

# Commands Used

```bash
gh auth
gh repo
gh issue
gh pr
gh run
gh api
gh gist
gh release
```

---

# What I Learned

- GitHub CLI allows managing GitHub without leaving terminal.
- Pull requests and issues can be fully automated.
- `gh` is powerful for scripting and DevOps workflows.
- GitHub Actions can be monitored directly from terminal.
- GitHub CLI improves productivity and workflow speed.

---

# Final Summary

Today I practiced:
- Installing GitHub CLI
- Managing repositories from terminal
- Creating issues and pull requests
- Viewing GitHub Actions workflows
- Exploring advanced GitHub automation commands

This challenge improved my understanding of terminal-based GitHub workflows and DevOps automation practices used in real engineering teams.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
