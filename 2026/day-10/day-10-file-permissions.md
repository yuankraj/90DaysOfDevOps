# Day 10 – File Permissions & File Operations Challenge

## Objective
Practice creating, reading, and modifying files while understanding Linux file permissions.

---

# Task 1 – Create Files

### Create empty file

```bash
touch devops.txt
```

### Create file with content

```bash
echo "DevOps notes for Linux practice" > notes.txt
```

### Create script file using vim

```bash
vim script.sh
```

Add this content inside vim:

```
echo "Hello DevOps"
```

Save and exit:

```
:wq
```

### Verify files

```bash
ls -l
```

Example Output

```
-rw-r--r-- 1 user user 0 Mar 20 devops.txt
-rw-r--r-- 1 user user 30 Mar 20 notes.txt
-rw-r--r-- 1 user user 18 Mar 20 script.sh
```

Observation  
All files are created successfully.

---

# Task 2 – Read Files

### Read notes.txt

```bash
cat notes.txt
```

Example Output

```
DevOps notes for Linux practice
```

---

### View script.sh in read-only mode

```bash
vim -R script.sh
```

Observation  
Opens file in read-only mode.

---

### Display first 5 lines of /etc/passwd

```bash
head -n 5 /etc/passwd
```

Example Output

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
```

---

### Display last 5 lines of /etc/passwd

```bash
tail -n 5 /etc/passwd
```

Example Output

```
systemd-timesync:x:998:998:systemd Time Sync:/run/systemd:/usr/sbin/nologin
messagebus:x:100:102::/nonexistent:/usr/sbin/nologin
```

---

# Task 3 – Understand Permissions

Check permissions:

```bash
ls -l devops.txt notes.txt script.sh
```

Example Output

```
-rw-r--r-- devops.txt
-rw-r--r-- notes.txt
-rw-r--r-- script.sh
```

Permission format:

```
rwxrwxrwx
owner group others
```

Meaning:

| Permission | Value |
|------|------|
| Read | 4 |
| Write | 2 |
| Execute | 1 |

Example:

```
rw-r--r--
Owner: read + write
Group: read
Others: read
```

---

# Task 4 – Modify Permissions

### Make script executable

```bash
chmod +x script.sh
```

Run script:

```bash
./script.sh
```

Output

```
Hello DevOps
```

---

### Make devops.txt read-only

```bash
chmod a-w devops.txt
```

Verify

```bash
ls -l devops.txt
```

Example Output

```
-r--r--r-- devops.txt
```

---

### Set notes.txt permission to 640

```bash
chmod 640 notes.txt
```

Verify

```bash
ls -l notes.txt
```

Example Output

```
-rw-r----- notes.txt
```

---

### Create directory with permission 755

```bash
mkdir project
chmod 755 project
```

Verify

```bash
ls -ld project
```

Example Output

```
drwxr-xr-x project
```

---

# Task 5 – Test Permissions

### Try writing to read-only file

```bash
echo "new text" >> devops.txt
```

Error

```
Permission denied
```

Observation  
Write operation fails because write permission was removed.

---

### Try executing a file without execute permission

```bash
chmod -x script.sh
./script.sh
```

Error

```
Permission denied
```

Observation  
File must have execute permission to run.

---

# Files Created

```
devops.txt
notes.txt
script.sh
project/
```

---

# Permission Changes

| File | Before | After |
|-----|------|------|
| script.sh | rw-r--r-- | rwxr-xr-x |
| devops.txt | rw-r--r-- | r--r--r-- |
| notes.txt | rw-r--r-- | rw-r----- |
| project | default | 755 |

---

# Commands Used

```
touch
echo
vim
cat
head
tail
ls
chmod
mkdir
```

---

# Screenshots

Add screenshots for:

```
file-creation.png
permission-before.png
permission-after.png
script-execution.png
```

---

# What I Learned

- Linux file permissions control who can read, write, and execute files.
- `chmod` is used to modify permissions using symbolic or numeric modes.
- Executable permissions are required to run scripts.

---

# Summary

In this exercise, I practiced Linux file operations and permission management. Understanding file permissions is critical for securing systems, controlling access, and managing scripts in DevOps environments.
