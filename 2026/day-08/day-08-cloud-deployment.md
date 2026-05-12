# Day 08 – Cloud Server Setup: Docker, Nginx & Web Deployment

# Objective

Today I deployed a real cloud server and configured Nginx for web hosting.

Tasks completed:
- Launched a cloud instance
- Connected using SSH
- Installed Docker and Nginx
- Configured firewall/security group
- Verified web access from browser
- Extracted and saved Nginx logs

---

# Part 1 – Launch Cloud Instance & SSH Access

## Step 1 – Launch EC2/Utho Instance

### Configuration Used
- OS: Ubuntu 24.04 LTS
- Instance Type: t2.micro
- Storage: 8GB
- Security Group:
  - Port 22 (SSH)
  - Port 80 (HTTP)

### Observation
- Instance launched successfully.
- Public IP assigned correctly.

---

## Step 2 – Connect Using SSH

### Input
```bash
ssh -i devops.pem ubuntu@<your-instance-ip>
```

### Output
```bash
Welcome to Ubuntu 24.04 LTS
```

### Observation
- SSH connection established successfully.
- Remote cloud server accessible.

📸 Screenshot Taken:
- ssh-connection.png

---

# Part 2 – Install Docker & Nginx

## Step 1 – Update System

### Input
```bash
sudo apt update && sudo apt upgrade -y
```

### Output
```bash
Packages upgraded successfully
```

### Observation
- System updated with latest packages.

---

## Step 2 – Install Docker

### Input
```bash
sudo apt install docker.io -y
```

### Output
```bash
Docker installed successfully
```

### Verify Docker

#### Input
```bash
docker --version
```

#### Output
```bash
Docker version 27.0.1
```

### Observation
- Docker installed and working properly.

---

## Step 3 – Install Nginx

### Input
```bash
sudo apt install nginx -y
```

### Output
```bash
Nginx installed successfully
```

---

## Step 4 – Verify Nginx Service

### Input
```bash
sudo systemctl status nginx
```

### Output
```bash
Active: active (running)
```

### Observation
- Nginx web server running successfully.

---

# Part 3 – Security Group Configuration

## Allowed Ports
- 22 → SSH Access
- 80 → HTTP Web Access

### Test Website

Browser URL:
```bash
http://<your-instance-ip>
```

### Observation
- Nginx welcome page opened successfully from browser.

📸 Screenshot Taken:
- nginx-webpage.png

---

# Part 4 – Extract Nginx Logs

## Step 1 – View Nginx Logs

### Input
```bash
sudo tail -n 20 /var/log/nginx/access.log
```

### Output
```bash
GET / HTTP/1.1 200
```

### Observation
- Incoming web requests recorded successfully.

---

## Step 2 – Save Logs to File

### Input
```bash
sudo cp /var/log/nginx/access.log ~/nginx-logs.txt
```

### Output
```bash
File copied successfully
```

### Observation
- Nginx logs extracted into text file.

---

## Step 3 – Download Log File

### Input
```bash
scp -i devops.pem ubuntu@<your-instance-ip>:~/nginx-logs.txt .
```

### Output
```bash
nginx-logs.txt downloaded successfully
```

### Observation
- Log file transferred to local machine.

📸 Screenshot Taken:
- docker-nginx.png

---

# Commands Used

```bash
ssh -i devops.pem ubuntu@<ip>
sudo apt update
sudo apt upgrade -y
sudo apt install docker.io -y
docker --version
sudo apt install nginx -y
sudo systemctl status nginx
tail -n 20 /var/log/nginx/access.log
scp -i devops.pem ubuntu@<ip>:~/nginx-logs.txt .
```

---

# Challenges Faced

## Problem 1 – Website Not Opening
### Cause
- Port 80 was blocked in security group.

### Solution
- Allowed HTTP traffic on port 80.

---

## Problem 2 – SSH Permission Error
### Cause
- Incorrect permissions on `.pem` file.

### Solution
```bash
chmod 400 devops.pem
```

---

# What I Learned

- How to launch and access a real cloud server
- How SSH works for remote server management
- Installing and managing Nginx service
- Basic Docker installation
- Importance of security groups and firewall rules
- How to access and save Nginx logs

---

# Final Summary

Today I deployed my first real cloud web server using Ubuntu, Docker, and Nginx.

I learned:
- Cloud infrastructure basics
- Remote server access with SSH
- Installing production services
- Managing logs and security groups

This felt like real DevOps work and improved my confidence with Linux and cloud deployment.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
