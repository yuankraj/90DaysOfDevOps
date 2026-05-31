# Day 51 – Kubernetes Manifests and Your First Pods

## Objective

Today I learned how Kubernetes resources are defined using YAML manifests and deployed my first Pods into a Kubernetes cluster. I created Pod manifests from scratch, explored running containers inside Pods, worked with labels, and understood the difference between imperative and declarative Kubernetes approaches.

---

# Task 1: Understanding Kubernetes Manifests

Every Kubernetes resource is defined using a YAML manifest.

## Four Required Fields

### apiVersion

Defines which Kubernetes API version should be used.

Example:

```yaml
apiVersion: v1
```

---

### kind

Defines the type of Kubernetes resource.

Example:

```yaml
kind: Pod
```

---

### metadata

Provides identity information such as name and labels.

Example:

```yaml
metadata:
  name: nginx-pod
  labels:
    app: nginx
```

---

### spec

Defines the desired state of the resource.

Example:

```yaml
spec:
  containers:
  - name: nginx
    image: nginx:latest
```

---

# Task 2: First Pod Manifest (Nginx)

## nginx-pod.yaml

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

---

## Apply Manifest

```bash
kubectl apply -f nginx-pod.yaml
```

---

## Verify Pod

```bash
kubectl get pods

kubectl get pods -o wide
```

Expected Output:

```bash
NAME        READY   STATUS    RESTARTS
nginx-pod   1/1     Running   0
```

---

## Explore Pod

### Describe Pod

```bash
kubectl describe pod nginx-pod
```

### View Logs

```bash
kubectl logs nginx-pod
```

### Access Container

```bash
kubectl exec -it nginx-pod -- /bin/bash
```

Inside Container:

```bash
curl localhost:80
```

Result:

```text
Welcome to nginx!
```

---

# Task 3: Custom BusyBox Pod

## busybox-pod.yaml

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

    command:
      - sh
      - -c
      - echo Hello from BusyBox && sleep 3600
```

---

## Apply

```bash
kubectl apply -f busybox-pod.yaml
```

---

## Check Logs

```bash
kubectl logs busybox-pod
```

Output:

```text
Hello from BusyBox
```

---

## Why Command is Needed

BusyBox exits immediately after finishing its default command.

Without:

```yaml
command:
```

The Pod would terminate and enter:

```text
CrashLoopBackOff
```

---

# Task 4: Third Pod Manifest

## devops-tools-pod.yaml

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: devops-tools

  labels:
    app: tools
    environment: lab
    team: devops

spec:
  containers:
  - name: tools
    image: alpine

    command:
      - sh
      - -c
      - sleep 3600
```

---

## Apply

```bash
kubectl apply -f devops-tools-pod.yaml
```

---

# Task 5: Imperative vs Declarative

## Imperative Approach

Create resources directly using commands.

Example:

```bash
kubectl run redis-pod --image=redis:latest
```

Advantages:

* Fast
* Good for testing
* Useful for quick troubleshooting

Disadvantages:

* No version control
* Hard to reproduce

---

## Declarative Approach

Define resources in YAML files.

Example:

```bash
kubectl apply -f nginx-pod.yaml
```

Advantages:

* Infrastructure as Code
* Version controlled
* Repeatable
* Production ready

Disadvantages:

* Requires writing manifests

---

## Comparison Table

| Feature          | Imperative  | Declarative      |
| ---------------- | ----------- | ---------------- |
| Uses YAML        | No          | Yes              |
| Version Control  | No          | Yes              |
| Repeatable       | Limited     | Yes              |
| Production Usage | Rare        | Common           |
| Example          | kubectl run | kubectl apply -f |

---

# Generate YAML Using Dry Run

```bash
kubectl run test-pod \
--image=nginx \
--dry-run=client \
-o yaml
```

Save to file:

```bash
kubectl run test-pod \
--image=nginx \
--dry-run=client \
-o yaml > test-pod.yaml
```

---

# Task 6: Validate Before Applying

## Client Validation

```bash
kubectl apply -f nginx-pod.yaml --dry-run=client
```

---

## Server Validation

```bash
kubectl apply -f nginx-pod.yaml --dry-run=server
```

---

## Example Error

If image field is removed:

```yaml
containers:
- name: nginx
```

Error:

```text
spec.containers[0].image: Required value
```

---

# Task 7: Pod Labels

## Show Labels

```bash
kubectl get pods --show-labels
```

---

## Filter by Label

### Nginx Pods

```bash
kubectl get pods -l app=nginx
```

### Development Pods

```bash
kubectl get pods -l environment=dev
```

### DevOps Team Pods

```bash
kubectl get pods -l team=devops
```

---

## Add Label

```bash
kubectl label pod nginx-pod environment=production
```

---

## Remove Label

```bash
kubectl label pod nginx-pod environment-
```

---

# Task 8: Cleanup

## Delete Pods

```bash
kubectl delete pod nginx-pod

kubectl delete pod busybox-pod

kubectl delete pod redis-pod

kubectl delete pod devops-tools
```

---

## Delete Using Manifest

```bash
kubectl delete -f nginx-pod.yaml
```

---

## Verify

```bash
kubectl get pods
```

Expected:

```text
No resources found
```

---

# What Happens When a Standalone Pod is Deleted?

A standalone Pod has no controller managing it.

When deleted:

* Pod disappears permanently
* Kubernetes does not recreate it
* Application becomes unavailable

This is why production environments use:

* Deployments
* ReplicaSets
* StatefulSets

instead of standalone Pods.

---

# Commands Practiced Today

```bash
kubectl apply -f

kubectl get pods

kubectl get pods -o wide

kubectl describe pod

kubectl logs

kubectl exec

kubectl run

kubectl label

kubectl delete pod

kubectl apply --dry-run

kubectl get pods --show-labels
```

---

# Key Learnings

1. Every Kubernetes resource is defined using a YAML manifest.
2. Pods are the smallest deployable unit in Kubernetes.
3. Labels help organize and filter resources.
4. Declarative configuration is preferred for production environments.
5. Standalone Pods are temporary and should be replaced with Deployments in real-world systems.

---

# Screenshots to Add

### Running Pods

```bash
kubectl get pods
```

### Pod Details

```bash
kubectl describe pod nginx-pod
```

### Labels

```bash
kubectl get pods --show-labels
```

### kube-system Pods

```bash
kubectl get pods -n kube-system
```

---

# Conclusion

Today I wrote my first Kubernetes Pod manifests completely from scratch. I created Nginx, BusyBox, Redis, and custom Pods, explored containers using kubectl exec, validated YAML manifests, used labels for filtering, and learned the difference between imperative and declarative Kubernetes management.

This was my first real hands-on experience deploying workloads into a Kubernetes cluster.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
