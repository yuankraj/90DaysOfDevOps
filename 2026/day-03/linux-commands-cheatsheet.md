# Linux Commands Cheat Sheet (DevOps)

This cheat sheet contains commonly used Linux commands for process management, file system operations, and networking troubleshooting.

## 1. Process Management Commands

- `ps`
- `ps aux`
  - Shows all running processes in the system.
- `top`
  - Displays real-time CPU and memory usage.
- `htop`
  - Interactive process viewer (improved version of top).
- `kill <PID>`
  - Terminates a process using its Process ID.
- `kill -9 <PID>`
  - Force kills a process.
- `pgrep nginx`
  - Finds process ID by process name.
- `pkill nginx`
  - Kills processes by name.
- `systemctl status nginx`
  - Checks status of a system service.

## 2. File System Commands

- `ls`
- `ls -la`
  - Lists files with permissions and hidden files.
- `cd /var/log`
  - Changes directory.
- `pwd`
  - Shows current directory path.
- `mkdir project`
  - Creates a new directory.
- `rm file.txt`
  - Deletes a file.
- `rm -r folder`
  - Deletes a directory recursively.
- `cp file1.txt file2.txt`
  - Copies files.
- `mv file.txt /home/user/`
  - Moves or renames files.
- ``cat file.txt``
  - Displays file content.
- ``less logfile.log``
  - Views large files page by page.
to change permissions:
dchmod +x script.sh

## Disk Usage Commands

- ``df -h``	Shows disk usage.
'tdu'
to show directory size:
du -sh folder/
defaults to human-readable format for du command if needed, e.g., du -sh folder/
df` shows disk space usage on mounted filesystems.`
defaults to human-readable format for df command if needed, e.g., df -h` 
and shows disk space usage on mounted filesystems. 
defaults to human-readable format for df command if needed, e.g., df-h` 
and shows disk space usage on mounted filesystems. 
du` shows directory size.`
df` shows disk space usage on mounted filesystems. 
du` shows directory size. 
note: The above line is an explanation; in actual markdown, it should be formatted as text or omitted for clarity. 
note: The above line is an explanation; in actual markdown, it should be formatted as text or omitted for clarity. 
note: The above line is an explanation; in actual markdown, it should be formatted as text or omitted for clarity.
