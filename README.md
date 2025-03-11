# RR-App Microservices Platform

> A production-grade, enterprise-ready microservices architecture for scalable commerce applications, featuring full-stack observability and service modularity across Node.js, Spring Boot, Golang, and React.

## 🌐 Overview

RR-App is a modular microservices system designed to support modern e-commerce platforms. It enables secure user authentication, catalog management, payment processing, and full observability — all deployed in a Kubernetes ecosystem. Services are containerized with Docker and integrate seamlessly using a central API gateway. Grafana and Prometheus provide real-time system monitoring, while each service remains independently testable and deployable.

## 🧱 Microservices Overview

| Service         | Technology Stack                | Description                                                                                                         |
| --------------- | ------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| [**rr-auth**](https://www.github.com/tyler-pritchard/rr-auth)     | Node.js, Express, MongoDB, JWT  | Handles user authentication, registration, and password reset with rate limiting, reCAPTCHA, and email integration. |
| [**rr-store**](https://www.github.com/tyler-pritchard/rr-store)    | Java, Spring Boot, PostgreSQL   | E-commerce product catalog with CRUD operations, filtering, pagination, and observability endpoints.                |
| [**rr-payments**](https://www.github.com/tyler-pritchard/rr-payments)  | Golang, Stripe API              | Processes payments via Stripe and serves internal billing endpoints.                                                |
| [**rr-gateway**](https://www.github.com/tyler-pritchard/rr-gateway)   | Golang, Reverse Proxy (Chi/Mux) | Lightweight API Gateway for routing requests to internal services.                                                  |
| [**rrsite**](https://www.github.com/tyler-pritchard/rrsite)       | React, Redux, TypeScript        | Frontend web application delivering a dynamic user experience. Integrates with backend APIs and user session state. |

> Each service is independently containerized and modular by design. While services may be developed and deployed individually, Kubernetes enables orchestration of the entire system as a cohesive unit.

## 📊 Observability

- **Prometheus** — Integrated metrics scraping for all services via `/health` or `/actuator/prometheus` endpoints.
- **Grafana** — Pre-configured dashboards via Helm for real-time insights.
- Health and readiness probes are configured for each service to support container orchestration and autoscaling.

See [`MONITORING.md`](./MONITORING.md) for detailed observability configuration.

## 🚀 Getting Started

The application is designed to be deployed as a complete Kubernetes stack, but services may also be run individually for modular development. See each service directory for detailed Docker and local development instructions.

### Quick Setup

1. Start a local Kubernetes cluster with Minikube:

   ```bash
   minikube start
   minikube addons enable ingress
   minikube addons enable metrics-server
   ```

2. Build Docker images locally (optional if not using Docker Hub):

   ```bash
   eval $(minikube docker-env)
   docker build -t rr-auth ./rr-auth
   docker build -t rr-store ./rr-store
   docker build -t rr-payments ./rr-payments
   docker build -t rr-gateway ./rr-gateway
   ```

3. Apply Kubernetes manifests for each service:

   ```bash
   kubectl apply -f rr-auth/
   kubectl apply -f rr-store/
   kubectl apply -f rr-payments/
   kubectl apply -f rr-gateway/
   ```

4. Monitor application state:

   ```bash
   kubectl get pods -A
   kubectl get svc -A
   ```

> See [`DEPLOYMENT.md`](./DEPLOYMENT.md) for detailed deployment and environment setup instructions.

## 📂 Directory Structure

```bash
rr-app/
├── rr-auth/           # Node.js Auth Service
├── rr-gateway/        # Golang API Gateway
├── rr-payments/       # Golang Payments Microservice
├── rr-store/          # Java Spring Boot Store Service
├── rrsite/            # React Frontend Application
├── rr-app/            # Central Project Documentation
│   ├── README.md
│   ├── DEPLOYMENT.md
│   ├── MONITORING.md
│   └── links.md
```

## 🛠️ Engineering Standards

- Microservice-first architecture with domain separation
- Environment-specific configuration for each service (e.g., `.env`, Kubernetes secrets)
- Helm chart deployment for observability stack
- Health/readiness probes on all services
- CI-ready folder structure and metrics instrumentation

## 👤 Author

Tyler Pritchard\
[GitHub](https://github.com/tyler-pritchard) • [LinkedIn](https://linkedin.com/in/tyler-pritchard)

---

## 📚 Additional Resources

- [Deployment Instructions](./DEPLOYMENT.md)
- [Monitoring Setup](./MONITORING.md)
- [Microservice Links](./links.md)

---
## 📄 License

This project is licensed under the [MIT License](../LICENSE.md).  
You are free to use, modify, and distribute this software with appropriate attribution.
