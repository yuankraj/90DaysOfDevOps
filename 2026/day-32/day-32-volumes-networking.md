# Day 32 – Docker Volumes & Networking

# Objective

Today I learned how Docker handles:
- Data persistence using volumes
- Container communication using Docker networks

This challenge helped me understand two major real-world Docker problems:
1. Containers losing data
2. Containers communicating with each other

---

# Task 1 – The Problem (Data Loss Without Volumes)

# Run MySQL Container

## Input
```bash
docker run -d \
--name mysql-demo \
-e MYSQL_ROOT_PASSWORD=root \
-e MYSQL_DATABASE=testdb \
mysql
```

---

# Enter MySQL Container

## Input
```bash
docker exec -it mysql-demo bash
```

---

# Create Data

## MySQL Commands
```sql
CREATE TABLE users (
id INT,
name VARCHAR(50)
);

INSERT INTO users VALUES (1, 'Vishal');
```

---

# Stop & Remove Container

## Input
```bash
docker stop mysql-demo

docker rm mysql-demo
```

---

# Run New Container Again

## Input
```bash
docker run -d \
--name mysql-demo-new \
-e MYSQL_ROOT_PASSWORD=root \
-e MYSQL_DATABASE=testdb \
mysql
```

---

# Observation

❌ Previous data was lost.

---

# Why Did This Happen?

Containers are ephemeral.

When container is removed:
- Writable container layer is deleted
- All stored data disappears

---

# Task 2 – Named Volumes

# Create Docker Volume

## Input
```bash
docker volume create mysql-data
```

---

# Verify Volume

## Input
```bash
docker volume ls
```

---

# Run MySQL with Volume

## Input
```bash
docker run -d \
--name mysql-volume \
-e MYSQL_ROOT_PASSWORD=root \
-e MYSQL_DATABASE=testdb \
-v mysql-data:/var/lib/mysql \
mysql
```

---

# Add Data Again

## SQL Commands
```sql
INSERT INTO users VALUES (2, 'Docker');
```

---

# Stop & Remove Container

## Input
```bash
docker stop mysql-volume

docker rm mysql-volume
```

---

# Run New Container with Same Volume

## Input
```bash
docker run -d \
--name mysql-volume-new \
-e MYSQL_ROOT_PASSWORD=root \
-e MYSQL_DATABASE=testdb \
-v mysql-data:/var/lib/mysql \
mysql
```

---

# Observation

✅ Data still existed.

---

# Inspect Volume

## Input
```bash
docker volume inspect mysql-data
```

---

# What I Learned

Named volumes store data outside container lifecycle.

Even if container is deleted:
- Volume data remains safe

---

# Task 3 – Bind Mounts

# Create Local Folder

## Input
```bash
mkdir website

cd website
```

---

# Create index.html

## index.html
```html
<h1>Hello from Bind Mount!</h1>
```

---

# Run Nginx with Bind Mount

## Input
```bash
docker run -d \
-p 8080:80 \
-v $(pwd):/usr/share/nginx/html \
nginx
```

---

# Access Browser

```bash
http://localhost:8080
```

### Observation
- Custom page displayed successfully.

---

# Edit index.html on Host

## Updated File
```html
<h1>Updated from Host Machine!</h1>
```

---

# Refresh Browser

### Observation
✅ Changes reflected instantly.

---

# Named Volume vs Bind Mount

| Named Volume | Bind Mount |
|--------------|-------------|
| Managed by Docker | Uses host filesystem directly |
| Better portability | Direct host access |
| Preferred for databases | Useful for development |
| Safer isolation | Easier live editing |

---

# Task 4 – Docker Networking Basics

# List Docker Networks

## Input
```bash
docker network ls
```

---

# Inspect Bridge Network

## Input
```bash
docker network inspect bridge
```

---

# Run Two Containers on Default Bridge

## Input
```bash
docker run -dit --name container1 ubuntu bash

docker run -dit --name container2 ubuntu bash
```

---

# Ping by Name

## Input
```bash
docker exec container1 ping container2
```

### Observation
❌ Failed

---

# Ping by IP

## Input
```bash
docker inspect container2
```

Get IP:
```bash
172.17.0.3
```

---

## Ping Using IP

```bash
docker exec container1 ping 172.17.0.3
```

### Observation
✅ Success

---

# Why?

Default bridge network:
- Does not provide automatic DNS name resolution

---

# Task 5 – Custom Networks

# Create Custom Network

## Input
```bash
docker network create my-app-net
```

---

# Run Containers on Custom Network

## Input
```bash
docker run -dit --name app1 --network my-app-net ubuntu bash

docker run -dit --name app2 --network my-app-net ubuntu bash
```

---

# Ping by Name

## Input
```bash
docker exec app1 ping app2
```

### Observation
✅ Success

---

# Why Custom Networks Work?

Custom bridge networks provide:
- Automatic DNS resolution
- Better container isolation
- Easier service communication

---

# Task 6 – Put It Together

# Create Network

## Input
```bash
docker network create full-stack-net
```

---

# Run Database Container

## Input
```bash
docker run -d \
--name postgres-db \
--network full-stack-net \
-v postgres-data:/var/lib/postgresql/data \
-e POSTGRES_PASSWORD=root \
postgres
```

---

# Run App Container

## Input
```bash
docker run -dit \
--name app-container \
--network full-stack-net \
ubuntu bash
```

---

# Test Database Connectivity

## Input
```bash
docker exec app-container ping postgres-db
```

### Observation
✅ Database reachable using container name.

---

# Important Commands Learned

| Command | Purpose |
|----------|----------|
| docker volume create | Create volume |
| docker volume ls | List volumes |
| docker volume inspect | Inspect volume |
| docker network create | Create network |
| docker network ls | List networks |
| docker network inspect | Inspect network |
| docker exec | Execute command in container |

---

# Commands Used

```bash
docker volume create
docker volume ls
docker volume inspect
docker network create
docker network ls
docker network inspect
docker exec
docker run
docker stop
docker rm
```

---

# What I Learned

- Containers lose data without volumes.
- Named volumes persist data safely.
- Bind mounts sync host and container files.
- Custom Docker networks support DNS-based communication.
- Containers communicate better using custom bridge networks.

---

# Why This Matters in DevOps

Volumes and networking are critical because:
- Databases require persistent storage
- Microservices communicate over networks
- Kubernetes heavily relies on networking concepts
- CI/CD environments often use containers communicating together

---

# Final Summary

Today I practiced:
- Docker data persistence
- Named volumes
- Bind mounts
- Docker networking
- Container communication
- Multi-container setups

This challenge gave me real-world understanding of how Docker containers store data and communicate with each other in production environments.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
