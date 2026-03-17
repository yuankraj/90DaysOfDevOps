# Day 52 – Kubernetes Namespaces and Deployments

---

# 📌 Task Overview

Today I learned:
- Kubernetes Namespaces (isolation)
- Deployments (self-healing + scaling)
- Rolling updates and rollbacks

---

# ✅ Task 1: Default Namespaces

## Command

```bash
kubectl get namespaces
```

## Output

- default
- kube-system
- kube-public
- kube-node-lease

## kube-system Pods

```bash
kubectl get pods -n kube-system
```

These pods run core Kubernetes components like:
- API server
- scheduler
- etcd

---

# ✅ Task 2: Create Namespaces

## Commands

```bash
kubectl create namespace dev
kubectl create namespace staging
```

## Verify

```bash
kubectl get namespaces
```

---

## Run Pods in Namespaces

```bash
kubectl run nginx-dev --image=nginx:latest -n dev
kubectl run nginx-staging --image=nginx:latest -n staging
```

---

## Check Pods

```bash
kubectl get pods
kubectl get pods -A
```

### Observation

- `kubectl get pods` → shows default namespace only  
- `kubectl get pods -A` → shows all namespaces  

---

# ✅ Task 3: Deployment

## File: nginx-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: dev
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.24
        ports:
        - containerPort: 80
```

---

## Apply Deployment

```bash
kubectl apply -f nginx-deployment.yaml
kubectl get deployments -n dev
kubectl get pods -n dev
```

---

## Output Meaning

- READY → pods ready
- UP-TO-DATE → updated pods
- AVAILABLE → running pods

---

# ✅ Task 4: Self-Healing

## Delete a Pod

```bash
kubectl get pods -n dev
kubectl delete pod <pod-name> -n dev
kubectl get pods -n dev
```

## Observation

- New pod created automatically  
- Name is different  

---

# ✅ Task 5: Scaling

## Scale Up

```bash
kubectl scale deployment nginx-deployment --replicas=5 -n dev
kubectl get pods -n dev
```

## Scale Down

```bash
kubectl scale deployment nginx-deployment --replicas=2 -n dev
kubectl get pods -n dev
```

## Observation

- Extra pods are terminated when scaling down  

---

# ✅ Task 6: Rolling Update

## Update Image

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.25 -n dev
```

## Check Rollout

```bash
kubectl rollout status deployment/nginx-deployment -n dev
```

---

## Rollback

```bash
kubectl rollout undo deployment/nginx-deployment -n dev
kubectl rollout status deployment/nginx-deployment -n dev
```

---

## Verify Image

```bash
kubectl describe deployment nginx-deployment -n dev | grep Image
```

### Observation

Image reverted to previous version (nginx:1.24)

---

# ✅ Task 7: Cleanup

```bash
kubectl delete deployment nginx-deployment -n dev
kubectl delete pod nginx-dev -n dev
kubectl delete pod nginx-staging -n staging
kubectl delete namespace dev staging
```

---

# 📸 Screenshots

Add:

- kubectl get deployments → `deployments.png`
- kubectl get pods -A → `pods-all.png`

---

# 💡 What I Learned

1. Namespaces isolate environments (dev, staging, prod)  
2. Deployments provide self-healing and scaling  
3. Rolling updates allow zero downtime deployments  

---

# 🚀 Key Concepts

## Namespace

Logical isolation inside cluster

---

## Deployment

Manages pods automatically

---

## Pod vs Deployment

| Pod | Deployment |
|-----|-----------|
| Manual | Automated |
| No restart | Self-healing |
| Single instance | Multiple replicas |

---

# 🧠 Interview Answers

### What happens if a pod is deleted in Deployment?

```
New pod is automatically created
```

---

### What happens if a standalone pod is deleted?

```
Pod is permanently gone
```

---

### What is scaling?

```
Increasing/decreasing number of pods
```

---

### What is rolling update?

```
Updating pods one by one with zero downtime
```

---

# 🚀 Summary

Today I learned how Kubernetes Deployments manage pods automatically. I created namespaces, deployed applications, scaled them, and performed rolling updates with rollback.

This is how real-world production systems run in Kubernetes.
