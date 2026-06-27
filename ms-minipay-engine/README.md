# ms-minipay-engine Kubernetes Local Deployment Guide

## Overview

This guide explains how to deploy and run `ms-minipay-engine` locally on Kubernetes using Docker Desktop Kubernetes on Windows.

### Architecture

```text
+-------------------+
|   Windows Host    |
|                   |
| PostgreSQL        |
| Redis             |
| Other Services    |
+---------+---------+
          |
          | host.docker.internal
          |
+---------v---------+
| Kubernetes        |
| (Docker Desktop)  |
|                   |
| ms-minipay-engine |
+-------------------+
```

The application runs inside Kubernetes while external dependencies such as PostgreSQL can continue running via Docker Compose.

---

# Prerequisites

## 1. Docker Desktop

Install Docker Desktop and ensure it is running.

Verify:

```bash
docker version
```

---

## 2. Kubernetes Enabled

Open Docker Desktop:

```text
Settings → Kubernetes → Enable Kubernetes
```

Apply and restart Docker Desktop.

Verify:

```bash
kubectl get nodes
```

Expected output:

```text
NAME                    STATUS   ROLES           AGE
desktop-control-plane   Ready    control-plane
```

---

## 3. Java and Maven

Verify Java:

```bash
java -version
```

Verify Maven:

```bash
mvn -version
```

or

```bash
./mvnw -version
```

---

## 4. Database Running

Start PostgreSQL using Docker Compose.

Example:

```bash
docker compose up -d
```

Verify:

```bash
docker ps
```

Ensure PostgreSQL is running and accessible on:

```text
localhost:5432
```

---

# Build the Application

Navigate to the project root.

Clean and package:

```bash
./mvnw clean package
```

Expected artifact:

```text
target/ms-minipay-engine.jar
```

---

# Build Docker Image

Build the application image:

```bash
docker build -t ms-minipay-engine:1.0.0 .
```

Verify image creation:

```bash
docker images
```

Expected:

```text
REPOSITORY          TAG
ms-minipay-engine   1.0.0
```

---

# Kubernetes Deployment

Create a file called:

```text
deployment.yaml
```

Contents:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ms-minipay-engine
  namespace: minipay

spec:
  replicas: 1

  selector:
    matchLabels:
      app: ms-minipay-engine

  template:
    metadata:
      labels:
        app: ms-minipay-engine

    spec:
      containers:
        - name: ms-minipay-engine
          image: ms-minipay-engine:1.0.0
          imagePullPolicy: IfNotPresent

          ports:
            - containerPort: 8080

          env:
            - name: SPRING_DATASOURCE_URL
              value: "jdbc:postgresql://host.docker.internal:5432/minipay"

---
apiVersion: v1
kind: Service
metadata:
  name: ms-minipay-engine
  namespace: minipay

spec:
  selector:
    app: ms-minipay-engine

  ports:
    - port: 80
      targetPort: 8080

  type: ClusterIP
```

---

# Create Namespace

Create the namespace:

```bash
kubectl create namespace minipay
```

Verify:

```bash
kubectl get ns
```

---

# Deploy Application

Apply the deployment:

```bash
kubectl apply -f deployment.yaml
```

Expected:

```text
deployment.apps/ms-minipay-engine created
service/ms-minipay-engine created
```

---

# Verify Deployment

Check deployments:

```bash
kubectl get deployments -n minipay
```

Check pods:

```bash
kubectl get pods -n minipay
```

Expected:

```text
NAME                                READY   STATUS
ms-minipay-engine-xxxxx             1/1     Running
```

Check services:

```bash
kubectl get svc -n minipay
```

---

# View Logs

View application logs:

```bash
kubectl logs -f deployment/ms-minipay-engine -n minipay
```

Or:

```bash
kubectl logs -f <pod-name> -n minipay
```

---

# Access the Application

Port-forward the service:

```bash
kubectl port-forward svc/ms-minipay-engine 8080:80 -n minipay
```

Expected:

```text
Forwarding from 127.0.0.1:8080
```

Application becomes available at:

```text
http://localhost:8080
```

---

# Testing Endpoints

## Health Endpoint

```bash
curl http://localhost:8080/actuator/health
```

Expected:

```json
{
  "status": "UP"
}
```

---

## Other Endpoints

Examples:

```bash
curl http://localhost:8080/api/v1/...
```

or use Postman:

```text
GET  http://localhost:8080/...
POST http://localhost:8080/...
```

---

# Troubleshooting

## Pod Not Starting

Check pod status:

```bash
kubectl get pods -n minipay
```

Describe pod:

```bash
kubectl describe pod <pod-name> -n minipay
```

View logs:

```bash
kubectl logs <pod-name> -n minipay
```

---

## Database Connection Refused

Error:

```text
Connection to localhost:5432 refused
```

Cause:

The application runs inside Kubernetes and cannot use `localhost`.

Use:

```properties
jdbc:postgresql://host.docker.internal:5432/minipay
```

instead of:

```properties
jdbc:postgresql://localhost:5432/minipay
```

---

## Image Pull Errors

Error:

```text
ErrImageNeverPull
```

Solution:

Use:

```yaml
imagePullPolicy: IfNotPresent
```

Ensure image exists:

```bash
docker images
```

---

# Useful Commands

View all resources:

```bash
kubectl get all -n minipay
```

Restart deployment:

```bash
kubectl rollout restart deployment/ms-minipay-engine -n minipay
```

Delete deployment:

```bash
kubectl delete deployment ms-minipay-engine -n minipay
```

Delete service:

```bash
kubectl delete service ms-minipay-engine -n minipay
```

Delete namespace:

```bash
kubectl delete namespace minipay
```

Scale application:

```bash
kubectl scale deployment ms-minipay-engine --replicas=2 -n minipay
```

Check rollout status:

```bash
kubectl rollout status deployment/ms-minipay-engine -n minipay
```

---

# Complete Deployment Flow

```bash
./mvnw clean package

docker build -t ms-minipay-engine:1.0.0 .

kubectl create namespace minipay

kubectl apply -f deployment.yaml

kubectl get pods -n minipay

kubectl port-forward svc/ms-minipay-engine 8080:80 -n minipay

curl http://localhost:8080/actuator/health
```

When all steps succeed, `ms-minipay-engine` is running locally on Kubernetes and accessible through `http://localhost:8080`.
