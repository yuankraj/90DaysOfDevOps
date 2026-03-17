# Day 53 – Kubernetes Services

---

# 📌 Task Overview

Today I learned:
- Why Services are needed
- ClusterIP, NodePort, LoadBalancer
- Service discovery using DNS
- Load balancing between pods

---

# ❓ Why Services?

Problem:
- Pod IPs are **not stable**
- Deployment has **multiple pods**

Solution:
- Service provides **stable IP + DNS**
- Load balances traffic

---

# ✅ Task 1: Deployment

## YAML (app-deployment.yaml)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
  labels:
    app: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
      - name: nginx
        image: nginx:1.25
        ports:
        - containerPort: 80
```

---

## Apply Deployment

```bash
kubectl apply -f app-deployment.yaml
kubectl get pods -o wide
```

### Observation

- 3 pods running  
- Each has different IP  

---

# ✅ Task 2: ClusterIP Service

## YAML (clusterip-service.yaml)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-app-clusterip
spec:
  type: ClusterIP
  selector:
    app: web-app
  ports:
  - port: 80
    targetPort: 80
```

---

## Apply

```bash
kubectl apply -f clusterip-service.yaml
kubectl get services
```

---

## Test Inside Cluster

```bash
kubectl run test-client --image=busybox --rm -it --restart=Never -- sh

wget -qO- http://web-app-clusterip
exit
```

### Observation

- Service works  
- Load balances traffic  

---

# ✅ Task 3: DNS Discovery

```bash
kubectl run dns-test --image=busybox --rm -it --restart=Never -- sh

wget -qO- http://web-app-clusterip
wget -qO- http://web-app-clusterip.default.svc.cluster.local

nslookup web-app-clusterip
exit
```

### Observation

- DNS resolves to ClusterIP  

---

# ✅ Task 4: NodePort Service

## YAML (nodeport-service.yaml)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-app-nodeport
spec:
  type: NodePort
  selector:
    app: web-app
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30080
```

---

## Apply

```bash
kubectl apply -f nodeport-service.yaml
kubectl get services
```

---

## Access

```bash
curl http://localhost:30080
```

### Observation

- Nginx page accessible externally  

---

# ✅ Task 5: LoadBalancer Service

## YAML (loadbalancer-service.yaml)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-app-loadbalancer
spec:
  type: LoadBalancer
  selector:
    app: web-app
  ports:
  - port: 80
    targetPort: 80
```

---

## Apply

```bash
kubectl apply -f loadbalancer-service.yaml
kubectl get services
```

---

### Observation

- EXTERNAL-IP = pending (local cluster)  

---

# ✅ Task 6: Compare Services

```bash
kubectl get services -o wide
```

---

## Comparison Table

| Type | Access | Use |
|------|-------|-----|
| ClusterIP | Internal | Service-to-service |
| NodePort | NodeIP:Port | Dev/testing |
| LoadBalancer | External IP | Production |

---

## Verify LoadBalancer

```bash
kubectl describe service web-app-loadbalancer
```

### Observation

- Has ClusterIP  
- Has NodePort  
- External IP pending  

---

# ✅ Task 7: Cleanup

```bash
kubectl delete -f app-deployment.yaml
kubectl delete -f clusterip-service.yaml
kubectl delete -f nodeport-service.yaml
kubectl delete -f loadbalancer-service.yaml

kubectl get pods
kubectl get services
```

---

# 📸 Screenshots

Add:

- kubectl get services → services.png  
- curl output → output.png  

---

# 💡 What I Learned

1. Services give stable network access  
2. ClusterIP is internal only  
3. NodePort exposes app externally  
4. LoadBalancer used in cloud  
5. DNS helps service discovery  

---

# 🚀 Key Concepts

## Service
Stable network endpoint

## Selector
Matches pods

## Endpoint
Actual pod IPs

---

# 🧠 Interview Answers

### Why Services?

```
Pods are dynamic → Services provide stable access
```

---

### ClusterIP?

```
Internal communication
```

---

### NodePort?

```
External via NodeIP:Port
```

---

### LoadBalancer?

```
Cloud external access
```

---

### What are Endpoints?

```
Pod IPs behind a service
```

---

# 🚀 Summary

Today I learned how Kubernetes Services provide stable communication and load balancing across pods. I implemented ClusterIP, NodePort, and LoadBalancer services and tested connectivity.

---

# ⚡ Commands Used

```bash
kubectl apply -f app-deployment.yaml
kubectl apply -f clusterip-service.yaml
kubectl apply -f nodeport-service.yaml
kubectl apply -f loadbalancer-service.yaml

kubectl get pods -o wide
kubectl get services -o wide

kubectl run test-client --image=busybox --rm -it --restart=Never -- sh
wget -qO- http://web-app-clusterip

kubectl describe service web-app-loadbalancer
```
