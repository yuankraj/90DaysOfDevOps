# Day 53 – Kubernetes Services

## Objective

Today I learned how Kubernetes Services provide stable networking and load balancing for Pods. Since Pod IPs are temporary and can change whenever a Pod restarts, Services provide a permanent endpoint that applications can use to communicate reliably.

---

# Why Services?

Every Pod receives its own IP address.

However:

1. Pod IPs are not permanent.
2. Deployments create multiple Pods.

This creates a problem because applications need a stable endpoint.

Services solve this by providing:

* Stable virtual IP address
* DNS name
* Load balancing across Pods
* Service discovery

Architecture:

```text
Client
   |
   v
Service (Stable IP)
   |
   +--> Pod 1
   +--> Pod 2
   +--> Pod 3
```

---

# Task 1 – Application Deployment

## app-deployment.yaml

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

## Deploy Application

```bash
kubectl apply -f app-deployment.yaml
```

Verify:

```bash
kubectl get pods -o wide
```

Example:

```bash
NAME                       READY STATUS  IP
web-app-7fd8c9d6f-km4q2   1/1   Running 10.244.0.5
web-app-7fd8c9d6f-qm8fd   1/1   Running 10.244.0.6
web-app-7fd8c9d6f-z7xkc   1/1   Running 10.244.0.7
```

---

# Task 2 – ClusterIP Service

ClusterIP is the default Kubernetes Service type.

Accessible only inside the cluster.

---

## clusterip-service.yaml

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
```

---

## Verify

```bash
kubectl get services
```

Example:

```bash
NAME                TYPE        CLUSTER-IP
web-app-clusterip   ClusterIP   10.96.25.120
```

---

## Test From Inside Cluster

```bash
kubectl run test-client \
--image=busybox:latest \
--rm -it \
--restart=Never -- sh
```

Inside container:

```bash
wget -qO- http://web-app-clusterip
```

Result:

```html
Welcome to nginx!
```

---

# Task 3 – Kubernetes DNS

Every Service automatically receives a DNS name.

Format:

```text
<service-name>.<namespace>.svc.cluster.local
```

Example:

```text
web-app-clusterip.default.svc.cluster.local
```

---

## DNS Test

```bash
kubectl run dns-test \
--image=busybox:latest \
--rm -it \
--restart=Never -- sh
```

Inside Pod:

```bash
wget -qO- http://web-app-clusterip

wget -qO- \
http://web-app-clusterip.default.svc.cluster.local

nslookup web-app-clusterip
```

Both URLs resolve to the same ClusterIP.

---

# Task 4 – NodePort Service

NodePort exposes applications outside the cluster using a node port.

Traffic Flow:

```text
NodeIP:30080
       |
       v
NodePort Service
       |
       v
Pods
```

---

## nodeport-service.yaml

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
```

---

## Verify

```bash
kubectl get services
```

Example:

```bash
web-app-nodeport NodePort 80:30080/TCP
```

---

## Access Service

### Minikube

```bash
minikube service web-app-nodeport --url
```

### Docker Desktop

```bash
curl http://localhost:30080
```

Result:

```html
Welcome to nginx!
```

---

# Task 5 – LoadBalancer Service

LoadBalancer is primarily used in cloud environments.

AWS, Azure and GCP automatically create external load balancers.

---

## loadbalancer-service.yaml

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
```

---

## Verify

```bash
kubectl get services
```

Example:

```bash
NAME                   TYPE           EXTERNAL-IP
web-app-loadbalancer   LoadBalancer   <pending>
```

---

## Why EXTERNAL-IP Shows Pending

Local Kubernetes clusters:

* kind
* minikube
* Docker Desktop

do not have a cloud provider.

Therefore Kubernetes cannot provision a real load balancer.

---

## Minikube Tunnel

```bash
minikube tunnel
```

This simulates cloud LoadBalancer functionality.

---

# Service Comparison

| Type         | Accessible From | Use Case               |
| ------------ | --------------- | ---------------------- |
| ClusterIP    | Inside Cluster  | Internal communication |
| NodePort     | NodeIP:Port     | Development & testing  |
| LoadBalancer | Public IP       | Production workloads   |

---

# Service Hierarchy

```text
LoadBalancer
      |
      v
NodePort
      |
      v
ClusterIP
```

Every LoadBalancer Service also creates:

* ClusterIP
* NodePort

---

## Verify

```bash
kubectl describe service web-app-loadbalancer
```

You can see:

* ClusterIP
* NodePort
* LoadBalancer configuration

---

# Endpoints

Endpoints show which Pods a Service is routing traffic to.

View endpoints:

```bash
kubectl get endpoints web-app-clusterip
```

Example:

```bash
NAME                ENDPOINTS
web-app-clusterip   10.244.0.5:80,
                    10.244.0.6:80,
                    10.244.0.7:80
```

These are the actual Pod IPs behind the Service.

---

# Cleanup

Delete deployment:

```bash
kubectl delete -f app-deployment.yaml
```

Delete services:

```bash
kubectl delete -f clusterip-service.yaml

kubectl delete -f nodeport-service.yaml

kubectl delete -f loadbalancer-service.yaml
```

Verify:

```bash
kubectl get pods

kubectl get services
```

Only Kubernetes default services remain.

---

# Commands Practiced

```bash
kubectl apply -f

kubectl get services

kubectl get services -o wide

kubectl describe service

kubectl get endpoints

kubectl run

kubectl delete -f

nslookup
```

---

# Key Learnings

1. Services provide stable networking for Pods.
2. ClusterIP is used for internal communication.
3. NodePort exposes applications externally through node ports.
4. LoadBalancer is used for production traffic in cloud environments.
5. Kubernetes DNS enables service discovery without hardcoded IPs.
6. Endpoints show the Pods behind a Service.

---

# Screenshots to Add

### Services

```bash
kubectl get services
```

### Endpoints

```bash
kubectl get endpoints
```

### ClusterIP Test

```bash
wget -qO- http://web-app-clusterip
```

### NodePort Access

Browser screenshot showing Nginx welcome page.

---

# Conclusion

Today I learned how Kubernetes Services provide stable networking and load balancing for applications. I created ClusterIP, NodePort, and LoadBalancer services, explored Kubernetes DNS-based service discovery, and understood how Services route traffic to healthy Pods through Endpoints.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
