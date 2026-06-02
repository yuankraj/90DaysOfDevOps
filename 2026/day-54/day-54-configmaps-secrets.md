# Day 54 – Kubernetes ConfigMaps and Secrets

## Objective

Today I learned how Kubernetes manages application configuration using ConfigMaps and Secrets. Instead of hardcoding configuration values inside container images, Kubernetes allows applications to consume configuration dynamically through environment variables and mounted files.

---

# What are ConfigMaps?

A ConfigMap is a Kubernetes object used to store non-sensitive configuration data.

Examples:

* Application environment
* Feature flags
* Ports
* URLs
* Configuration files

ConfigMaps help separate configuration from application code.

---

# What are Secrets?

A Secret is a Kubernetes object used to store sensitive information.

Examples:

* Database passwords
* API keys
* Tokens
* Certificates

Secrets are stored as Base64 encoded values and can be consumed securely by Pods.

---

# ConfigMap vs Secret

| Feature               | ConfigMap            | Secret               |
| --------------------- | -------------------- | -------------------- |
| Purpose               | Non-sensitive config | Sensitive data       |
| Storage               | Plain text           | Base64 encoded       |
| Use Cases             | App settings         | Passwords & API Keys |
| Volume Mount          | Yes                  | Yes                  |
| Environment Variables | Yes                  | Yes                  |

---

# Task 1 – ConfigMap from Literals

## Create ConfigMap

```bash
kubectl create configmap app-config \
--from-literal=APP_ENV=production \
--from-literal=APP_DEBUG=false \
--from-literal=APP_PORT=8080
```

---

## Verify

```bash
kubectl describe configmap app-config
```

```bash
kubectl get configmap app-config -o yaml
```

Example Output:

```yaml
data:
  APP_ENV: production
  APP_DEBUG: "false"
  APP_PORT: "8080"
```

---

# Observation

ConfigMap values are stored as plain text.

No encryption or encoding is used.

---

# Task 2 – ConfigMap from File

## Create Nginx Configuration

### default.conf

```nginx
server {
    listen 80;

    location /health {
        return 200 "healthy";
    }
}
```

---

## Create ConfigMap

```bash
kubectl create configmap nginx-config \
--from-file=default.conf=default.conf
```

---

## Verify

```bash
kubectl get configmap nginx-config -o yaml
```

Output contains:

```yaml
data:
  default.conf: |
    server {
      listen 80;
      location /health {
        return 200 "healthy";
      }
    }
```

---

# Task 3 – Using ConfigMaps in Pods

## Environment Variables Example

### busybox-configmap-pod.yaml

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: busybox-configmap

spec:
  containers:
  - name: busybox
    image: busybox

    command:
      - sh
      - -c
      - env && sleep 3600

    envFrom:
    - configMapRef:
        name: app-config
```

---

## Apply

```bash
kubectl apply -f busybox-configmap-pod.yaml
```

---

## Verify

```bash
kubectl logs busybox-configmap
```

Expected Variables:

```bash
APP_ENV=production
APP_DEBUG=false
APP_PORT=8080
```

---

# Mount ConfigMap as Volume

### nginx-configmap-pod.yaml

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx-configmap

spec:
  containers:
  - name: nginx
    image: nginx

    volumeMounts:
    - name: nginx-config
      mountPath: /etc/nginx/conf.d

  volumes:
  - name: nginx-config
    configMap:
      name: nginx-config
```

---

## Verify

```bash
kubectl exec nginx-configmap -- \
curl -s http://localhost/health
```

Output:

```text
healthy
```

---

# Task 4 – Create Secret

## Create Secret

```bash
kubectl create secret generic db-credentials \
--from-literal=DB_USER=admin \
--from-literal=DB_PASSWORD=s3cureP@ssw0rd
```

---

## Inspect Secret

```bash
kubectl get secret db-credentials -o yaml
```

Example:

```yaml
data:
  DB_USER: YWRtaW4=
  DB_PASSWORD: czNjdXJlUEBzc3cwcmQ=
```

---

# Decode Secret

```bash
echo 'YWRtaW4=' | base64 --decode
```

Output:

```text
admin
```

---

# Important Observation

Base64 is NOT encryption.

It is only encoding.

Anyone with access can decode the value.

---

# Why Secrets Still Matter

Benefits:

* RBAC access control
* Separate management
* Optional encryption at rest
* Mounted using tmpfs memory

---

# Task 5 – Using Secrets in Pods

## Secret Environment Variable

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: secret-pod

spec:
  containers:
  - name: busybox
    image: busybox

    command:
      - sh
      - -c
      - env && sleep 3600

    env:
    - name: DB_USER
      valueFrom:
        secretKeyRef:
          name: db-credentials
          key: DB_USER
```

---

# Mount Secret as Volume

```yaml
volumeMounts:
- name: secret-volume
  mountPath: /etc/db-credentials
  readOnly: true

volumes:
- name: secret-volume
  secret:
    secretName: db-credentials
```

---

## Verify

```bash
kubectl exec secret-pod -- ls /etc/db-credentials
```

Output:

```text
DB_USER
DB_PASSWORD
```

---

## Read Secret File

```bash
kubectl exec secret-pod -- \
cat /etc/db-credentials/DB_PASSWORD
```

Output:

```text
s3cureP@ssw0rd
```

Mounted files contain plaintext values.

They are NOT Base64 encoded.

---

# Task 6 – Live ConfigMap Updates

## Create ConfigMap

```bash
kubectl create configmap live-config \
--from-literal=message=hello
```

---

## Pod Manifest

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: live-config-pod

spec:
  containers:
  - name: busybox
    image: busybox

    command:
      - sh
      - -c
      - |
        while true
        do
          cat /config/message
          sleep 5
        done

    volumeMounts:
    - name: config-volume
      mountPath: /config

  volumes:
  - name: config-volume
    configMap:
      name: live-config
```

---

## Update ConfigMap

```bash
kubectl patch configmap live-config \
--type merge \
-p '{"data":{"message":"world"}}'
```

---

## Observation

After approximately 30–60 seconds:

```text
hello
hello
hello
world
world
world
```

Volume-mounted ConfigMaps update automatically.

---

# Important Difference

| Method                | Auto Updates |
| --------------------- | ------------ |
| Environment Variables | No           |
| Volume Mounts         | Yes          |

Environment variables require Pod restart.

Volumes update automatically.

---

# Cleanup

Delete Pods:

```bash
kubectl delete pod busybox-configmap
kubectl delete pod nginx-configmap
kubectl delete pod secret-pod
kubectl delete pod live-config-pod
```

Delete ConfigMaps:

```bash
kubectl delete configmap app-config
kubectl delete configmap nginx-config
kubectl delete configmap live-config
```

Delete Secret:

```bash
kubectl delete secret db-credentials
```

---

# Commands Practiced

```bash
kubectl create configmap

kubectl create secret

kubectl describe configmap

kubectl get configmap -o yaml

kubectl get secret -o yaml

kubectl patch configmap

kubectl exec

kubectl logs

base64 --decode
```

---

# Key Learnings

1. ConfigMaps store non-sensitive configuration.
2. Secrets store sensitive information.
3. Base64 encoding is not encryption.
4. Environment variables are loaded once at Pod startup.
5. Volume-mounted ConfigMaps automatically update.
6. Volume-mounted Secrets appear as plaintext files.

---

# Screenshots to Add

### ConfigMap

```bash
kubectl get configmap
```

### Secret

```bash
kubectl get secret
```

### Mounted Config

```bash
curl localhost/health
```

### Live Update

```bash
kubectl logs live-config-pod
```

---

# Conclusion

Today I learned how Kubernetes separates application configuration from application code using ConfigMaps and Secrets. I created ConfigMaps from literals and files, mounted them as environment variables and volumes, worked with Secrets securely, and observed how ConfigMap updates automatically propagate through mounted volumes.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
