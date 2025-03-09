# DEPLOYMENT.md

## 📦 Project Setup & Deployment Guide

This document outlines the local development, Docker, and Kubernetes deployment instructions for the RR Microservice Architecture.

---

### 🔧 Kubernetes Setup

#### 1. Start Minikube Cluster
```bash
minikube start
minikube addons enable ingress
minikube addons enable metrics-server
```

#### 2. Verify Cluster Status
```bash
kubectl get pods -A
kubectl get svc -A
kubectl get deployments -A
```

#### 3. Apply Kubernetes Deployments
```bash
kubectl apply -f rr-store/postgres-deployment.yaml
kubectl apply -f rr-store/rr-store-deployment.yaml
kubectl apply -f rr-store/rr-store-service.yaml

kubectl apply -f rr-payments/rr-payments-deployment.yaml
kubectl apply -f rr-payments/rr-payments-service.yaml

kubectl apply -f rr-gateway/rr-gateway-deployment.yaml

kubectl apply -f rr-auth/rr-auth-deployment.yaml
kubectl apply -f rr-auth/rr-auth-service.yaml
kubectl apply -f rr-auth/rr-auth-config.yaml
kubectl apply -f rr-auth/rr-auth-ingress.yaml
kubectl apply -f rr-auth/rr-auth-secret.yaml
```

#### 4. Confirm Pod Status
```bash
kubectl get pods -A
```

---

### 🐳 Local Docker Development

#### 1. Set Minikube Docker Environment
```bash
eval $(minikube docker-env)
```

#### 2. Build Docker Images Locally
```bash
docker build -t rr-store:latest ./rr-store
docker build -t rr-payments:latest ./rr-payments
docker build -t rr-gateway:latest ./rr-gateway
docker build -t rr-auth:latest ./rr-auth
```

#### 3. Reapply Deployment Configs (if needed)
```bash
kubectl apply -f rr-store/rr-store-deployment.yaml
kubectl apply -f rr-payments/rr-payments-deployment.yaml
kubectl apply -f rr-gateway/rr-gateway-deployment.yaml
kubectl apply -f rr-auth/rr-auth-deployment.yaml
```

---

### 🔁 Docker Compose (Local Testing)

#### 1. Stop Previous Services
```bash
docker-compose -f rr-store/docker-compose.yml down
docker-compose -f rr-gateway/docker-compose.yml down
docker-compose -f rr-payments/docker-compose.yml down
docker-compose -f rr-auth/docker-compose.yml down
```

#### 2. Run Services
```bash
docker-compose -f rr-store/docker-compose.yml up --build -d
docker-compose -f rr-auth/docker-compose.yml up --build -d
docker-compose -f rr-payments/docker-compose.yml up --build -d
docker-compose -f rr-gateway/docker-compose.yml up --build -d
```

---

### 🔍 Health Check Endpoints
```bash
curl http://localhost:8080/actuator/health              # Store
curl http://localhost:8081/health                       # Gateway
curl http://localhost:8081/api/auth/health              # Auth
curl http://localhost:8081/api/payments/health          # Payments
curl http://localhost:8081/api/estore/health            # Store via Gateway
```
