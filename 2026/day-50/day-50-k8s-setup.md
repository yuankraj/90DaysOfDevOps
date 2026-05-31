# Day 50 – Kubernetes Architecture and Cluster Setup

## Objective

Today's goal was to begin my Kubernetes journey by understanding its architecture, setting up a local Kubernetes cluster, and exploring the core components that make container orchestration possible.

---

# Task 1: Kubernetes Story

## Why was Kubernetes created?

Docker made it easy to package and run applications in containers, but managing hundreds or thousands of containers across multiple servers became difficult. Kubernetes was created to automate deployment, scaling, networking, load balancing, and self-healing of containerized applications.

## Who created Kubernetes?

Kubernetes was originally developed by Google and released as an open-source project in 2014. It was inspired by Google's internal container orchestration platform called Borg.

## What does Kubernetes mean?

The word **Kubernetes** comes from Greek and means **"Helmsman"** or **"Ship Pilot"**, representing the idea of steering and managing containers.

---

# Task 2: Kubernetes Architecture

## Kubernetes Architecture Diagram

```text
                          +-------------+
                          |   kubectl   |
                          +------+------+
                                 |
                                 v
                    +----------------------+
                    |      API Server      |
                    +----------+-----------+
                               |
        +----------------------+----------------------+
        |                      |                      |
        v                      v                      v
    +--------+         +-------------+       +------------------+
    | etcd   |         | Scheduler   |       | Controller Mgr   |
    +--------+         +-------------+       +------------------+

                               |
                               |
              +----------------+----------------+
              |                                 |
              v                                 v

      +-------------------+          +-------------------+
      |   Worker Node 1   |          |   Worker Node 2   |
      +-------------------+          +-------------------+
      | kubelet           |          | kubelet           |
      | kube-proxy        |          | kube-proxy        |
      | containerd        |          | containerd        |
      | Pods              |          | Pods              |
      +-------------------+          +-------------------+
```

---

## Control Plane Components

### API Server

The front door of Kubernetes. Every request from kubectl goes through the API Server.

### etcd

A distributed key-value database that stores the entire cluster state.

### Scheduler

Decides which worker node should run newly created pods.

### Controller Manager

Continuously monitors the cluster and ensures the desired state matches the actual state.

---

## Worker Node Components

### kubelet

Agent running on each worker node that communicates with the API Server and manages pods.

### kube-proxy

Handles networking and communication between pods and services.

### Container Runtime

Runs containers on the node (containerd, CRI-O, Docker).

---

## What Happens When Running:

```bash
kubectl apply -f pod.yaml
```

1. kubectl sends request to API Server.
2. API Server validates the request.
3. Desired state is stored in etcd.
4. Scheduler selects a worker node.
5. kubelet receives instructions.
6. Container Runtime starts the container.
7. kube-proxy configures networking.
8. Pod becomes available.

---

## What If API Server Goes Down?

* New kubectl requests cannot be processed.
* Cluster management operations stop.
* Existing running workloads continue running.

---

## What If a Worker Node Goes Down?

* Pods on that node become unavailable.
* Controller Manager detects failure.
* Scheduler recreates pods on healthy nodes.

---

# Task 3: Install kubectl

## Installation Verification

```bash
kubectl version --client
```

### Example Output

```bash
Client Version: v1.34.0
```

---

# Task 4: Local Cluster Setup

## Selected Tool

### kind (Kubernetes in Docker)

### Why I Chose kind

* Lightweight and fast
* Uses Docker containers as nodes
* Easy installation
* Perfect for learning Kubernetes locally

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

---

# Task 5: Explore the Cluster

## Cluster Information

```bash
kubectl cluster-info
```

---

## List Nodes

```bash
kubectl get nodes
```

Example:

```bash
NAME                           STATUS   ROLES           AGE
devops-cluster-control-plane   Ready    control-plane   5m
```

---

## Detailed Node Information

```bash
kubectl describe node devops-cluster-control-plane
```

---

## List Namespaces

```bash
kubectl get namespaces
```

---

## List All Pods

```bash
kubectl get pods -A
```

---

## List kube-system Pods

```bash
kubectl get pods -n kube-system
```

Example Components:

```bash
coredns
etcd
kube-apiserver
kube-controller-manager
kube-scheduler
kube-proxy
```

---

# kube-system Components and Purpose

| Component               | Purpose                  |
| ----------------------- | ------------------------ |
| kube-apiserver          | Handles all API requests |
| etcd                    | Stores cluster state     |
| kube-scheduler          | Assigns pods to nodes    |
| kube-controller-manager | Maintains desired state  |
| coredns                 | Provides DNS services    |
| kube-proxy              | Handles pod networking   |

---

# Task 6: Cluster Lifecycle

## Delete Cluster

```bash
kind delete cluster --name devops-cluster
```

---

## Recreate Cluster

```bash
kind create cluster --name devops-cluster
```

---

## Verify Cluster

```bash
kubectl get nodes
```

---

# Kubernetes Configuration Commands

## Current Context

```bash
kubectl config current-context
```

---

## List Contexts

```bash
kubectl config get-contexts
```

---

## View kubeconfig

```bash
kubectl config view
```

---

# What is kubeconfig?

A kubeconfig file stores cluster information, user credentials, and contexts used by kubectl to connect to Kubernetes clusters.

### Default Location

```bash
~/.kube/config
```

---

# Commands Practiced Today

```bash
kubectl version --client

kind create cluster --name devops-cluster

kubectl cluster-info

kubectl get nodes

kubectl describe node

kubectl get namespaces

kubectl get pods -A

kubectl get pods -n kube-system

kubectl config current-context

kubectl config get-contexts

kubectl config view

kind delete cluster --name devops-cluster
```

---

# Key Learnings

1. Kubernetes solves container orchestration challenges at scale.
2. The Control Plane manages the cluster while Worker Nodes run workloads.
3. kubectl communicates with the API Server to manage resources.
4. Core Kubernetes components run as pods inside the kube-system namespace.
5. kind provides a lightweight local Kubernetes environment for learning.

---

# Screenshots to Add

* kubectl get nodes
* kubectl get pods -n kube-system
* Kubernetes Architecture Diagram

---

# Conclusion

Today I officially started my Kubernetes journey. I set up a local Kubernetes cluster using kind, explored the Control Plane and Worker Node architecture, learned how kubectl interacts with the API Server, and observed critical Kubernetes components running inside the kube-system namespace.

This marks the beginning of container orchestration and cloud-native infrastructure learning.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
