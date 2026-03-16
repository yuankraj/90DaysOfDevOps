# Day 26 – GitHub CLI (gh): Manage GitHub from Your Terminal

## Task Summary
Today I learned how to use the **GitHub CLI (`gh`)** to interact with GitHub directly from the terminal.  
This allows developers and DevOps engineers to manage repositories, issues, pull requests, and workflows without switching to the browser.

---

# Task 1 – Install and Authenticate

### Install GitHub CLI

Ubuntu/Debian:

```bash
sudo apt update
sudo apt install gh
```

Mac (Homebrew):

```bash
brew install gh
```

Verify installation:

```bash
gh --version
```

Example Output

```
gh version 2.x.x
```

---

### Authenticate with GitHub

```bash
gh auth login
```

Steps:
- Choose GitHub.com
- Choose HTTPS
- Authenticate via browser or token

---

### Verify Login

```bash
gh auth status
```

Example Output

```
Logged in to github.com as <username>
```

---

### Authentication Methods Supported

`gh` supports:

| Method | Description |
|------|-------------|
| Browser login | Opens GitHub in browser for authentication |
| Personal Access Token (PAT) | Login using a GitHub token |
| SSH authentication | Use existing SSH keys |

---

# Task 2 – Working with Repositories

### Create Repository from Terminal

```bash
gh repo create test-gh-cli --public --clone
```

This creates a GitHub repository and clones it locally.

---

### Clone Repository Using gh

```bash
gh repo clone <username>/<repo-name>
```

Example

```
gh repo clone vishal/devops-git-practice
```

---

### View Repository Details

```bash
gh repo view
```

---

### List All Repositories

```bash
gh repo list
```

---

### Open Repo in Browser

```bash
gh repo view --web
```

---

### Delete Repository

```bash
gh repo delete <username>/<repo-name>
```

Example

```
gh repo delete vishal/test-gh-cli
```

---

# Task 3 – Issues

### Create Issue

```bash
gh issue create --title "Test Issue" --body "Testing GitHub CLI issue creation" --label bug
```

---

### List Issues

```bash
gh issue list
```

---

### View Specific Issue

```bash
gh issue view 1
```

---

### Close Issue

```bash
gh issue close 1
```

---

### Using gh issue in Automation

The `gh issue` command can be used in scripts to:

- Automatically create issues from monitoring alerts
- Track CI/CD failures
- Manage backlog tasks programmatically

---

# Task 4 – Pull Requests

### Create Branch

```bash
git switch -c feature-cli-test
```

Make change and commit:

```bash
git add .
git commit -m "Add CLI test change"
git push origin feature-cli-test
```

---

### Create Pull Request

```bash
gh pr create --fill
```

This auto-fills title and description from commits.

---

### List Pull Requests

```bash
gh pr list
```

---

### View Pull Request

```bash
gh pr view
```

---

### Merge Pull Request

```bash
gh pr merge
```

---

### Merge Methods Supported

| Method | Description |
|------|-------------|
| merge | Creates merge commit |
| squash | Combines commits into one |
| rebase | Rebases commits before merging |

Example

```
gh pr merge --squash
```

---

### Review Someone Else’s PR

Commands:

```bash
gh pr checkout <pr-number>
gh pr view
gh pr review
```

You can approve or comment on PRs directly from the terminal.

---

# Task 5 – GitHub Actions Preview

### List Workflow Runs

```bash
gh run list
```

---

### View Workflow Run

```bash
gh run view <run-id>
```

---

### Use in CI/CD Pipelines

`gh run` and `gh workflow` can help:

- Monitor pipeline status
- Trigger workflows
- Automate deployments
- Debug failing CI jobs

---

# Task 6 – Useful gh Tricks

### GitHub API Calls

```bash
gh api repos/<owner>/<repo>
```

---

### Manage Gists

```bash
gh gist create file.txt
```

---

### Manage Releases

```bash
gh release create v1.0
```

---

### Create Aliases

```bash
gh alias set co 'pr checkout'
```

Example usage:

```
gh co 12
```

---

### Search GitHub Repositories

```bash
gh search repos devops
```

---

# Commands Used

```
gh auth login
gh auth status
gh repo create
gh repo clone
gh repo view
gh repo list
gh repo delete
gh issue create
gh issue list
gh issue view
gh issue close
gh pr create
gh pr list
gh pr view
gh pr merge
gh run list
gh run view
gh api
gh gist
gh release
gh alias
gh search repos
```

---

# What I Learned

1. GitHub CLI allows managing repositories, issues, and pull requests directly from the terminal.
2. It helps automate GitHub workflows and integrate with scripts.
3. The CLI is especially useful for DevOps engineers working with CI/CD pipelines and automation.

---

# Summary

Today I explored GitHub CLI (`gh`) and learned how to manage repositories, issues, pull requests, and workflow runs from the terminal. This tool improves productivity by reducing the need to switch between the terminal and browser while working with GitHub.
