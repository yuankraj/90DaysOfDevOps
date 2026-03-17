# Day 51 – Kubernetes Manifests and Your First Pods

---

# 📌 Task Overview

Today I learned:
- Kubernetes manifest structure
- Creating Pods using YAML
- Imperative vs Declarative approach
- Labels and filtering

---

# ✅ Kubernetes Manifest Structure

Every Kubernetes manifest has 4 required fields:

### 1. apiVersion
Defines API version (for Pod → v1)

### 2. kind
Defines resource type (Pod)

### 3. metadata
Defines name and labels

### 4. spec
Defines actual container configuration

---

# ✅ Task 1: Nginx Pod

## File: nginx-pod.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

## Commands Used

```bash
kubectl apply -f nginx-pod.yaml
kubectl get pods
kubectl describe pod nginx-pod
kubectl logs nginx-pod
kubectl exec -it nginx-pod -- /bin/sh
```

## Observation

- Pod status → Running
- Nginx server running inside container

---

# ✅ Task 2: BusyBox Pod

## File: busybox-pod.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox-pod
  labels:
    app: busybox
    environment: dev
spec:
  containers:
  - name: busybox
    image: busybox:latest
    command: ["sh", "-c", "echo Hello from BusyBox && sleep 3600"]
```

## Commands

```bash
kubectl apply -f busybox-pod.yaml
kubectl logs busybox-pod
```

## Observation

- Output: Hello from BusyBox
- Pod stays running because of sleep command

---

# ✅ Task 3: Third Pod (Custom Labels)

## File: custom-pod.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: custom-pod
  labels:
    app: demo
    environment: test
    team: devops
spec:
  containers:
  - name: nginx
    image: nginx:latest
```

## Commands

```bash
kubectl apply -f custom-pod.yaml
kubectl get pods --show-labels
kubectl get pods -l app=demo
kubectl get pods -l environment=test
```

---

# ✅ Task 4: Imperative vs Declarative

### Imperative

```bash
kubectl run redis-pod --image=redis:latest
```

### Declarative

```bash
kubectl apply -f nginx-pod.yaml
```

### Difference

| Imperative | Declarative |
|-----------|------------|
| Direct command | YAML based |
| Quick testing | Production use |
| Hard to track | Version controlled |

---

# ✅ Task 5: Dry Run Validation

```bash
kubectl apply -f nginx-pod.yaml --dry-run=client
kubectl apply -f nginx-pod.yaml --dry-run=server
```

## Error Example

If image is removed:
```
error: spec.containers[0].image: Required value
```

---

# ✅ Task 6: Labels Practice

```bash
kubectl get pods --show-labels
kubectl label pod nginx-pod environment=production
kubectl label pod nginx-pod environment-
```

---

# ✅ Task 7: Cleanup

```bash
kubectl delete pod nginx-pod
kubectl delete pod busybox-pod
kubectl delete pod redis-pod
kubectl delete pod custom-pod
```

---

# 📸 Screenshot

Add:

- kubectl get pods → `pods.png`

---

# 💡 What I Learned

1. Pods are the smallest unit in Kubernetes  
2. YAML manifests define desired state  
3. Labels help organize and filter resources  

---

# 🚀 Summary

Today I created my first Kubernetes Pods using YAML manifests. I learned how to deploy containers, view logs, execute inside pods, and manage resources using kubectl.

---

# 🧠 Interview Answer

**What happens when you delete a Pod?**

```
Standalone Pod → permanently deleted
(No auto-recreation)
```

(Deployments fix this in real-world usage)
