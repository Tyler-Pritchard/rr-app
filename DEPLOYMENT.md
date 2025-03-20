# DEPLOYMENT.md

## 📦 Project Setup & Deployment Guide

This document provides a comprehensive overview for deploying the RR-App Microservices Architecture in local and production-like environments.

---

## ⚙️ Kubernetes Deployment

### 1. Start Minikube and Enable Addons
```bash
minikube start
minikube addons enable ingress
minikube addons enable metrics-server
```

### 2. Verify Cluster Health
```bash
kubectl get pods -A
kubectl get svc -A
kubectl get deployments -A
```

### 3. Apply Service Deployments
```bash
# RR-Store
kubectl apply -f rr-store/postgres-deployment.yaml
kubectl apply -f rr-store/rr-store-deployment.yaml
kubectl apply -f rr-store/rr-store-service.yaml

# RR-Payments
kubectl apply -f rr-payments/rr-payments-deployment.yaml
kubectl apply -f rr-payments/rr-payments-service.yaml

# RR-Gateway
kubectl apply -f rr-gateway/rr-gateway-deployment.yaml
kubectl apply -f rr-gateway/rr-gateway-service.yaml

# RR-Auth
kubectl apply -f rr-auth/rr-auth-config.yaml
kubectl apply -f rr-auth/rr-auth-secret.yaml
kubectl apply -f rr-auth/rr-auth-deployment.yaml
kubectl apply -f rr-auth/rr-auth-service.yaml
kubectl apply -f rr-auth/rr-auth-ingress.yaml
```

### 4. Confirm System Status
```bash
kubectl get pods -A
```

### 5. Helm-Based Observability Stack
```bash
# Prometheus
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/prometheus \
  --namespace monitoring \
  --create-namespace

# Grafana
helm install grafana prometheus-community/grafana \
  --namespace monitoring \
  --create-namespace
```

---

## 🐳 Docker-Based Development

### 1. Configure Local Docker Environment
```bash
eval $(minikube docker-env)
```

### 2. Build Service Images
```bash
docker build -t rr-store:latest ./rr-store
docker build -t rr-payments:latest ./rr-payments
docker build -t rr-gateway:latest ./rr-gateway
docker build -t rr-auth:latest ./rr-auth
```

### 3. Reapply Kubernetes Deployment (if updated)
```bash
kubectl apply -f rr-store/rr-store-deployment.yaml
kubectl apply -f rr-payments/rr-payments-deployment.yaml
kubectl apply -f rr-gateway/rr-gateway-deployment.yaml
kubectl apply -f rr-auth/rr-auth-deployment.yaml
```

---

## 🧪 Docker Compose (Optional for Local Testing)

### 1. Shut Down Existing Containers
```bash
docker-compose -f rr-store/docker-compose.yml down
docker-compose -f rr-auth/docker-compose.yml down
docker-compose -f rr-payments/docker-compose.yml down
docker-compose -f rr-gateway/docker-compose.yml down
```

### 2. Run Services in Detached Mode
```bash
docker-compose -f rr-store/docker-compose.yml up --build -d
docker-compose -f rr-auth/docker-compose.yml up --build -d
docker-compose -f rr-payments/docker-compose.yml up --build -d
docker-compose -f rr-gateway/docker-compose.yml up --build -d
```

---

## 🔍 Health Check Endpoints
```bash
curl http://localhost:8080/actuator/health              # RR-Store
curl http://localhost:8081/health                       # RR-Gateway
curl http://localhost:8081/api/auth/health              # RR-Auth
curl http://localhost:8081/api/payments/health          # RR-Payments
curl http://localhost:8081/api/estore/health            # RR-Store via Gateway
```

---

## ✅ Best Practices
- Use `--validate=false` when applying manifests that include non-Kubernetes files (e.g., `docker-compose.yml`).
- Declare shared Docker networks when using Compose across multiple services.
- Consolidate environment variable management using `secrets.env` per service.
- Run `kubectl rollout restart deployment <deployment>` when applying updates to service images.
- Refer to [`MONITORING.md`](./MONITORING.md) for observability stack setup.

For detailed service-specific instructions, refer to each service's respective `README.md` file.

### 🛠 Troubleshooting
- Check Prometheus targets: [http://localhost:9090/targets](http://localhost:9090/targets)
- Use `kubectl describe pod <pod>` to investigate crashing containers.
- Use `kubectl logs <pod>` to view runtime exceptions.
- Restart deployments if necessary:
  ```bash
  kubectl rollout restart deployment <deployment-name>
