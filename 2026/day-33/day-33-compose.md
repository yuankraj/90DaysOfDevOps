# Day 33 – Docker Compose: Multi-Container Basics

# Objective

Today I learned how to manage multi-container applications using Docker Compose.

Instead of manually creating:
- Containers
- Networks
- Volumes

Docker Compose allows everything to be managed from a single YAML file.

This challenge helped me understand how real-world applications run multiple services together.

---

# Task 1 – Install & Verify Docker Compose

# Check Docker Compose

## Input
```bash
docker compose version
```

## Output
```bash
Docker Compose version v2.27.0
```

### Observation
- Docker Compose installed successfully.

---

# Task 2 – First Docker Compose File

# Create Project Folder

## Input
```bash
mkdir compose-basics

cd compose-basics
```

---

# Create docker-compose.yml

## docker-compose.yml
```yaml
services:
  nginx:
    image: nginx
    ports:
      - "8080:80"
```

---

# Start Compose Application

## Input
```bash
docker compose up
```

### Observation
- Nginx container started successfully.

---

# Access in Browser

```bash
http://localhost:8080
```

### Result
✅ Nginx welcome page displayed successfully.

---

# Stop Compose Application

## Input
```bash
docker compose down
```

### Observation
- Containers and network removed automatically.

---

# Task 3 – WordPress + MySQL Setup

# Create docker-compose.yml

## docker-compose.yml
```yaml
services:

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wppassword

    volumes:
      - mysql-data:/var/lib/mysql

  wordpress:
    image: wordpress
    restart: always
    ports:
      - "8081:80"

    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppassword
      WORDPRESS_DB_NAME: wordpress

    depends_on:
      - db

volumes:
  mysql-data:
```

---

# Start Multi-Container Application

## Input
```bash
docker compose up -d
```

### Observation
- Both MySQL and WordPress containers started successfully.

---

# Access WordPress

```bash
http://localhost:8081
```

### Result
✅ WordPress setup page loaded successfully.

---

# Why WordPress Connected Automatically?

Because:
- Docker Compose automatically creates a shared network
- Service names become DNS names

Example:
```bash
WORDPRESS_DB_HOST=db
```

WordPress resolves `db` automatically.

---

# Verify Data Persistence

# Stop Application

## Input
```bash
docker compose down
```

---

# Restart Application

## Input
```bash
docker compose up -d
```

### Observation
✅ WordPress data still existed.

---

# Why Data Persisted?

Because MySQL data was stored inside:
```yaml
volumes:
  - mysql-data:/var/lib/mysql
```

Named volumes survive container deletion.

---

# Task 4 – Docker Compose Commands

# Start in Detached Mode

## Input
```bash
docker compose up -d
```

---

# View Running Services

## Input
```bash
docker compose ps
```

---

# View Logs of All Services

## Input
```bash
docker compose logs
```

---

# Follow Logs in Real Time

## Input
```bash
docker compose logs -f
```

---

# Logs of Specific Service

## Input
```bash
docker compose logs wordpress
```

---

# Stop Services Without Removing

## Input
```bash
docker compose stop
```

---

# Remove Containers & Networks

## Input
```bash
docker compose down
```

---

# Rebuild Images

## Input
```bash
docker compose up --build
```

---

# Task 5 – Environment Variables

# Add Variables Directly

## Example
```yaml
environment:
  MYSQL_ROOT_PASSWORD: rootpassword
```

---

# Create .env File

## .env
```env
MYSQL_ROOT_PASSWORD=rootpassword
MYSQL_DATABASE=wordpress
MYSQL_USER=wpuser
MYSQL_PASSWORD=wppassword
```

---

# Reference Variables in Compose File

## docker-compose.yml
```yaml
environment:
  MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
  MYSQL_DATABASE: ${MYSQL_DATABASE}
```

---

# Verify Variables

## Input
```bash
docker compose config
```

### Observation
✅ Variables loaded successfully from `.env`

---

# Important Docker Compose Concepts

| Concept | Purpose |
|----------|----------|
| services | Define containers |
| volumes | Persistent storage |
| networks | Container communication |
| environment | Environment variables |
| depends_on | Service dependency |

---

# Commands Used

```bash
docker compose up
docker compose up -d
docker compose down
docker compose stop
docker compose ps
docker compose logs
docker compose logs -f
docker compose config
```

---

# What I Learned

- Docker Compose manages multi-container applications easily.
- Compose automatically creates networks.
- Service names work as DNS names.
- Volumes persist database data safely.
- `.env` files simplify configuration management.

---

# Why Docker Compose Matters in DevOps

Docker Compose is important because:
- Real applications use multiple services
- Simplifies local development environments
- Makes deployments reproducible
- Useful for testing microservices locally

Compose is widely used before moving applications to Kubernetes.

---

# Final Summary

Today I practiced:
- Writing Docker Compose files
- Running multi-container applications
- Managing WordPress + MySQL setup
- Using named volumes
- Docker networking with service discovery
- Environment variable management

This challenge helped me understand how modern multi-container applications are managed using Docker Compose.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
