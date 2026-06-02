# Day 56 – Kubernetes StatefulSets

## Objective

Today I learned about Kubernetes StatefulSets and how they differ from Deployments. StatefulSets are designed for stateful applications like databases, Kafka, Elasticsearch, and other distributed systems that require stable identities, persistent storage, and ordered deployment.

---

# What are StatefulSets?

A StatefulSet is a Kubernetes workload controller used for managing stateful applications.

Unlike Deployments, StatefulSets provide:

* Stable Pod names
* Stable network identities
* Persistent storage per Pod
* Ordered Pod creation and deletion

Common Use Cases:

* MySQL
* PostgreSQL
* MongoDB
* Kafka
* Elasticsearch
* Redis Clusters

---

# Deployment vs StatefulSet

| Feature          | Deployment     | StatefulSet     |
| ---------------- | -------------- | --------------- |
| Pod Names        | Random         | Stable          |
| Startup Order    | Parallel       | Ordered         |
| Shutdown Order   | Parallel       | Reverse Ordered |
| Storage          | Shared         | Per-Pod PVC     |
| Network Identity | Dynamic        | Stable DNS      |
| Best For         | Stateless Apps | Stateful Apps   |

---

# Task 1 – Understanding the Problem

## Deployment Example

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx-deployment

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
        image: nginx
```

---

## Verify

```bash
kubectl get pods
```

Example Output:

```text
nginx-deployment-6d4cf56d98-4w9hc
nginx-deployment-6d4cf56d98-jl9ks
nginx-deployment-6d4cf56d98-zr2pf
```

---

### Problem

Pod names are random.

If a Pod is deleted:

```bash
kubectl delete pod nginx-deployment-6d4cf56d98-4w9hc
```

New Pod:

```text
nginx-deployment-6d4cf56d98-h2rj8
```

Different name.

For databases, this breaks cluster communication.

---

# Task 2 – Create Headless Service

## headless-service.yaml

```yaml
apiVersion: v1
kind: Service

metadata:
  name: web-headless

spec:
  clusterIP: None

  selector:
    app: web

  ports:
  - port: 80
```

---

## Apply

```bash
kubectl apply -f headless-service.yaml
```

---

## Verify

```bash
kubectl get svc
```

Output:

```text
NAME           TYPE        CLUSTER-IP
web-headless   ClusterIP   None
```

---

### Observation

Headless Service has:

```text
CLUSTER-IP = None
```

Required for StatefulSets.

---

# Task 3 – Create StatefulSet

## statefulset.yaml

```yaml
apiVersion: apps/v1
kind: StatefulSet

metadata:
  name: web

spec:
  serviceName: web-headless

  replicas: 3

  selector:
    matchLabels:
      app: web

  template:
    metadata:
      labels:
        app: web

    spec:
      containers:
      - name: nginx
        image: nginx

        ports:
        - containerPort: 80

        volumeMounts:
        - name: web-data
          mountPath: /usr/share/nginx/html

  volumeClaimTemplates:
  - metadata:
      name: web-data

    spec:
      accessModes:
        - ReadWriteOnce

      resources:
        requests:
          storage: 100Mi
```

---

## Apply

```bash
kubectl apply -f statefulset.yaml
```

---

## Watch Creation

```bash
kubectl get pods -l app=web -w
```

Creation Order:

```text
web-0
web-1
web-2
```

Each Pod waits until previous Pod becomes Ready.

---

# Verify Pods

```bash
kubectl get pods
```

Output:

```text
web-0
web-1
web-2
```

---

# Verify PVCs

```bash
kubectl get pvc
```

Output:

```text
web-data-web-0
web-data-web-1
web-data-web-2
```

---

### Observation

Each Pod receives its own dedicated PVC.

---

# Task 4 – Stable Network Identity

StatefulSet DNS Pattern:

```text
<pod-name>.<service-name>.<namespace>.svc.cluster.local
```

Examples:

```text
web-0.web-headless.default.svc.cluster.local

web-1.web-headless.default.svc.cluster.local

web-2.web-headless.default.svc.cluster.local
```

---

## DNS Test

```bash
kubectl run dns-test \
--image=busybox \
--rm -it --restart=Never -- sh
```

Inside Pod:

```bash
nslookup web-0.web-headless.default.svc.cluster.local

nslookup web-1.web-headless.default.svc.cluster.local

nslookup web-2.web-headless.default.svc.cluster.local
```

---

## Verify

```bash
kubectl get pods -o wide
```

Example:

```text
web-0   10.244.0.10
web-1   10.244.0.11
web-2   10.244.0.12
```

DNS Resolution:

```text
web-0 → 10.244.0.10

web-1 → 10.244.0.11

web-2 → 10.244.0.12
```

DNS and Pod IPs match.

---

# Task 5 – Stable Storage

Write unique data:

```bash
kubectl exec web-0 -- \
sh -c "echo 'Data from web-0' > /usr/share/nginx/html/index.html"
```

Verify:

```bash
kubectl exec web-0 -- cat /usr/share/nginx/html/index.html
```

Output:

```text
Data from web-0
```

---

## Delete Pod

```bash
kubectl delete pod web-0
```

Wait for recreation:

```bash
kubectl get pods
```

New Pod:

```text
web-0
```

Same Pod name.

---

## Verify Data

```bash
kubectl exec web-0 -- cat /usr/share/nginx/html/index.html
```

Output:

```text
Data from web-0
```

---

### Observation

Data survived Pod recreation.

Same PVC was reattached.

---

# Task 6 – Ordered Scaling

## Scale Up

```bash
kubectl scale statefulset web --replicas=5
```

Creation Order:

```text
web-3

web-4
```

---

## Scale Down

```bash
kubectl scale statefulset web --replicas=3
```

Deletion Order:

```text
web-4

web-3
```

Reverse order.

---

## Verify PVCs

```bash
kubectl get pvc
```

Output:

```text
web-data-web-0
web-data-web-1
web-data-web-2
web-data-web-3
web-data-web-4
```

---

### Observation

Even after scaling down:

All PVCs remain.

Kubernetes preserves data.

---

# Task 7 – Cleanup

Delete StatefulSet:

```bash
kubectl delete statefulset web
```

Delete Headless Service:

```bash
kubectl delete service web-headless
```

---

## Verify PVCs

```bash
kubectl get pvc
```

PVCs still exist.

---

## Delete PVCs

```bash
kubectl delete pvc --all
```

---

### Observation

PVCs are NOT automatically deleted.

This protects data from accidental loss.

---

# StatefulSet Architecture

```text
                 Headless Service
                        │
        ┌───────────────┼───────────────┐
        │               │               │
      web-0           web-1          web-2
        │               │               │
        ▼               ▼               ▼
  PVC-web-0      PVC-web-1      PVC-web-2
```

---

# Commands Practiced

```bash
kubectl get sts

kubectl apply -f

kubectl get pvc

kubectl get pods -o wide

kubectl scale statefulset

kubectl delete pod

kubectl delete statefulset

nslookup
```

---

# Key Learnings

1. StatefulSets provide stable Pod identities.
2. Each Pod receives its own PVC.
3. Headless Services provide stable DNS records.
4. Pod names remain consistent after recreation.
5. Data survives Pod deletion.
6. Pods scale up sequentially.
7. Pods scale down in reverse order.
8. PVCs remain after StatefulSet deletion for data safety.

---

# Screenshots to Add

### StatefulSet Pods

```bash
kubectl get pods
```

### StatefulSet PVCs

```bash
kubectl get pvc
```

### DNS Resolution

```bash
nslookup web-0.web-headless.default.svc.cluster.local
```

### Scaling

```bash
kubectl scale statefulset web --replicas=5
```

---

# Conclusion

Today I learned Kubernetes StatefulSets and why they are essential for databases and other stateful applications. Unlike Deployments, StatefulSets provide stable pod names, dedicated storage, stable DNS records, and ordered scaling behavior, making them ideal for production-grade distributed systems.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
