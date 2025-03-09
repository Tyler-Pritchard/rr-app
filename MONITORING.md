# MONITORING.md

## 📊 Prometheus & Grafana Observability Stack

This document outlines how to install, configure, and access the observability tools used to monitor the RR Microservice Architecture.

---

### 🔧 Prometheus Setup

#### 1. Install Prometheus via Helm
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/prometheus \
  --namespace monitoring \
  --create-namespace
```

#### 2. Port Forward Prometheus Web UI
```bash
kubectl port-forward -n monitoring svc/prometheus-server 9090:80
```
Access via [http://localhost:9090](http://localhost:9090)

#### 3. Prometheus Auto Discovery (Kubernetes Annotations)
Ensure your deployments include the following metadata annotations:
```yaml
annotations:
  prometheus.io/scrape: "true"
  prometheus.io/port: "8080"
  prometheus.io/path: "/actuator/prometheus"
```


---

### 📈 Grafana Setup

#### 1. Install Grafana via Helm
```bash
helm install grafana prometheus-community/grafana \
  --namespace monitoring \
  --create-namespace
```

#### 2. Retrieve Grafana Admin Password
```bash
kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode
```
Username: `admin`

#### 3. Port Forward Grafana
```bash
kubectl port-forward -n monitoring svc/grafana 3000:80
```
Access via [http://localhost:3000](http://localhost:3000)


---

### 📂 Suggested Dashboards
- JVM Micrometer Dashboard
- Spring Boot Metrics
- PostgreSQL Exporter Metrics
- Go Services Metrics

---

### 🔍 Example PromQL Queries
```sql
up
http_server_requests_seconds_count{app="rr-auth"}
cpu_usage_seconds_total{job="kubelet"}
jvm_memory_used_bytes{app="rr-store"}
go_gc_duration_seconds_sum{job="rr-payments"}
```

---

### 📌 Notes
- If a service is down in Prometheus, check `/actuator/prometheus` directly.
- Use `kubectl describe pod <pod>` for debug logs.
- Restart deployments if necessary via:
```bash
kubectl rollout restart deployment <deployment-name>
```

Let me know if you'd like me to generate an ARCHITECTURE.png or a polished README.md sample next.

