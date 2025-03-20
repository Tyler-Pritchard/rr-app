# MONITORING.md

## 📊 Observability Stack: Prometheus & Grafana

This guide outlines best practices and deployment instructions for the Prometheus + Grafana monitoring stack within the RR-App microservices platform.

---

### 🔧 Prometheus Setup (Helm)

1. **Install Prometheus using Helm:**
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/prometheus \
  --namespace monitoring \
  --create-namespace
```

2. **Port-forward Prometheus Server UI:**
```bash
kubectl port-forward -n monitoring svc/prometheus-server 9090:80
```
Access Prometheus at [http://localhost:9090](http://localhost:9090)

3. **Enable Pod-Level Metrics Scraping:**
Ensure each deployment includes appropriate scrape annotations:
```yaml
annotations:
  prometheus.io/scrape: "true"
  prometheus.io/port: "8080"
  prometheus.io/path: "/actuator/prometheus"
```

### ✅ Verify Metrics Are Flowing

Open the Prometheus Web UI:

- Navigate to: [http://localhost:9090/targets](http://localhost:9090/targets)
- Confirm all targets are marked as **`UP`**

You can also test metrics availability by running basic queries from the Prometheus "Expression" tab:

```promql
up
jvm_memory_used_bytes{app="rr-store"}
http_server_requests_seconds_count{job="kubernetes-pods"}
```

If any targets are listed as **`DOWN`**, inspect the associated pod:

```bash
kubectl describe pod <pod-name>
kubectl logs <pod-name> --tail=100
```

---

### 📈 Grafana Setup (Helm)

1. **Install Grafana:**
```bash
helm install grafana prometheus-community/grafana \
  --namespace monitoring \
  --create-namespace
```

2. **Retrieve Admin Credentials:**
```bash
kubectl get secret --namespace monitoring grafana \
  -o jsonpath="{.data.admin-password}" | base64 --decode
```
Username: `admin`

3. **Port-forward Grafana UI:**
```bash
kubectl port-forward -n monitoring svc/grafana 3000:80
```
Access Grafana at [http://localhost:3000](http://localhost:3000)

---

### 📂 Recommended Dashboards
- JVM Micrometer Metrics (rr-store)
- Spring Boot Health & Metrics
- PostgreSQL Exporter
- Golang Service Runtime Metrics (rr-gateway, rr-payments)
- Node.js Application Metrics (rr-auth)
- Kubernetes Cluster Overview

---

### 📊 Sample PromQL Queries
```sql
up
jvm_memory_used_bytes{app="rr-store"}
go_gc_duration_seconds_sum{job="rr-payments"}
http_server_requests_seconds_count{app="rr-auth"}
process_cpu_usage{app="rr-gateway"}
```

---

### 🔍 Troubleshooting Tips
- Check `/actuator/prometheus` on each service to confirm metrics exposure.
- Use `kubectl describe pod <pod>` for pod-level debugging.
- Restart pods when necessary:
```bash
kubectl rollout restart deployment <deployment-name>
```

---

### 📌 Observability Best Practices
- Monitor system availability via `up` query.
- Track SLI metrics: request latency, error rate, CPU/memory utilization.
- Define SLOs in Grafana using threshold markers.
- Enable alerting rules in Prometheus or Grafana for proactive monitoring.

For production environments, consider integrating OpenTelemetry for distributed tracing.

