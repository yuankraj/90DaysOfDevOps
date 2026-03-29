# Day 54 – Kubernetes ConfigMaps and Secrets

## 📌 Overview

Applications require configuration such as database URLs, API keys, and feature flags. Hardcoding these values inside container images is not recommended because it requires rebuilding images whenever values change.

Kubernetes solves this problem using:

* **ConfigMaps** → for non-sensitive configuration
* **Secrets** → for sensitive data

---

# 🔹 Task 1: Create ConfigMap from Literals

## Command

```bash
kubectl create configmap app-config \
  --from-literal=APP_ENV=production \
  --from-literal=APP_DEBUG=false \
  --from-literal=APP_PORT=8080
```

## Verify

```bash
kubectl describe configmap app-config
kubectl get configmap app-config -o yaml
```

## ✅ Output

* All key-value pairs are visible
* Stored as plain text (no encryption)

---

# 🔹 Task 2: Create ConfigMap from File

## Step 1: Create File

```bash
nano default.conf
```

Paste:

```nginx
server {
    listen 80;

    location /health {
        return 200 'healthy';
        add_header Content-Type text/plain;
    }
}
```

## Step 2: Create ConfigMap

```bash
kubectl create configmap nginx-config \
  --from-file=default.conf=default.conf
```

## Verify

```bash
kubectl get configmap nginx-config -o yaml
```

## ✅ Output

* File content stored inside ConfigMap
* Key becomes filename when mounted

---

# 🔹 Task 3: Use ConfigMaps in Pods

## Pod using Environment Variables

```yaml
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
```

## Apply & Check Logs

```bash
kubectl apply -f config-env-pod.yaml
kubectl logs config-env-pod
```

---

## Pod using Volume Mount (Nginx)

```yaml
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
```

## Apply

```bash
kubectl apply -f nginx-config-pod.yaml
```

## Test

```bash
kubectl exec nginx-config-pod -- curl -s http://localhost/health
```

## ✅ Output

```text
healthy
```

---

# 🔹 Task 4: Create Secret

## Command

```bash
kubectl create secret generic db-credentials \
  --from-literal=DB_USER=admin \
  --from-literal=DB_PASSWORD=s3cureP@ssw0rd
```

## Verify

```bash
kubectl get secret db-credentials -o yaml
```

## Decode

```bash
echo '<base64-value>' | base64 --decode
```

## ✅ Output

* Values are base64 encoded
* Easily decoded to plaintext

---

# 🔹 Task 5: Use Secrets in Pod

```yaml
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
```

## Apply

```bash
kubectl apply -f secret-pod.yaml
```

## Verify

```bash
kubectl exec secret-pod -- env | grep DB_USER
kubectl exec secret-pod -- cat /etc/db-credentials/DB_PASSWORD
```

## ✅ Output

* Environment variable injected
* Secret mounted as files
* File contents are plaintext (auto-decoded)

---

# 🔹 Task 6: Update ConfigMap and Observe Changes

## Create ConfigMap

```bash
kubectl create configmap live-config --from-literal=message=hello
```

## Pod

```yaml
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
```

## Apply & Logs

```bash
kubectl apply -f live-config-pod.yaml
kubectl logs -f live-config-pod
```

## Update ConfigMap

```bash
kubectl patch configmap live-config \
  --type merge \
  -p '{"data":{"message":"world"}}'
```

## ✅ Output

* Value updates automatically (30–60 seconds)
* No pod restart required
* Environment variables do NOT update

---

# 🔹 Task 7: Clean Up

```bash
kubectl delete pod config-env-pod nginx-config-pod secret-pod live-config-pod
kubectl delete configmap app-config nginx-config live-config
kubectl delete secret db-credentials
```

---

# 📘 Key Concepts

## 🔹 What are ConfigMaps and Secrets?

* **ConfigMaps** store non-sensitive configuration data
* **Secrets** store sensitive data like passwords and API keys

---

## 🔹 Environment Variables vs Volume Mounts

| Feature          | Environment Variables | Volume Mounts     |
| ---------------- | --------------------- | ----------------- |
| Updates          | No                    | Yes               |
| Use Case         | Simple key-value      | Full config files |
| Restart Required | Yes                   | No                |

---

## 🔹 Why Base64 is NOT Encryption

* Base64 is just encoding
* It does NOT secure data
* Anyone can decode it easily

---

## 🔹 ConfigMap Update Behavior

* Volume-mounted ConfigMaps update automatically
* Environment variables remain static after pod startup

---

# 🚀 Summary

* Created ConfigMaps from literals and files
* Injected configuration using env variables and volumes
* Created and used Secrets securely
* Verified base64 decoding
* Observed live updates in ConfigMaps

---

# 📢 Learn in Public

Learned Kubernetes ConfigMaps and Secrets today. Injected config as environment variables and volume mounts, and discovered that base64 encoding is not encryption.

---

#90DaysOfDevOps #DevOpsKaJosh #TrainWithShubham
