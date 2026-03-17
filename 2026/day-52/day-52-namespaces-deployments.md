# Day 52 – Kubernetes Namespaces and Deployments

---

# 📌 Task Overview

Today I learned:
- Namespaces (environment isolation)
- Deployments (self-healing pods)
- Scaling and rolling updates

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

---

## Check kube-system Pods

```bash
kubectl get pods -n kube-system
```

### Observation

These are Kubernetes internal components (API server, scheduler, etc.)

---

# ✅ Task 2: Create Namespaces

## Commands

```bash
kubectl create namespace dev
kubectl create namespace staging
```

---

## YAML (Namespace Manifest)

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: production
```

Apply:

```bash
kubectl apply -f namespace.yaml
```

---

## Run Pods in Namespaces

```bash
kubectl run nginx-dev --image=nginx -n dev
kubectl run nginx-staging --image=nginx -n staging
```

---

## Verify

```bash
kubectl get pods
kubectl get pods -A
```

### Observation

- Default → only default namespace  
- -A → all namespaces  

---

# ✅ Task 3: Deployment

## YAML (Deployment Manifest)

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

- READY → running pods  
- UP-TO-DATE → updated pods  
- AVAILABLE → available pods  

---

# ✅ Task 4: Self-Healing

```bash
kubectl get pods -n dev
kubectl delete pod <pod-name> -n dev
kubectl get pods -n dev
```

### Observation

- New pod created automatically  
- Pod name is different  

---

# ✅ Task 5: Scaling

## Scale Up

```bash
kubectl scale deployment nginx-deployment --replicas=5 -n dev
```

## Scale Down

```bash
kubectl scale deployment nginx-deployment --replicas=2 -n dev
```

### Observation

Extra pods are removed automatically  

---

# ✅ Task 6: Rolling Update

## Update Image

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.25 -n dev
```

---

## Check Rollout

```bash
kubectl rollout status deployment/nginx-deployment -n dev
```

---

## Rollback

```bash
kubectl rollout undo deployment/nginx-deployment -n dev
```

---

## Verify Image

```bash
kubectl describe deployment nginx-deployment -n dev | grep Image
```

### Observation

Image reverted back to previous version  

---

# ✅ Task 7: Cleanup

```bash
kubectl delete deployment nginx-deployment -n dev
kubectl delete pod nginx-dev -n dev
kubectl delete pod nginx-staging -n staging
kubectl delete namespace dev staging production
```

---

# 📸 Screenshots

Add:

- kubectl get deployments → deployments.png  
- kubectl get pods -A → pods-all.png  

---

# 💡 What I Learned

1. Namespaces isolate environments  
2. Deployments provide self-healing  
3. Scaling adjusts number of pods  
4. Rolling updates ensure zero downtime  

---

# 🚀 Key Concepts

## Namespace
Logical separation inside cluster

## Deployment
Manages pods automatically

---

# 🧠 Interview Answers

### What happens when a pod is deleted?

Deployment recreates it automatically  

---

### What happens to standalone pod?

It is permanently deleted  

---

### What is scaling?

Increasing or decreasing number of pods  

---

### What is rolling update?

Updating pods one by one without downtime  

---

# 🚀 Summary

Today I learned how Kubernetes Deployments manage pods automatically. I created namespaces, scaled applications, and performed rolling updates with rollback.

---

# ⚡ Commands Used

```bash
kubectl get namespaces
kubectl create namespace dev
kubectl create namespace staging
kubectl apply -f namespace.yaml
kubectl run nginx-dev --image=nginx -n dev
kubectl run nginx-staging --image=nginx -n staging
kubectl apply -f nginx-deployment.yaml
kubectl get deployments -n dev
kubectl get pods -A
kubectl scale deployment nginx-deployment --replicas=5 -n dev
kubectl scale deployment nginx-deployment --replicas=2 -n dev
kubectl set image deployment/nginx-deployment nginx=nginx:1.25 -n dev
kubectl rollout undo deployment/nginx-deployment -n dev
```
