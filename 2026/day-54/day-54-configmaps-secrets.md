# Day 54 – Kubernetes ConfigMaps and Secrets

## 📌 Overview

Applications need configuration like database URLs, API keys, and feature flags. Hardcoding these values inside container images is not recommended because it requires rebuilding images for every change.

Kubernetes provides:

- **ConfigMaps** → for non-sensitive configuration
- **Secrets** → for sensitive data

---

## 🔹 Task 1: Create a ConfigMap from Literals

### Command

kubectl create configmap app-config \
  --from-literal=APP_ENV=production \
  --from-literal=APP_DEBUG=false \
  --from-literal=APP_PORT=8080

### Verify

kubectl describe configmap app-config  
kubectl get configmap app-config -o yaml  

### ✅ Output

- All key-value pairs are visible  
- Stored as plain text (no encryption)

---

## 🔹 Task 2: Create a ConfigMap from File

### Nginx Config (default.conf)

server {
    listen 80;

    location /health {
        return 200 'healthy';
        add_header Content-Type text/plain;
    }
}

### Create ConfigMap

kubectl create configmap nginx-config \
  --from-file=default.conf=default.conf

### Verify

kubectl get configmap nginx-config -o yaml

### ✅ Output

- File content stored inside ConfigMap  
- Key becomes filename when mounted  

---

## 🔹 Task 3: Use ConfigMaps in Pods

### Pod using Environment Variables

apiVersion: v1
kind: Pod
metadata:
  name: config-env-pod
spec:
  containers:
  - name: busybox
    image: busybox
    command: ["sh", "-c", "env && sleep 3600"]
    envFrom:
    - configMapRef:
        name: app-config

---

### Pod using Volume Mount

apiVersion: v1
kind: Pod
metadata:
  name: nginx-config-pod
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: config-volume
      mountPath: /etc/nginx/conf.d
  volumes:
  - name: config-volume
    configMap:
      name: nginx-config

### Test

kubectl exec nginx-config-pod -- curl -s http://localhost/health

### ✅ Output

- Env variables injected  
- Config file mounted  
- `/health` returns: healthy  

---

## 🔹 Task 4: Create a Secret

### Command

kubectl create secret generic db-credentials \
  --from-literal=DB_USER=admin \
  --from-literal=DB_PASSWORD=s3cureP@ssw0rd

### Verify

kubectl get secret db-credentials -o yaml

### Decode

echo '<base64-value>' | base64 --decode

### ✅ Output

- Values are base64 encoded  
- Can be decoded easily  

---

## 🔹 Task 5: Use Secrets in Pod

apiVersion: v1
kind: Pod
metadata:
  name: secret-pod
spec:
  containers:
  - name: busybox
    image: busybox
    command: ["sh", "-c", "env && sleep 3600"]
    env:
    - name: DB_USER
      valueFrom:
        secretKeyRef:
          name: db-credentials
          key: DB_USER
    volumeMounts:
    - name: secret-volume
      mountPath: /etc/db-credentials
      readOnly: true
  volumes:
  - name: secret-volume
    secret:
      secretName: db-credentials

### ✅ Output

- Env variable injected  
- Secret mounted as files  
- Files contain plaintext values  

---

## 🔹 Task 6: Update ConfigMap (Live Update)

### Create

kubectl create configmap live-config --from-literal=message=hello

### Pod

apiVersion: v1
kind: Pod
metadata:
  name: live-config-pod
spec:
  containers:
  - name: busybox
    image: busybox
    command: ["sh", "-c", "while true; do cat /config/message; sleep 5; done"]
    volumeMounts:
    - name: config-volume
      mountPath: /config
  volumes:
  - name: config-volume
    configMap:
      name: live-config

### Update

kubectl patch configmap live-config \
  --type merge \
  -p '{"data":{"message":"world"}}'

### ✅ Output

- Value updates automatically  
- No pod restart needed  
- Env vars do NOT update  

---

## 🔹 Task 7: Cleanup

kubectl delete pod config-env-pod nginx-config-pod secret-pod live-config-pod  
kubectl delete configmap app-config nginx-config live-config  
kubectl delete secret db-credentials  

---

## 📘 Key Concepts

### 🔹 ConfigMaps vs Secrets

| Feature | ConfigMap | Secret |
|--------|----------|--------|
| Data | Non-sensitive | Sensitive |
| Storage | Plain text | Base64 encoded |
| Usage | Config | Passwords, keys |

---

### 🔹 Env Variables vs Volume Mounts

| Feature | Env Vars | Volume Mount |
|--------|---------|-------------|
| Updates | No | Yes |
| Use Case | Simple values | Files |
| Restart Needed | Yes | No |

---

### 🔹 Base64

- Not encryption  
- Easily decoded  
- Used only for encoding  

---

## 🚀 Summary

- Used ConfigMaps for configuration  
- Used Secrets for sensitive data  
- Injected configs using env vars and volumes  
- Learned base64 is NOT secure encryption  
- Observed live updates in mounted ConfigMaps  

---

## 📢 Learn in Public

Learned Kubernetes ConfigMaps and Secrets today. Injected config as environment variables and volume mounts, and discovered that base64 encoding is not encryption.

---

#90DaysOfDevOps #DevOpsKaJosh #TrainWithShubham
