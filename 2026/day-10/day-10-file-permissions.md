# Day 10 – File Permissions & File Operations Challenge

# Objective

Today I practiced Linux file operations and file permission management using commands like `touch`, `cat`, `vim`, and `chmod`.

---

# Task 1 – Create Files

## Create Empty File

### Input
```bash
touch devops.txt
```

### Output
```bash
File created successfully
```

---

## Create notes.txt with Content

### Input
```bash
echo "Linux permissions are important." > notes.txt
```

### Output
```bash
(No output)
```

---

## Create script.sh using vim

### Input
```bash
vim script.sh
```

### File Content
```bash
echo "Hello DevOps"
```

---

## Verify Files

### Input
```bash
ls -l
```

### Output
```bash
-rw-r--r-- 1 user user 0 May 13 10:10 devops.txt
-rw-r--r-- 1 user user 35 May 13 10:12 notes.txt
-rw-r--r-- 1 user user 21 May 13 10:15 script.sh
```

### Observation
- All files created successfully.
- No execute permission set initially.

---

# Task 2 – Read Files

## Read notes.txt

### Input
```bash
cat notes.txt
```

### Output
```bash
Linux permissions are important.
```

---

## Open script.sh in Read-only Mode

### Input
```bash
vim -R script.sh
```

### Observation
- File opened in read-only mode.

---

## Display First 5 Lines of /etc/passwd

### Input
```bash
head -n 5 /etc/passwd
```

### Output
```bash
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
```

---

## Display Last 5 Lines of /etc/passwd

### Input
```bash
tail -n 5 /etc/passwd
```

### Output
```bash
tokyo:x:1001:1001::/home/tokyo:/bin/sh
berlin:x:1002:1002::/home/berlin:/bin/sh
```

---

# Task 3 – Understand Permissions

## Check Permissions

### Input
```bash
ls -l devops.txt notes.txt script.sh
```

### Output
```bash
-rw-r--r-- devops.txt
-rw-r--r-- notes.txt
-rw-r--r-- script.sh
```

---

## Permission Breakdown

| Permission | Meaning |
|------------|----------|
| rw- | Owner can read/write |
| r-- | Group can read |
| r-- | Others can read |

### Observation
- Files are readable by everyone.
- No file is executable yet.

---

# Task 4 – Modify Permissions

## Make script.sh Executable

### Input
```bash
chmod +x script.sh
```

---

## Verify Permission

### Input
```bash
ls -l script.sh
```

### Output
```bash
-rwxr-xr-x script.sh
```

---

## Run Script

### Input
```bash
./script.sh
```

### Output
```bash
Hello DevOps
```

---

## Make devops.txt Read-only

### Input
```bash
chmod a-w devops.txt
```

---

## Verify Permission

### Input
```bash
ls -l devops.txt
```

### Output
```bash
-r--r--r-- devops.txt
```

---

## Set notes.txt Permission to 640

### Input
```bash
chmod 640 notes.txt
```

---

## Verify Permission

### Input
```bash
ls -l notes.txt
```

### Output
```bash
-rw-r----- notes.txt
```

---

## Create project Directory with 755 Permission

### Input
```bash
mkdir project
chmod 755 project
```

---

## Verify Directory Permission

### Input
```bash
ls -ld project
```

### Output
```bash
drwxr-xr-x 2 user user 4096 May 13 10:30 project
```

---

# Task 5 – Test Permissions

## Try Writing to Read-only File

### Input
```bash
echo "Test" >> devops.txt
```

### Output
```bash
Permission denied
```

### Observation
- Write access removed successfully.

---

## Try Executing Non-executable File

### Input
```bash
./notes.txt
```

### Output
```bash
Permission denied
```

### Observation
- Files require execute permission (`x`) to run.

---

# Files Created

- devops.txt
- notes.txt
- script.sh
- project/

---

# Permission Changes

| File/Directory | Before | After |
|----------------|--------|-------|
| script.sh | rw-r--r-- | rwxr-xr-x |
| devops.txt | rw-r--r-- | r--r--r-- |
| notes.txt | rw-r--r-- | rw-r----- |
| project/ | default | rwxr-xr-x |

---

# Commands Used

```bash
touch
echo
cat
vim
head
tail
chmod
mkdir
ls -l
```

---

# What I Learned

- Linux permissions control who can read, write, and execute files.
- `chmod` is used to modify permissions quickly.
- Scripts require execute permission to run.
- Directories also use permissions for access control.
- File permission management is important for Linux security.

---

# Final Summary

Today I practiced:
- File creation and reading
- Understanding Linux permission structure
- Modifying permissions using chmod
- Testing access control and execution

This challenge improved my Linux fundamentals and understanding of file security in DevOps environments.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
