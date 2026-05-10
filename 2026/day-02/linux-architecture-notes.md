# Linux Architecture, Processes, and systemd

## 1. Core Components of Linux

### Kernel
- Core part of Linux OS
- Manages:
  - CPU
  - Memory
  - Devices
  - File system
  - Processes
- Acts as bridge between hardware and software

### User Space
- Area where user applications run
- Examples:
  - Bash
  - Chrome
  - VS Code
  - Python programs
- Cannot directly access hardware

### init / systemd
- First process started by kernel (PID 1)
- Manages system startup and services
- Starts background services like:
  - SSH
  - Docker
  - Nginx

---

## 2. Linux Process Management

### What is a Process?
- A running instance of a program
- Each process has:
  - PID (Process ID)
  - State
  - Memory usage
  - CPU usage

### Process Creation
- Parent process creates child process using `fork()`
- New program loaded using `exec()`

### Important Process States

| State | Meaning |
|---|---|
| Running (R) | Currently using CPU |
| Sleeping (S) | Waiting for event/input |
| Stopped (T) | Manually stopped |
| Zombie (Z) | Finished but not cleaned |
| Dead (X) | Process terminated |

---

## 3. What is systemd?

### systemd
- Modern Linux init system
- Handles:
  - Service management
  - Boot process
  - Logging
  - Restart policies

### Why systemd Matters in DevOps
- Automatically restarts failed services
- Helps monitor system services
- Used in almost all production Linux servers

### Useful systemd Commands

```bash
systemctl status nginx
systemctl start nginx
systemctl stop nginx
systemctl restart nginx
journalctl -u nginx
