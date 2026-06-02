# Day 55 – Persistent Volumes (PV) and Persistent Volume Claims (PVC)

## Objective

Today I learned how Kubernetes handles persistent storage using Persistent Volumes (PV) and Persistent Volume Claims (PVC).

Containers are ephemeral by nature. When a Pod is deleted, all data stored inside the container is lost. To solve this problem, Kubernetes provides Persistent Volumes and Persistent Volume Claims, allowing data to survive Pod restarts and recreations.

---

# Why Containers Need Persistent Storage

By default:

* Pods are temporary
* Containers can be recreated anytime
* Data inside containers is lost after deletion

This is a major problem for:

* Databases
* Log storage
* Application uploads
* Stateful applications

Persistent storage ensures data remains available even when Pods disappear.

---

# Task 1 – Demonstrating Data Loss with emptyDir

## Pod Manifest

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: emptydir-demo

spec:
  containers:
  - name: busybox
    image: busybox

    command:
      - sh
      - -c
      - |
        date > /data/message.txt
        sleep 3600

    volumeMounts:
    - mountPath: /data
      name: temp-storage

  volumes:
  - name: temp-storage
    emptyDir: {}
```

---

## Apply Pod

```bash
kubectl apply -f emptydir-demo.yaml
```

---

## Verify Data

```bash
kubectl exec emptydir-demo -- cat /data/message.txt
```

Output:

```text
Tue Jun 2 10:30:15 UTC 2026
```

---

## Delete and Recreate

```bash
kubectl delete pod emptydir-demo

kubectl apply -f emptydir-demo.yaml
```

---

## Verify Again

```bash
kubectl exec emptydir-demo -- cat /data/message.txt
```

Output:

```text
Tue Jun 2 10:45:20 UTC 2026
```

---

### Observation

The timestamp changed.

This proves that data stored in `emptyDir` is deleted when the Pod is removed.

---

# Task 2 – Create Persistent Volume (Static Provisioning)

## Persistent Volume Manifest

```yaml
apiVersion: v1
kind: PersistentVolume

metadata:
  name: my-pv

spec:
  capacity:
    storage: 1Gi

  accessModes:
    - ReadWriteOnce

  persistentVolumeReclaimPolicy: Retain

  hostPath:
    path: /tmp/k8s-pv-data
```

---

## Apply

```bash
kubectl apply -f pv.yaml
```

---

## Verify

```bash
kubectl get pv
```

Output:

```text
NAME    CAPACITY   ACCESS MODES   STATUS      RECLAIM POLICY
my-pv   1Gi        RWO            Available   Retain
```

---

### Observation

PV status is **Available**.

---

# Access Modes

## ReadWriteOnce (RWO)

Read-write by a single node.

## ReadOnlyMany (ROX)

Read-only by multiple nodes.

## ReadWriteMany (RWX)

Read-write by multiple nodes simultaneously.

---

# Task 3 – Create Persistent Volume Claim

## PVC Manifest

```yaml
apiVersion: v1
kind: PersistentVolumeClaim

metadata:
  name: my-pvc

spec:
  accessModes:
    - ReadWriteOnce

  resources:
    requests:
      storage: 500Mi
```

---

## Apply

```bash
kubectl apply -f pvc.yaml
```

---

## Verify

```bash
kubectl get pvc
kubectl get pv
```

Output:

```text
NAME     STATUS   VOLUME
my-pvc   Bound    my-pv
```

---

### Observation

The PVC automatically matched and bound to the PV.

---

# Task 4 – Use PVC in a Pod

## Pod Manifest

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: pvc-demo

spec:
  containers:
  - name: busybox
    image: busybox

    command:
      - sh
      - -c
      - |
        echo "Pod Started $(date)" >> /data/message.txt
        sleep 3600

    volumeMounts:
    - mountPath: /data
      name: persistent-storage

  volumes:
  - name: persistent-storage
    persistentVolumeClaim:
      claimName: my-pvc
```

---

## Apply

```bash
kubectl apply -f pvc-demo.yaml
```

---

## Verify

```bash
kubectl exec pvc-demo -- cat /data/message.txt
```

Output:

```text
Pod Started Tue Jun 2 11:00:00 UTC 2026
```

---

## Delete and Recreate Pod

```bash
kubectl delete pod pvc-demo

kubectl apply -f pvc-demo.yaml
```

---

## Verify Again

```bash
kubectl exec pvc-demo -- cat /data/message.txt
```

Output:

```text
Pod Started Tue Jun 2 11:00:00 UTC 2026
Pod Started Tue Jun 2 11:10:00 UTC 2026
```

---

### Observation

Data persisted after Pod deletion.

PVC successfully preserved storage.

---

# Task 5 – Storage Classes

## View Storage Classes

```bash
kubectl get storageclass
```

Output:

```text
NAME                 PROVISIONER
standard (default)   rancher.io/local-path
```

---

## Describe Storage Class

```bash
kubectl describe storageclass standard
```

Important Properties:

* Provisioner
* Reclaim Policy
* Volume Binding Mode

---

### Default StorageClass

```text
standard
```

---

# Task 6 – Dynamic Provisioning

## Dynamic PVC

```yaml
apiVersion: v1
kind: PersistentVolumeClaim

metadata:
  name: dynamic-pvc

spec:
  accessModes:
    - ReadWriteOnce

  storageClassName: standard

  resources:
    requests:
      storage: 1Gi
```

---

## Apply

```bash
kubectl apply -f dynamic-pvc.yaml
```

---

## Verify

```bash
kubectl get pvc
kubectl get pv
```

Output:

```text
NAME          STATUS   VOLUME
dynamic-pvc   Bound    pvc-ae45bd

NAME          STATUS   CLAIM
my-pv         Bound    default/my-pvc
pvc-ae45bd    Bound    default/dynamic-pvc
```

---

### Observation

Kubernetes automatically created a new PV.

No manual PV creation was required.

---

# Static vs Dynamic Provisioning

| Feature        | Static | Dynamic    |
| -------------- | ------ | ---------- |
| PV Created By  | Admin  | Kubernetes |
| PVC Required   | Yes    | Yes        |
| Manual Effort  | High   | Low        |
| Production Use | Rare   | Common     |

---

# Reclaim Policies

## Retain

Storage remains after PVC deletion.

Manual cleanup required.

## Delete

Storage automatically deleted when PVC is removed.

---

# Task 7 – Cleanup

## Delete Pods

```bash
kubectl delete pod pvc-demo
kubectl delete pod emptydir-demo
```

---

## Delete PVCs

```bash
kubectl delete pvc my-pvc
kubectl delete pvc dynamic-pvc
```

---

## Verify PV Status

```bash
kubectl get pv
```

Output:

```text
NAME         STATUS
my-pv        Released
pvc-ae45bd   Deleted
```

---

### Observation

Manual PV:

* Retained
* Status = Released

Dynamic PV:

* Automatically deleted

Reason:

* Different reclaim policies

---

# PV Lifecycle

```text
Available
    ↓
Bound
    ↓
Released
    ↓
Deleted (optional)
```

---

# Commands Practiced

```bash
kubectl get pv

kubectl get pvc

kubectl describe pv

kubectl describe pvc

kubectl apply -f

kubectl delete pod

kubectl delete pvc

kubectl get storageclass

kubectl describe storageclass
```

---

# Key Learnings

1. Containers are ephemeral and lose data after deletion.
2. Persistent Volumes provide durable storage.
3. Persistent Volume Claims request storage from available PVs.
4. Static provisioning requires manual PV creation.
5. Dynamic provisioning automatically creates PVs using StorageClasses.
6. Reclaim policies determine what happens after PVC deletion.
7. Data stored through PVCs survives Pod restarts and recreation.

---

# Screenshots to Add

### Data Loss Demonstration

```bash
kubectl exec emptydir-demo -- cat /data/message.txt
```

### Persistent Volume

```bash
kubectl get pv
```

### Persistent Volume Claim

```bash
kubectl get pvc
```

### Bound State

```bash
kubectl get pv,pvc
```

### Dynamic Provisioning

```bash
kubectl get storageclass
```

---

# Conclusion

Today I explored Kubernetes Persistent Volumes and Persistent Volume Claims. I demonstrated how data is lost in ephemeral containers, created Persistent Volumes and Claims, verified data persistence across Pod recreation, and learned how StorageClasses enable dynamic provisioning in modern Kubernetes environments.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
