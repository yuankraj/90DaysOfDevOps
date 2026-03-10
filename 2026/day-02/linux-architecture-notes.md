Linux Architecture Overview
---------------------------

Linux follows a layered architecture that separates hardware interaction from user applications.

### 1\. Kernel

The **kernel** is the core of the Linux operating system.

Responsibilities:

*   Hardware communication (CPU, memory, disk, network)
    
*   Process management
    
*   Memory management
    
*   Device drivers
    
*   File system management
    

The kernel acts as a bridge between **hardware and user applications**.

### 2\. User Space

User space is where **applications and user programs run**.

Examples:

*   Bash shell
    
*   Python programs
    
*   Web servers (Nginx, Apache)
    
*   System utilities
    

Applications cannot directly access hardware; they interact with the **kernel using system calls**.

### 3\. Init / systemd

When Linux boots, the first process started is **init**.

In modern Linux systems, **systemd** replaces traditional init systems.

systemd responsibilities:

*   Starts system services during boot
    
*   Manages background services (daemons)
    
*   Handles service restarts
    
*   Manages logs through journald
    

Example services managed by systemd:

*   ssh
    
*   docker
    
*   nginx
    

systemd makes system management **faster, more organized, and easier to troubleshoot**.

Linux Processes
===============

A **process** is a running instance of a program.

Examples:

*   Running a Python script
    
*   Starting a web server
    
*   Executing a shell command
    

Each process has:

*   **PID (Process ID)**
    
*   Memory allocation
    
*   CPU usage
    
*   Process state
    

Processes are created using the **fork() system call**, where a parent process creates a child process.

Process States
==============

Common Linux process states:

**Running (R)**

*   Process is currently executing on CPU.
    

**Sleeping (S)**

*   Waiting for an event (very common state).
    

**Stopped (T)**

*   Process paused or stopped by user.
    

**Zombie (Z)**

*   Process finished execution but still exists in the process table until the parent process reads its exit status.
    

Zombie processes usually indicate **poor process handling in programs**.

systemd Service Management
==========================

systemd manages services using **units**.

Common service operations:

Start a service


systemctl start nginx  

Stop a service


systemctl stop nginx 

Check service status

 
systemctl status nginx 

Enable service at boot


systemctl enable nginx 



systemd improves system reliability by **automatically restarting failed services**.


Daily Linux Commands for DevOps
===============================

These commands are commonly used for troubleshooting.

**1\. ps**


ps aux  

Shows running processes.

**2\. top**

  
top   

Displays real-time CPU and memory usage.

**3\. systemctl**


systemctl status nginx

Manages system services.

**4\. journalctl**


journalctl -u nginx   

View service logs.

**5\. kill**

 
kill -9

Stops a process manually.

Key Takeaway
============

Understanding **Linux architecture, processes, and systemd** helps DevOps engineers:

*   Troubleshoot failing services
    
*   Diagnose CPU/memory problems
    
*   Manage production servers efficiently
    

Linux knowledge is the **foundation of DevOps operations**.
