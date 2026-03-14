# Day 08 – Cloud Server Setup: Docker, Nginx & Web Deployment

## Cloud Provider
AWS EC2 (Ubuntu 22.04 LTS)

Instance Type: t2.micro (Free Tier)

---

# Part 1: Launch Cloud Instance & SSH Access

## Step 1: Create EC2 Instance

- Logged into AWS Console
- Launched a new EC2 instance
- Selected **Ubuntu Server 22.04**
- Instance type **t2.micro**
- Created key pair `devops-key.pem`

Security Group Configuration:

| Port | Protocol | Purpose |
|-----|-----|-----|
| 22 | TCP | SSH |
| 80 | TCP | HTTP |

Instance Public IP:

```
<your-instance-ip>
```

---

## Step 2: Connect via SSH

Change key permissions:

```bash
chmod 400 devops-key.pem
```

Connect to server:

```bash
ssh -i devops-key.pem ubuntu@<your-instance-ip>
```

Observation  
Successfully connected to the remote server via SSH.

### Screenshot

SSH connection terminal:

![SSH Connection](ssh-connection.png)

---

# Part 2: Install Docker & Nginx

## Step 1: Update System

```bash
sudo apt update && sudo apt upgrade -y
```

Observation  
System packages updated successfully.

---

## Step 2: Install Docker

```bash
sudo apt install docker.io -y
```

Verify installation:

```bash
docker --version
```

Example Output

```
Docker version 24.x.x
```

Enable Docker:

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

Check status:

```bash
sudo systemctl status docker
```

Observation  
Docker service is active and running.

---

## Step 3: Install Nginx

```bash
sudo apt install nginx -y
```

Check service:

```bash
sudo systemctl status nginx
```

Example Output

```
Active: active (running)
```

Observation  
Nginx installed successfully.

---

# Part 3: Test Web Access

Open browser and visit:

```
http://<your-instance-ip>
```

You should see the **Nginx welcome page**.

### Screenshot

Nginx webpage from browser:

![Nginx Webpage](nginx-webpage.png)

---

# Part 4: Extract Nginx Logs

## Step 1: View Nginx Logs

```bash
sudo tail -n 20 /var/log/nginx/access.log
```

Observation  
Displays recent HTTP requests to the web server.

---

## Step 2: Save Logs to File

Create a log file:

```bash
sudo tail -n 20 /var/log/nginx/access.log > nginx-logs.txt
```

Verify file:

```bash
cat nginx-logs.txt
```

Observation  
Nginx access logs saved successfully.

---

## Example nginx-logs.txt Content

```
127.0.0.1 - - [20/Mar/2026:10:20:11 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.81.0"
127.0.0.1 - - [20/Mar/2026:10:21:02 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0"
```

These logs show incoming HTTP requests to the Nginx server.

---

## Screenshot of Log File

Example screenshot of log file output:

![Nginx Logs](docker-nginx.png)

---

# Download Log File to Local Machine

Run this command on your local machine:

```bash
scp -i devops-key.pem ubuntu@<your-instance-ip>:~/nginx-logs.txt .
```

Observation  
Log file downloaded successfully to local system.

---

# Commands Used

```
ssh
apt update
apt install docker.io
apt install nginx
systemctl status nginx
systemctl enable docker
systemctl start docker
tail /var/log/nginx/access.log
scp
```

---

# Challenges Faced

Issue: Nginx page was not accessible initially.

Cause: HTTP port (80) was not enabled in security group.

Solution: Added inbound rule for port 80.

---

# What I Learned

- How to launch and configure an AWS EC2 instance
- How to connect to a cloud server using SSH
- Installing and managing services (Docker & Nginx)
- Configuring security groups for web access
- Extracting and analyzing server logs

---

# Files Included in Submission

```
day-08-cloud-deployment.md
nginx-logs.txt
ssh-connection.png
nginx-webpage.png
docker-nginx.png
```

---

# Summary

In this exercise, I launched a cloud server, connected using SSH, installed Docker and Nginx, configured network access, and extracted server logs. This is a practical DevOps workflow used in real-world production environments.
