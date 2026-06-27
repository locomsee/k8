# ☸️ Kubernetes Deployment Guides

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?logo=kubernetes\&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker\&logoColor=white)
![Java](https://img.shields.io/badge/Java-17+-orange)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?logo=springboot\&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-C71A36?logo=apachemaven\&logoColor=white)

This directory contains Kubernetes deployment, configuration, and operational guides for microservices and supporting applications.

---

## 🎯 Purpose

These guides provide step-by-step instructions for:

* 📦 Building and packaging applications
* 🐳 Containerizing services with Docker
* ☸️ Deploying workloads to Kubernetes
* 🔐 Managing application configuration and secrets
* 🌐 Exposing services for local and remote access
* 🧪 Testing and validating deployments
* 📊 Monitoring and troubleshooting applications
* 🔄 Managing deployment lifecycles and rollbacks

---

## 🛠️ Technology Stack

Depending on the project, guides may include setup and configuration for:

| Category         | Technologies                          |
| ---------------- | ------------------------------------- |
| Containerization | Docker, Docker Compose                |
| Orchestration    | Kubernetes, Docker Desktop Kubernetes |
| Build Tools      | Maven, Gradle                         |
| Runtime          | Java, Spring Boot                     |
| Databases        | PostgreSQL, MySQL                     |
| Caching          | Redis                                 |
| Messaging        | Kafka                                 |
| API Testing      | Postman, Curl                         |
| Observability    | Spring Actuator, Prometheus, Grafana  |
| CLI Tools        | kubectl, Docker CLI                   |

---

## 📚 Available Guides

| Project             | Description                                                                                       |
| ------------------- | ------------------------------------------------------------------------------------------------- |
| `ms-minipay-engine` | Local Kubernetes deployment guide for the Minipay Engine service using Docker Desktop Kubernetes. |

---

## 📋 Prerequisites

Ensure the following tools are installed before following any guide:

| Tool           | Purpose                                        |
| -------------- | ---------------------------------------------- |
| Docker Desktop | Container runtime and local Kubernetes cluster |
| Kubernetes     | Container orchestration platform               |
| kubectl        | Kubernetes command-line interface              |
| Java 17+       | Application runtime                            |
| Maven / Gradle | Build and dependency management                |
| Git            | Source code management                         |

Verify installation:

```bash
docker --version
kubectl version --client
java -version
mvn -version
git --version
```

---

## 🚀 Getting Started

Navigate to the required project guide and follow the deployment instructions.

```text
k8/
└── ├── README.md
    └── ms-minipay-engine/
        └── README.md
```

Example:

```bash
cd k8/ms-minipay-engine
```

Each guide contains:

* Environment setup
* Application build instructions
* Docker image creation
* Kubernetes manifests
* Configuration management
* Service exposure
* Endpoint validation
* Troubleshooting procedures

---

## 📂 Repository Structure

```text
k8/
└── ├── README.md
    ├── ms-minipay-engine/
    │   └── README.md
    ├── ms-authentication/
    │   └── README.md
    └── ms-payments/
        └── README.md
```

---

## 🔍 Common Operations

### View Kubernetes Resources

```bash
kubectl get all -A
```

### View Application Logs

```bash
kubectl logs -f <pod-name> -n <namespace>
```

### Restart a Deployment

```bash
kubectl rollout restart deployment/<deployment-name> -n <namespace>
```

### Port Forward a Service

```bash
kubectl port-forward svc/<service-name> 8080:80 -n <namespace>
```

---

## 🤝 Contributing

When adding a new deployment guide:

1. Create a dedicated project folder.
2. Add a project-specific README.
3. Include deployment manifests and configuration examples.
4. Document prerequisites, setup, validation, and troubleshooting steps.
5. Update the **Available Guides** section in this document.
6. Follow the existing documentation structure and formatting standards.

---

## 📖 Documentation Standards

All deployment guides should include:

* Overview
* Architecture
* Prerequisites
* Build Process
* Docker Setup
* Kubernetes Deployment
* Service Configuration
* Testing Procedures
* Troubleshooting
* Useful Commands

---

### 💡 Goal

Provide a centralized knowledge base for deploying, operating, and troubleshooting microservices in Kubernetes environments while maintaining consistency across engineering teams.
