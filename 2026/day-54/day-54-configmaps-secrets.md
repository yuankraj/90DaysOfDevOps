# Day 54 – Kubernetes ConfigMaps and Secrets

## 📌 Overview

Applications require configuration such as database URLs, API keys, and feature flags. Hardcoding these values inside container images is not recommended because it requires rebuilding images whenever values change.

Kubernetes provides:

- **ConfigMaps** → for non-sensitive configuration  
- **Secrets** → for sensitive data  

---

# 🔹 Task 1: Create ConfigMap from Literals

## Command
```bash
kubectl create configmap app-config \
  --from-literal=APP_ENV=production \
  --from-literal=APP_DEBUG=false \
  --from-literal=APP_PORT=8080
