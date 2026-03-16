# Day 32 – Docker Volumes & Networking

## Task Summary
Today I explored two important Docker concepts:

- **Volumes** for persistent data storage
- **Docker networking** for communication between containers

Containers are **ephemeral**, meaning their data is lost when they are removed. Volumes solve this problem. Docker networking allows containers to communicate with each other.

---

# Task 1 – The Problem (Data Loss)

Run a Postgres container:

```bash
docker run -d --name postgres-test \
-e POSTGRES_PASSWORD=pass \
postgres
```

Enter the container:

```bash
docker exec -it postgres-test psql -U postgres
```

Create a table:

```sql
CREATE TABLE users(id INT, name TEXT);
INSERT INTO users VALUES (1, 'DevOps');
SELECT * FROM users;
```

Stop and remove the container:

```bash
docker stop postgres-test
docker rm postgres-test
```

Run a new container:

```bash
docker run -d --name postgres-test \
-e POSTGRES_PASSWORD=pass \
postgres
```

Check data again.

### Result

The data **was gone**.

### Why?

Because container storage is **temporary** and deleted when the container is removed.

---

# Task 2 – Named Volumes (Data Persistence)

Create a named volume:

```bash
docker volume create postgres-data
```

Verify volume:

```bash
docker volume ls
```

Run container with volume:

```bash
docker run -d \
--name postgres-volume \
-e POSTGRES_PASSWORD=pass \
-v postgres-data:/var/lib/postgresql/data \
postgres
```

Create data again:

```bash
docker exec -it postgres-volume psql -U postgres
```

Stop and remove container:

```bash
docker stop postgres-volume
docker rm postgres-volume
```

Run a new container using the **same volume**:

```bash
docker run -d \
--name postgres-volume-new \
-e POSTGRES_PASSWORD=pass \
-v postgres-data:/var/lib/postgresql/data \
postgres
```

### Result

The data **still exists**.

### Verify Volume

```
docker volume ls
docker volume inspect postgres-data
```

---

# Task 3 – Bind Mounts

Create folder on host:

```bash
mkdir my-site
cd my-site
```

Create HTML file:

```
index.html
```

Example content:

```html
<h1>Hello from Docker Bind Mount</h1>
```

Run Nginx container:

```bash
docker run -d \
--name nginx-bind \
-p 8080:80 \
-v $(pwd):/usr/share/nginx/html \
nginx
```

Open browser:

```
http://localhost:8080
```

Edit the HTML file on your host machine and refresh the page.

### Result

Changes appear **instantly**.

---

## Named Volume vs Bind Mount

| Feature | Named Volume | Bind Mount |
|--------|-------------|------------|
| Managed by Docker | Yes | No |
| Stored inside Docker directory | Yes | No |
| Direct host file access | No | Yes |
| Best for | Databases | Development |

---

# Task 4 – Docker Networking Basics

List Docker networks:

```bash
docker network ls
```

Example:

```
bridge
host
none
```

Inspect default bridge network:

```bash
docker network inspect bridge
```

Run two containers:

```bash
docker run -dit --name container1 ubuntu
docker run -dit --name container2 ubuntu
```

Find container IPs:

```bash
docker inspect container1
docker inspect container2
```

Ping by IP:

```bash
docker exec container1 ping <container2-ip>
```

### Result

Containers can communicate by **IP address**.

Try by name:

```bash
docker exec container1 ping container2
```

### Result

Name resolution **does not work on default bridge**.

---

# Task 5 – Custom Networks

Create custom network:

```bash
docker network create my-app-net
```

Run containers on this network:

```bash
docker run -dit --name app1 --network my-app-net ubuntu
docker run -dit --name app2 --network my-app-net ubuntu
```

Test connectivity:

```bash
docker exec app1 ping app2
```

### Result

Containers can now communicate using **container names**.

---

## Why Custom Networks Work

Custom bridge networks include **built-in DNS resolution**.

This allows containers to resolve each other by name.

---

# Task 6 – Application + Database Example

Create network:

```bash
docker network create app-network
```

Run database container:

```bash
docker run -d \
--name mydb \
--network app-network \
-e MYSQL_ROOT_PASSWORD=pass \
-v mysql-data:/var/lib/mysql \
mysql
```

Run application container:

```bash
docker run -dit \
--name myapp \
--network app-network \
ubuntu
```

Test connection:

```bash
docker exec -it myapp ping mydb
```

### Result

The app container successfully reaches the database container using its name.

---

# Commands Used

```
docker volume create
docker volume ls
docker volume inspect
docker run
docker stop
docker rm
docker network ls
docker network create
docker network inspect
docker exec
```

---

# What I Learned

1. Containers lose data when removed unless volumes are used.
2. Named volumes store persistent data managed by Docker.
3. Custom Docker networks allow containers to communicate using container names.

---

# Screenshots

Add screenshots for:

```
docker-volume-ls.png
postgres-volume-test.png
nginx-bind-mount.png
docker-network-ls.png
container-ping-test.png
```

---

# Summary

Today I learned how Docker handles persistent storage and container networking. Volumes ensure that data survives container deletion, and custom networks allow containers to communicate easily. These are essential concepts for running real multi-container applications.
