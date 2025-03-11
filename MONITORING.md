# MONITORING.md

## 📊 Observability with Prometheus & Grafana

This document outlines the setup and configuration of system-wide observability tools for the RR-App Microservices Architecture. The monitoring stack enables developers and DevOps engineers to inspect runtime metrics, service health, and performance patterns across all microservices in the Kubernetes environment.

---

### 🔧 Prometheus Setup

#### 1️⃣ Install Prometheus via Helm
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/prometheus \
  --namespace monitoring \
  --create-namespace
```

#### 2️⃣ Port Forward the Prometheus Server UI
```bash
kubectl port-forward -n monitoring svc/prometheus-server 9090:80
```
Access Prometheus at: [http://localhost:9090](http://localhost:9090)

#### 3️⃣ Enable Prometheus Scraping for Services
Ensure each Kubernetes deployment includes annotations:
```yaml
annotations:
  prometheus.io/scrape: "true"
  prometheus.io/port: "8080"
  prometheus.io/path: "/actuator/prometheus"
```
> Adjust the port and path depending on your service architecture.

---

### 📈 Grafana Setup

#### 1️⃣ Install Grafana via Helm
```bash
helm install grafana prometheus-community/grafana \
  --namespace monitoring \
  --create-namespace
```

#### 2️⃣ Retrieve Admin Credentials
```bash
kubectl get secret --namespace monitoring grafana \
  -o jsonpath="{.data.admin-password}" | base64 --decode
```
- **Username:** `admin`

#### 3️⃣ Port Forward the Grafana UI
```bash
kubectl port-forward -n monitoring svc/grafana 3000:80
```
Access Grafana at: [http://localhost:3000](http://localhost:3000)

---

### 📂 Suggested Dashboards
- Spring Boot JVM & Micrometer Metrics
- Golang Services (Go Runtime Metrics)
- PostgreSQL Exporter (if integrated)
- Kubernetes Node Metrics (CPU, Memory, Network)

> You can install prebuilt dashboards from [grafana.com/dashboards](https://grafana.com/dashboards)

---

### 🔍 Example PromQL Queries
```sql
up
http_server_requests_seconds_count{app="rr-auth"}
jvm_memory_used_bytes{app="rr-store"}
go_gc_duration_seconds_sum{job="rr-payments"}
cpu_usage_seconds_total{job="kubelet"}
```

---

### 🧪 Troubleshooting Tips
- Check failing services directly:
  ```bash
  curl http://<pod-ip>:8080/actuator/prometheus
  ```
- Investigate scrape errors in Prometheus UI under **Status → Targets**.
- View detailed pod info:
  ```bash
  kubectl describe pod <pod-name>
  ```
- Restart pods to reload configuration:
  ```bash
  kubectl rollout restart deployment <deployment-name>
  ```
- Confirm `imagePullPolicy` and `secrets` are applied correctly if pods fail to start.

---

For additional metrics strategy, instrumentation libraries, and production-grade alerting rules, see future documentation under `monitoring/alert-rules/` and `monitoring/dashboards/` directories (planned).

