# Day 50 – Kubernetes Architecture and Cluster Setup

## Introduction

Today I started my Kubernetes journey by understanding the architecture of a Kubernetes cluster, setting up a local cluster using KIND, and exploring the core control plane and worker node components.

Kubernetes was created to solve the challenges of managing containers at scale. While Docker can run containers, Kubernetes automates deployment, scaling, networking, and recovery of containerized applications across multiple machines.

---

# Task 1: Kubernetes History

## Why was Kubernetes created?

Docker is excellent for creating and running containers, but managing hundreds or thousands of containers across multiple servers becomes difficult.

Challenges include:

- Container scheduling
- Auto-healing failed containers
- Scaling applications
- Service discovery
- Load balancing

Kubernetes solves these problems through container orchestration.

## Who created Kubernetes?

Kubernetes was created by Google and was inspired by Google's internal cluster management system called Borg.

## What does Kubernetes mean?

The word Kubernetes comes from Greek and means:

**"Helmsman" or "Ship Captain"**

This is why the Kubernetes logo resembles a ship's steering wheel.

---

# Task 2: Kubernetes Architecture

## Architecture Diagram

```text
                    kubectl
                       |
                       ▼
                 API Server
                       |
    -------------------------------------
    |                |                 |
    ▼                ▼                 ▼
 Scheduler         etcd       Controller Manager

=================================================

              Worker Node 1

        +-------------------+
        |     kubelet       |
        |    kube-proxy     |
        |    containerd     |
        +-------------------+

              Pod A
              Pod B

=================================================

              Worker Node 2

        +-------------------+
        |     kubelet       |
        |    kube-proxy     |
        |    containerd     |
        +-------------------+

              Pod C
              Pod D
```

## Control Plane Components

### API Server

Acts as the entry point to the Kubernetes cluster.

Responsibilities:

- Accepts kubectl commands
- Validates requests
- Communicates with cluster components

### etcd

Distributed key-value database.

Responsibilities:

- Stores cluster state
- Stores configurations
- Stores secrets and metadata

### Scheduler

Responsible for deciding where pods should run.

Responsibilities:

- Checks resource availability
- Selects the most suitable worker node

### Controller Manager

Maintains the desired state of the cluster.

Responsibilities:

- Creates missing pods
- Monitors cluster health
- Handles replication and recovery

---

## Worker Node Components

### kubelet

Agent running on every worker node.

Responsibilities:

- Receives instructions from API Server
- Creates and manages pods
- Reports node status

### kube-proxy

Handles networking between pods and services.

Responsibilities:

- Traffic routing
- Service communication

### Container Runtime

Actually runs containers.

Examples:

- containerd
- CRI-O

---

# What Happens When Running kubectl apply?

Command:

```bash
kubectl apply -f pod.yaml
```

Flow:

1. kubectl sends request to API Server.
2. API Server validates request.
3. Configuration is stored in etcd.
4. Scheduler selects an appropriate node.
5. kubelet receives instructions.
6. Container Runtime creates the container.
7. Pod becomes available.

---

# Failure Scenarios

## If API Server Goes Down

- No new operations can be performed.
- Existing workloads continue running.
- Cluster management becomes unavailable.

## If Worker Node Goes Down

- Kubernetes detects node failure.
- Pods are recreated on healthy nodes.
- Application remains available if replicas exist.

---

# Task 3: Install kubectl

## Verification Command

```bash
kubectl version --client
```

## Output

```bash
Client Version: v1.xx.x
```

This confirms kubectl is installed successfully.

---

# Task 4: Local Cluster Setup

## Selected Tool

### KIND (Kubernetes IN Docker)

### Why I Chose KIND

- Lightweight
- Fast setup
- Uses Docker containers as nodes
- Perfect for local development and learning

---

## Create Cluster

```bash
kind create cluster --name devops-cluster
```

## Verify Cluster

```bash
kubectl cluster-info
kubectl get nodes
```

### Output

```bash
NAME                           STATUS   ROLES           AGE
devops-cluster-control-plane   Ready    control-plane   2m
```

📸 Screenshot: Insert kubectl get nodes screenshot here.

---

# Task 5: Explore the Cluster

## Cluster Information

```bash
kubectl cluster-info
```

Displays Kubernetes control plane information.

---

## View Nodes

```bash
kubectl get nodes
```

Lists all nodes in the cluster.

---

## Describe Node

```bash
kubectl describe node <node-name>
```

Shows:

- CPU resources
- Memory resources
- Conditions
- Running pods

---

## View Namespaces

```bash
kubectl get namespaces
```

Output:

```bash
default
kube-system
kube-public
kube-node-lease
```

---

## View All Pods

```bash
kubectl get pods -A
```

Shows pods running across all namespaces.

---

## View Kubernetes System Pods

```bash
kubectl get pods -n kube-system
```

Example Output:

```bash
coredns
etcd
kube-apiserver
kube-controller-manager
kube-scheduler
kube-proxy
```

📸 Screenshot: Insert kube-system pods screenshot here.

---

# kube-system Components

| Component | Purpose |
|------------|----------|
| etcd | Stores cluster state |
| kube-apiserver | Cluster entry point |
| kube-scheduler | Assigns pods to nodes |
| kube-controller-manager | Maintains desired state |
| kube-proxy | Networking and routing |
| CoreDNS | DNS resolution inside cluster |

---

# Task 6: Cluster Lifecycle

## Delete Cluster

```bash
kind delete cluster --name devops-cluster
```

## Recreate Cluster

```bash
kind create cluster --name devops-cluster
```

## Verify

```bash
kubectl get nodes
```

---

# Kubernetes Configuration

## Current Context

```bash
kubectl config current-context
```

Shows currently connected cluster.

---

## Available Contexts

```bash
kubectl config get-contexts
```

Lists all configured clusters.

---

## View Configuration

```bash
kubectl config view
```

Displays complete kubeconfig details.

---

# What is kubeconfig?

Kubeconfig is the configuration file used by kubectl to connect to Kubernetes clusters.

It contains:

- Cluster information
- User credentials
- Contexts

Default Location:

```bash
~/.kube/config
```

---

# Key Learnings

- Learned why Kubernetes was created.
- Understood Control Plane and Worker Nodes.
- Learned the purpose of API Server, etcd, Scheduler, and Controller Manager.
- Installed and configured kubectl.
- Created a local Kubernetes cluster using KIND.
- Explored namespaces and system pods.
- Learned how Kubernetes schedules and manages workloads.
- Understood kubeconfig and cluster contexts.

---

# Conclusion

Today I successfully started my Kubernetes journey by setting up a local cluster, understanding Kubernetes architecture, and exploring the core control plane components running inside the cluster. This marks the beginning of learning container orchestration and cloud-native infrastructure management.

#90DaysOfDevOps #DevOpsKaJosh #Kubernetes #Docker #CloudNative #TrainWithShubham
