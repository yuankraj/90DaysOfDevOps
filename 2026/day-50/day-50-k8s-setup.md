# Day 50 – Kubernetes Architecture and Cluster Setup

---

# 📌 Task Overview

Today's goal:
- Understand Kubernetes architecture
- Set up a local cluster (kind)
- Run kubectl commands
- Explore system components

---

# ✅ Task 1: Kubernetes Story

### Why Kubernetes?
Kubernetes was created to manage containers at scale. Docker can run containers on a single machine, but Kubernetes manages containers across multiple machines with features like scaling, self-healing, and load balancing.

### Who created Kubernetes?
Kubernetes was created by Google and inspired by their internal system called Borg.

### Meaning of Kubernetes
"Kubernetes" means **Helmsman (Ship Pilot)**, which represents container orchestration.

---

# ✅ Task 2: Kubernetes Architecture

## Control Plane (Master Node)

- **API Server**
  Entry point of cluster — all kubectl commands go through it

- **etcd**
  Stores cluster state (database)

- **Scheduler**
  Assigns pods to nodes

- **Controller Manager**
  Maintains desired state

---

## Worker Node

- **kubelet**
  Agent that runs pods

- **kube-proxy**
  Handles networking

- **Container Runtime**
  Runs containers (containerd, CRI-O)

---

## 🔁 Flow: kubectl apply

```
kubectl → API Server → etcd → Scheduler → kubelet → Container runs
```

---

## Failure Scenarios

- API Server Down → Cluster unusable  
- Worker Node Down → Pods rescheduled  

---

# ✅ Task 3: Install kubectl

## Command

```bash
kubectl version --client
```

## Output Example

```
Client Version: v1.28.x
```

---

# ✅ Task 4: Setup Local Cluster

## Tool Chosen: kind

### Why?
- Lightweight
- Fast
- Uses Docker

---

## Install kind

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

---

## Create Cluster

```bash
kind create cluster --name devops-cluster
```

---

## Verify Cluster

```bash
kubectl cluster-info
kubectl get nodes
```

### Output

```
NAME                 STATUS   ROLES           AGE   VERSION
kind-control-plane   Ready    control-plane   1m    v1.xx.x
```

---

# ✅ Task 5: Explore Cluster

## Commands

```bash
kubectl cluster-info
kubectl get nodes
kubectl describe node kind-control-plane
kubectl get namespaces
kubectl get pods -A
kubectl get pods -n kube-system
```

---

## kube-system Pods Mapping

| Component | Pod |
|----------|-----|
| API Server | kube-apiserver |
| Database | etcd |
| Scheduler | kube-scheduler |
| Controller | kube-controller-manager |
| DNS | coredns |
| Networking | kube-proxy |

---

# ✅ Task 6: Cluster Lifecycle

## Delete Cluster

```bash
kind delete cluster --name devops-cluster
```

## Recreate Cluster

```bash
kind create cluster --name devops-cluster
kubectl get nodes
```

---

# ✅ kubeconfig

### What is kubeconfig?

File that stores cluster connection details.

### Location

```
~/.kube/config
```

---

## Commands

```bash
kubectl config current-context
kubectl config get-contexts
kubectl config view
```

---

# 📸 Screenshots (Add Here)

- kubectl get nodes → `k8s-nodes.png`
- kubectl get pods -n kube-system → `kube-system.png`

---

# 💡 What I Learned

1. Kubernetes manages containers across multiple nodes  
2. Control plane manages cluster state  
3. Worker nodes run applications  

---

# 🚀 Summary

Today I set up my first Kubernetes cluster using kind, explored its architecture, and understood how different components work together to manage containerized workloads.

This is the foundation of container orchestration.

---

# 🧠 Interview Quick Answer

**What happens when you run kubectl apply?**

```
kubectl → API Server → etcd → Scheduler → kubelet → Container runs
```

---

# ✅ Commands Used (Quick Reference)

```bash
kubectl version --client
kind create cluster --name devops-cluster
kubectl cluster-info
kubectl get nodes
kubectl get pods -A
kubectl get pods -n kube-system
kubectl config view
kind delete cluster --name devops-cluster
```
