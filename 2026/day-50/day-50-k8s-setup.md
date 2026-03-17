# Day 50 – Kubernetes Architecture and Cluster Setup

## Kubernetes Story

Kubernetes was created to manage containers at scale. While Docker can run containers on a single machine, Kubernetes helps manage hundreds of containers across multiple servers, ensuring high availability, scaling, and automation.

Kubernetes was originally developed by Google and inspired by their internal system called Borg. The name "Kubernetes" means "helmsman" or "ship pilot", representing container orchestration.

---

## Kubernetes Architecture

### Control Plane (Master Node)

- **API Server**  
  Entry point of the cluster. All commands (kubectl) go through it.

- **etcd**  
  Key-value database storing cluster state.

- **Scheduler**  
  Assigns pods to nodes based on resources.

- **Controller Manager**  
  Maintains desired state (e.g., restarts failed pods).

---

### Worker Node

- **kubelet**  
  Agent that runs on each node and communicates with API server.

- **kube-proxy**  
  Handles networking and service communication.

- **Container Runtime**  
  Runs containers (containerd, CRI-O).

---

## What Happens When You Run kubectl apply?

1. kubectl sends request to API Server  
2. API Server stores desired state in etcd  
3. Scheduler assigns pod to a node  
4. kubelet creates and runs the container  
5. Controller Manager ensures it stays running  

---

## Failure Scenarios

- **API Server Down** → Cluster becomes unusable (no commands work)  
- **Worker Node Down** → Pods are rescheduled to other nodes  

---

## Tool Used

I used **kind (Kubernetes in Docker)**

**Reason:**  
- Lightweight  
- Fast setup  
- Uses Docker (already installed)  

---

## Cluster Setup Commands

```bash
# Create cluster
kind create cluster --name devops-cluster

# Check cluster
kubectl cluster-info

# List nodes
kubectl get nodes
```

---

## Cluster Exploration

```bash
kubectl get nodes
kubectl describe node kind-control-plane
kubectl get namespaces
kubectl get pods -A
kubectl get pods -n kube-system
```

---

## kube-system Pods and Their Roles

- **kube-apiserver** → Handles API requests  
- **etcd** → Stores cluster data  
- **kube-scheduler** → Assigns pods to nodes  
- **kube-controller-manager** → Maintains desired state  
- **coredns** → DNS for cluster services  
- **kube-proxy** → Networking rules  

---

## Cluster Lifecycle Commands

```bash
# Delete cluster
kind delete cluster --name devops-cluster

# Recreate cluster
kind create cluster --name devops-cluster

# Check nodes
kubectl get nodes
```

---

## kubeconfig

- kubeconfig is a file that stores cluster connection details
- Default location: `~/.kube/config`
- It contains cluster, user, and context information

Commands:

```bash
kubectl config current-context
kubectl config get-contexts
kubectl config view
```

---

## What I Learned

1. Kubernetes manages containers across multiple machines efficiently  
2. Control plane components manage the cluster state and scheduling  
3. Worker nodes run actual workloads (pods)  

---

## Screenshots

Add screenshots here:

- kubectl get nodes
- kubectl get pods -n kube-system

Example file names:
- k8s-nodes.png
- kube-system-pods.png

---

## Summary

Today I set up my first Kubernetes cluster using kind, explored its architecture, and understood how control plane and worker nodes work together. This is the foundation for container orchestration and scalable deployments.
