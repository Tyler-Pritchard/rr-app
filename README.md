# RR-App Microservices Platform

> A production-grade, enterprise-level microservices architecture engineered to solve scalable commerce challenges using React, Kubernetes, Docker, Spring Boot, Node.js, Golang, Prometheus, and Grafana.

## 🌐 Overview

RR-App is a modular, containerized microservices platform designed to support a scalable, observable, and secure system architecture. It includes user authentication, payment processing, product catalog management, and full-stack observability — all orchestrated via Kubernetes.

### 📐 Architecture Diagram
![Architecture Diagram](./architecture.png)

## 🧱 Microservices Overview

| Service        | Technology Stack              | Description                                      |
|----------------|-------------------------------|--------------------------------------------------|
| **rr-auth**    | Node.js, MongoDB, JWT         | User authentication and authorization service    |
| **rr-store**   | Java, Spring Boot, PostgreSQL | Product catalog and storefront service           |
| **rr-payments**| Go, Stripe API                | Payment processing and transaction service       |
| **rr-events**  | .NET 8/ASP.NET, PostgreSQL    | Events calendaring and tracking service          |
| **rr-gateway** | Go, Reverse Proxy             | Central API Gateway for routing and aggregation  |
| **rrsite**     | React, Redux, TypeScript      | Frontend web application                         |

More details on each service can be found in their respective repos:
- [rr-auth](https://www.github.com/tyler-pritchard/rr-auth)
- [rr-store](https://www.github.com/tyler-pritchard/rr-store)
- [rr-payments](https://www.github.com/tyler-pritchard/rr-payments)
- [rr-events](https://www.github.com/tyler-pritchard/rr-events)
- [rr-gateway](https://www.github.com/tyler-pritchard/rr-gateway)
- [rrsite](https://www.github.com/tyler-pritchard/rrsite)

## 📦 System Observability

- **Prometheus** for service metrics scraping
- **Grafana** for visual dashboards and alerting
- Health, readiness, and liveness probes configured for each service

See [`MONITORING.md`](./MONITORING.md) for full setup and configuration.

## 🚀 Getting Started (Unified Deployment via Kubernetes or Docker)

RR-App is designed to be deployed as a unified stack through Kubernetes orchestration. While each service is independently containerized and can be developed in isolation, production deployment is managed collectively through declarative manifests. Developers can optionally spin up individual services locally using Docker or Docker Compose for modular testing and iteration.

Deployment, configuration, and image-building instructions are provided in:
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

- Full environment parity with Docker Compose and Minikube-based Kubernetes deployment
- Centralized secrets and environment variable management per service
- Helm-based observability stack provisioning (Prometheus + Grafana)
- Structured logging, RESTful error handling, and health check endpoints
- Documentation optimized for onboarding senior engineers and technical reviewers

## 👤 Author

Tyler Pritchard  
[GitHub](https://github.com/tyler-pritchard) • [LinkedIn](https://linkedin.com/in/tyler-pritchard)

---

### 📚 Additional Resources

- [Deployment Instructions](./DEPLOYMENT.md)
- [Monitoring Setup](./MONITORING.md)
- [Microservice Links](./links.md)

