# Day 52 – Kubernetes Namespaces and Deployments

## Objective

Learned how to organize Kubernetes resources using Namespaces and manage applications using Deployments. Explored self-healing, scaling, rolling updates, and rollbacks.

---

## Task 1: Explore Default Namespaces

### List Namespaces

```bash
kubectl get namespaces
```

### Output

```bash
NAME              STATUS   AGE
default           Active   1d
kube-node-lease   Active   1d
kube-public       Active   1d
kube-system       Active   1d
```

### Check Pods in kube-system

```bash
kubectl get pods -n kube-system
```

### Observation

The `kube-system` namespace contains Kubernetes control plane and networking components required for cluster operation.

### Built-in Namespaces

| Namespace       | Purpose                              |
| --------------- | ------------------------------------ |
| default         | Default namespace for user resources |
| kube-system     | Kubernetes system components         |
| kube-public     | Publicly readable resources          |
| kube-node-lease | Node heartbeat information           |

---

## Task 2: Create and Use Custom Namespaces

### Create Namespaces

```bash
kubectl create namespace dev
kubectl create namespace staging
```

### Verify

```bash
kubectl get namespaces
```

### Create Pods in Namespaces

```bash
kubectl run nginx-dev --image=nginx:latest -n dev
kubectl run nginx-staging --image=nginx:latest -n staging
```

### View Pods Across All Namespaces

```bash
kubectl get pods -A
```

### Observation

* `kubectl get pods` shows pods only in the current namespace.
* `kubectl get pods -A` shows pods from every namespace.

---

## Task 3: Create a Deployment

### Deployment Manifest

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

### Apply Deployment

```bash
kubectl apply -f nginx-deployment.yaml
```

### Verify

```bash
kubectl get deployments -n dev
kubectl get pods -n dev
```

### Sample Output

```bash
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           1m
```

### Meaning of Columns

| Column     | Description                                |
| ---------- | ------------------------------------------ |
| READY      | Running and healthy pods                   |
| UP-TO-DATE | Pods using latest deployment specification |
| AVAILABLE  | Pods available to serve traffic            |

---

## Task 4: Self-Healing Deployment

### Delete a Pod

```bash
kubectl get pods -n dev

kubectl delete pod nginx-deployment-xxxxxx -n dev
```

### Verify

```bash
kubectl get pods -n dev
```

### Observation

A new pod was automatically created by the Deployment controller.

### Result

* Deleted pod was removed.
* Replacement pod received a different name.
* Desired replica count remained unchanged.

---

## Task 5: Scale the Deployment

### Scale Up

```bash
kubectl scale deployment nginx-deployment --replicas=5 -n dev
```

### Verify

```bash
kubectl get pods -n dev
```

### Scale Down

```bash
kubectl scale deployment nginx-deployment --replicas=2 -n dev
```

### Observation

Kubernetes automatically created additional pods while scaling up and removed extra pods while scaling down.

### Scaling Methods

#### Imperative

```bash
kubectl scale deployment nginx-deployment --replicas=5 -n dev
```

#### Declarative

Modify:

```yaml
replicas: 5
```

Apply:

```bash
kubectl apply -f nginx-deployment.yaml
```

---

## Task 6: Rolling Update and Rollback

### Update Image

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.25 -n dev
```

### Check Rollout

```bash
kubectl rollout status deployment/nginx-deployment -n dev
```

### View History

```bash
kubectl rollout history deployment/nginx-deployment -n dev
```

### Rollback

```bash
kubectl rollout undo deployment/nginx-deployment -n dev
```

### Verify Image

```bash
kubectl describe deployment nginx-deployment -n dev | grep Image
```

### Output

```bash
Image: nginx:1.24
```

### Observation

Rolling updates replaced pods gradually without downtime. Rollback restored the previous stable version.

---

## What are Namespaces?

Namespaces provide logical separation within a Kubernetes cluster.

### Benefits

* Environment isolation (Dev, Staging, Production)
* Resource organization
* Access control
* Resource quotas
* Multi-team management

Example:

```text
Cluster
├── dev
├── staging
└── production
```

---

## Deployment Manifest Explanation

### metadata

Contains deployment name, labels, and namespace.

### replicas

Defines the number of desired pod instances.

### selector

Matches pods managed by the deployment.

### template

Blueprint used to create pods.

### containers

Defines container image, ports, and runtime configuration.

---

## Deployment vs Standalone Pod

| Feature          | Standalone Pod | Deployment  |
| ---------------- | -------------- | ----------- |
| Self-Healing     | ❌ No           | ✅ Yes       |
| Scaling          | ❌ Manual       | ✅ Automatic |
| Rolling Updates  | ❌ No           | ✅ Yes       |
| Rollback         | ❌ No           | ✅ Yes       |
| Production Ready | ❌ No           | ✅ Yes       |

### Example

Standalone Pod:

```bash
kubectl delete pod nginx
```

Result:

```text
Pod permanently removed.
```

Deployment Pod:

```bash
kubectl delete pod nginx-deployment-xxxxx
```

Result:

```text
New pod automatically created.
```

---

## Screenshots

### Namespaces

```bash
kubectl get namespaces
```

### Deployments

```bash
kubectl get deployments -A
```

### Pods

```bash
kubectl get pods -A
```

Attach screenshots of the above commands in the repository.

---

## Cleanup

```bash
kubectl delete deployment nginx-deployment -n dev

kubectl delete pod nginx-dev -n dev

kubectl delete pod nginx-staging -n staging

kubectl delete namespace dev staging production
```

### Verify Cleanup

```bash
kubectl get namespaces

kubectl get pods -A
```

### Result

All created resources were successfully removed.

---

## Conclusion

Successfully learned Kubernetes Namespaces and Deployments. Created isolated environments using namespaces, deployed applications using Deployments, tested self-healing behavior, scaled replicas, performed rolling updates, and executed rollbacks.
