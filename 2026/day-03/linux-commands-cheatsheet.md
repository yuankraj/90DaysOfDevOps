# Linux Commands Cheat Sheet

## 1. Process Management Commands

| Command | Usage |
|---|---|
| `ps aux` | Show all running processes |
| `top` | Real-time process monitoring |
| `htop` | Interactive process viewer |
| `kill PID` | Kill a process using PID |
| `killall nginx` | Kill process by name |
| `jobs` | Show background jobs |
| `bg` | Run job in background |
| `fg` | Bring job to foreground |
| `nice` | Start process with priority |
| `uptime` | Show system uptime and load |

---

## 2. File System Commands

| Command | Usage |
|---|---|
| `pwd` | Show current directory |
| `ls -la` | List files with details |
| `cd` | Change directory |
| `mkdir test` | Create directory |
| `rm -rf folder` | Remove directory/files |
| `cp file1 file2` | Copy files |
| `mv file1 file2` | Move or rename files |
| `touch file.txt` | Create empty file |
| `cat file.txt` | View file content |
| `find / -name nginx` | Search files/directories |

---

## 3. Log & Monitoring Commands

| Command | Usage |
|---|---|
| `journalctl -xe` | View system logs |
| `tail -f app.log` | Live log monitoring |
| `df -h` | Check disk usage |
| `du -sh folder` | Check folder size |
| `free -m` | Check memory usage |

---

## 4. Networking Troubleshooting Commands

| Command | Usage |
|---|---|
| `ping google.com` | Check network connectivity |
| `ip addr` | Show IP addresses |
| `curl https://google.com` | Test HTTP request |
| `dig google.com` | DNS lookup |
| `ss -tulnp` | Show listening ports |
| `netstat -tulnp` | Network connections info |
| `traceroute google.com` | Trace network path |

---

## 5. Why These Commands Matter in DevOps

These commands help to:
- Debug production issues
- Monitor processes and logs
- Troubleshoot network problems
- Analyze system performance
- Manage Linux servers efficiently

Linux command-line skills are essential for every DevOps Engineer.
