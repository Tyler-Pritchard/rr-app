# RR-App Microservices Platform


> A production-grade, enterprise-level microservices architecture designed to showcase industry-standard engineering practices using React, Kubernetes, Docker, Spring Boot, Node.js, Golang, Prometheus, and Grafana.

## 🌐 Overview

RR-App is a modular, containerized microservices platform built to demonstrate scalable, maintainable architecture for real-world commerce applications. It includes user authentication, payment processing, product catalog management, and system observability — all orchestrated via Kubernetes.

### 📐 Architecture Diagram
![Architecture Diagram](./architecture.png)

## 🧱 Microservices Overview

| Service        | Technology Stack        | Description                                 |
|----------------|-------------------------|---------------------------------------------|
| **rr-auth**    | Node.js, MongoDB, JWT   | User authentication and authorization.     |
| **rr-store**   | Java, Spring Boot, PostgreSQL | E-commerce catalog and product management. |
| **rr-payments**| Go, Stripe API          | Payment processing microservice.            |
| **rr-gateway** | Go, Reverse Proxy       | Central API Gateway routing traffic.        |
| **rrsite**	| React, Redux, TypeScript	|Frontend web application for end-user interface.

More details on each service can be found in their respective repos:
- [rr-auth](https://www.github.com/tyler-pritchard/rr-auth)
- [rr-store](https://www.github.com/tyler-pritchard/rr-store)
- [rr-payments](https://www.github.com/tyler-pritchard/rr-payments)
- [rr-gateway](https://www.github.com/tyler-pritchard/rr-gateway)
- [rrsite](https://www.github.com/tyler-pritchard/rrsite)

## 📦 System Observability

- **Prometheus** for metrics scraping
- **Grafana** for visual dashboards
- Health and readiness probes configured for each service

See [`MONITORING.md`](./MONITORING.md) for setup and configuration.

## 🚀 Getting Started

Deployment, configuration, and image-building instructions are documented in:
- [`DEPLOYMENT.md`](./DEPLOYMENT.md)

## 📂 Directory Structure

```
rr-app/
├── rr-auth/
├── rr-gateway/
├── rr-payments/
├── rr-store/
├── rrsite/
├── rr-app/
│   ├── README.md
│   ├── DEPLOYMENT.md
│   ├── MONITORING.md
│   └── links.md
```

## 🛠️ Engineering Standards

- Full environment parity with local Docker, Compose, and Minikube deployments
- Centralized secret/environment management per service
- Helm deployment for Prometheus/Grafana
- Logging, security, metrics, and container health probes
- Project-wide documentation designed for onboarding senior developers

## 👤 Author

Tyler Pritchard  
[GitHub](https://github.com/tyler-pritchard) • [LinkedIn](https://linkedin.com/in/tyler-pritchard)

---

### 📚 Additional Resources

- [Deployment Instructions](./DEPLOYMENT.md)
- [Monitoring Setup](./MONITORING.md)
- [Microservice Links](./links.md)
