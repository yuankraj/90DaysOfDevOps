# Day 33 – Docker Compose: Multi-Container Basics

## Task Summary
Today I learned how to run **multi-container applications using Docker Compose**.  
Docker Compose allows us to define services, networks, and volumes in a single YAML file and start everything with one command.

---

# Task 1 – Install & Verify Docker Compose

Check Docker Compose version:

```bash
docker compose version
```

Example Output

```
Docker Compose version v2.x.x
```

Docker Compose is included with Docker Desktop and modern Docker installations.

---

# Task 2 – First Docker Compose File

## Project Structure

```
compose-basics/
 ├── docker-compose.yml
```

---

## docker-compose.yml

```yaml
version: "3"

services:
  nginx:
    image: nginx
    ports:
      - "8080:80"
```

---

## Start Container

```bash
docker compose up
```

Run in background:

```bash
docker compose up -d
```

---

## Access in Browser

```
http://localhost:8080
```

You should see the **Nginx welcome page**.

---

## Stop Containers

```bash
docker compose down
```

---

# Task 3 – WordPress + MySQL Setup

## Project Structure

```
wordpress-compose/
 ├── docker-compose.yml
```

---

## docker-compose.yml

```yaml
version: "3.8"

services:

  db:
    image: mysql:5.7
    container_name: wordpress-db
    restart: always
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
      MYSQL_ROOT_PASSWORD: rootpassword
    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    container_name: wordpress-app
    restart: always
    ports:
      - "8081:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db

volumes:
  db_data:
```

---

## Start WordPress

```bash
docker compose up -d
```

Open browser:

```
http://localhost:8081
```

Follow the WordPress installation steps.

---

## Verify Data Persistence

Stop services:

```bash
docker compose down
```

Start again:

```bash
docker compose up -d
```

Result:

WordPress data remains because MySQL uses a **named volume**.

---

# Task 4 – Docker Compose Commands

## Start Services in Detached Mode

```bash
docker compose up -d
```

---

## View Running Services

```bash
docker compose ps
```

---

## View Logs of All Services

```bash
docker compose logs
```

---

## Follow Logs (Real-Time)

```bash
docker compose logs -f
```

---

## Logs of Specific Service

```bash
docker compose logs wordpress
```

---

## Stop Services Without Removing

```bash
docker compose stop
```

---

## Remove Containers, Networks, and Volumes

```bash
docker compose down
```

Remove volumes too:

```bash
docker compose down -v
```

---

## Rebuild Images

```bash
docker compose up --build
```

---

# Task 5 – Environment Variables

## Using Environment Variables in Compose

Example inside `docker-compose.yml`:

```yaml
environment:
  MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
```

---

## Create `.env` File

```
MYSQL_ROOT_PASSWORD=rootpassword
MYSQL_DATABASE=wordpress
MYSQL_USER=wordpress
MYSQL_PASSWORD=wordpress
```

---

## Updated docker-compose.yml Example

```yaml
environment:
  MYSQL_DATABASE: ${MYSQL_DATABASE}
  MYSQL_USER: ${MYSQL_USER}
  MYSQL_PASSWORD: ${MYSQL_PASSWORD}
  MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
```

Docker Compose automatically loads variables from `.env`.

---

# Commands Used

```
docker compose version
docker compose up
docker compose up -d
docker compose down
docker compose ps
docker compose logs
docker compose logs -f
docker compose stop
docker compose up --build
```

---

# What I Learned

1. Docker Compose allows running multiple containers using a single YAML configuration file.
2. Compose automatically creates networks so services can communicate using service names.
3. Named volumes allow persistent data storage for applications like databases.

---

# Screenshots

Add screenshots for:

```
compose-nginx-running.png
wordpress-compose-running.png
docker-compose-ps.png
docker-compose-logs.png
```

---

# Summary

Today I learned how to use Docker Compose to manage multi-container applications. Using Compose simplifies running complex setups like WordPress and MySQL by defining everything in a single configuration file and running it with one command.
