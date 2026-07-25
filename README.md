<p align="center">
  <img src="docs/banner.png" alt="Production DevOps Pipeline Banner" width="100%">
</p>

<h1 align="center">Production DevOps Pipeline</h1>

<p align="center">
  <strong>A Production-Ready CI/CD Pipeline built using Spring Boot, Jenkins, Docker, Kubernetes, SonarQube, Trivy, and MySQL.</strong>
</p>

<p align="center">

![Java](https://img.shields.io/badge/Java-17-orange?style=for-the-badge&logo=openjdk)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-green?style=for-the-badge&logo=springboot)
![Maven](https://img.shields.io/badge/Maven-3.9-red?style=for-the-badge&logo=apachemaven)
![Jenkins](https://img.shields.io/badge/Jenkins-CI-red?style=for-the-badge&logo=jenkins)
![Docker](https://img.shields.io/badge/Docker-Container-blue?style=for-the-badge&logo=docker)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Orchestration-blue?style=for-the-badge&logo=kubernetes)
![MySQL](https://img.shields.io/badge/MySQL-Database-blue?style=for-the-badge&logo=mysql)
![SonarQube](https://img.shields.io/badge/SonarQube-Code_Quality-4E9BCD?style=for-the-badge&logo=sonarqube)
![Trivy](https://img.shields.io/badge/Trivy-Security-purple?style=for-the-badge)

</p>

---

# Project Overview

Modern software delivery requires more than writing application code—it demands automation, security, quality assurance, and reliable deployments.

This project demonstrates a **production-style DevOps CI/CD pipeline** for a Spring Boot application. It automates the complete software delivery lifecycle, including code compilation, testing, static code analysis, security scanning, containerization, image publishing, and Kubernetes deployment.

The pipeline integrates industry-standard DevOps tools to ensure every change is validated, securely packaged, and deployed consistently.

## Key Highlights

- Automated CI/CD pipeline using Jenkins
- Spring Boot REST API with MySQL
- Maven build, test, and package automation
- Code quality analysis using SonarQube
- Filesystem and container image security scanning using Trivy
- Docker image creation and publishing to Docker Hub
- Kubernetes deployment with rolling updates
- Health verification using Spring Boot Actuator
- Infrastructure organized using Docker Compose and Kubernetes manifests
- Production-oriented repository structure and automation scripts

---

# Features

## CI/CD Automation

- Automated CI/CD pipeline using Jenkins
- Source code checkout from GitHub
- Maven build lifecycle automation
- Automated unit testing with JUnit 5
- Code coverage generation using JaCoCo
- Automatic artifact packaging
- Docker image build and push to Docker Hub
- Automated Kubernetes deployment
- Deployment verification and health checks

---

## Code Quality & Security

- SonarQube static code analysis
- Trivy filesystem vulnerability scanning
- Trivy Docker image security scanning
- Secure Docker image creation
- Spring Boot Actuator health monitoring

---

## Containerization

- Production-ready Dockerfile
- Multi-container setup using Docker Compose
- Docker Hub image publishing
- Container health checks

---

## Kubernetes Deployment

- Deployment with rolling updates
- Service exposure using NodePort
- ConfigMaps for application configuration
- Kubernetes Secrets for sensitive data
- Persistent Volume Claims (PVC)
- MySQL deployment inside Kubernetes
- Readiness and Liveness Probes
- Resource Requests & Limits

---

## Spring Boot Application

- RESTful Employee Management API
- CRUD Operations
- DTO Pattern
- Validation
- Exception Handling
- Spring Data JPA
- MySQL Database
- Swagger/OpenAPI Documentation
- Spring Boot Actuator

---

# Architecture

The following diagram illustrates the end-to-end CI/CD workflow implemented in this project.

<p align="center">
    <img src="docs/architecture.png" alt="Architecture Diagram" width="100%">
</p>

---

# CI/CD Pipeline Workflow

The Jenkins pipeline automates the complete software delivery lifecycle, from source code checkout to Kubernetes deployment. Every stage has a specific responsibility to ensure the application is built, tested, secured, containerized, and deployed consistently.

The workflow follows two major phases:

- **Continuous Integration (CI)** – Build, test, validate code quality, and create a deployable artifact.
- **Continuous Delivery (CD)** – Package the application as a Docker image, publish it to Docker Hub, deploy it to Kubernetes, and verify a successful rollout.

---

## Continuous Integration (CI)

The Continuous Integration phase validates every code change before deployment. It ensures that the application compiles successfully, passes all automated tests, meets quality standards, and is free from known security vulnerabilities.

### Stage 1 – Checkout Source Code

**Purpose**

Retrieve the latest source code from the GitHub repository.

**Why it is required**

Every pipeline execution must work on the latest version of the application to ensure builds are reproducible.

**Outcome**

- Downloads the latest code
- Initializes the Jenkins workspace

---

### Stage 2 – Maven Verify

**Purpose**

Compile the application and execute the Maven verification lifecycle.

**Why it is required**

This stage ensures the application builds successfully and all project dependencies are resolved.

**Outcome**

- Source code compilation
- Dependency validation
- Maven lifecycle verification

---

### Stage 3 – JUnit Test Execution

**Purpose**

Execute automated unit tests.

**Why it is required**

Automated testing detects regressions early and validates business logic before deployment.

**Outcome**

- Executes JUnit 5 test cases
- Generates test reports
- Prevents deployment if tests fail

---

### Stage 4 – JaCoCo Code Coverage

**Purpose**

Measure automated test coverage.

**Why it is required**

Code coverage helps evaluate how much of the application is validated through automated testing.

**Outcome**

- Generates HTML coverage report
- Highlights untested code
- Supports quality improvement

---

### Stage 5 – SonarQube Code Analysis

**Purpose**

Perform static code analysis.

**Why it is required**

Detects bugs, vulnerabilities, security hotspots, and code smells before deployment.

**Outcome**

- Code quality analysis
- Maintainability metrics
- Security analysis
- Technical debt reporting

---

### Stage 6 – Trivy Filesystem Scan

**Purpose**

Scan the project filesystem for vulnerabilities and security issues.

**Why it is required**

Security scanning before containerization reduces the risk of packaging vulnerable dependencies.

**Outcome**

- Dependency vulnerability scanning
- Secret detection
- Misconfiguration detection

---

## Continuous Delivery (CD)

After the application successfully completes all validation and security checks, the pipeline proceeds with packaging and deployment.

### Stage 7 – Docker Image Build

**Purpose**

Build a production-ready Docker image.

**Why it is required**

Containers ensure consistent application behavior across development, testing, and production environments.

**Outcome**

- Creates Docker image
- Packages the Spring Boot application
- Produces a deployable container

---

### Stage 8 – Docker Push

**Purpose**

Push the Docker image to Docker Hub.

**Why it is required**

Docker Hub acts as the centralized image registry from which Kubernetes pulls application images.

**Outcome**

- Authenticates with Docker Hub
- Pushes the latest application image
- Makes the image available for deployment

---

### Stage 9 – Kubernetes Deployment

**Purpose**

Deploy the latest application version to the Kubernetes cluster.

**Why it is required**

Kubernetes manages application lifecycle, scaling, and availability.

**Outcome**

- Applies Kubernetes manifests
- Updates Deployment
- Creates or updates Pods
- Maintains desired state

---

### Stage 10 – Rollout Verification

**Purpose**

Verify that Kubernetes successfully completed the deployment.

**Why it is required**

Ensures the application starts correctly before marking the deployment successful.

**Outcome**

- Waits for rollout completion
- Detects deployment failures
- Validates Pod readiness

---

### Stage 11 – Health Check

**Purpose**

Verify application availability after deployment.

**Why it is required**

A successful deployment should also produce a healthy, responsive application.

**Outcome**

- Executes Spring Boot Actuator health endpoint
- Confirms application readiness
- Marks the pipeline successful

---

## Continuous Delivery (CD)

After the application successfully completes all validation and security checks, the pipeline proceeds with packaging and deployment.

### Stage 7 – Docker Image Build

**Purpose**

Build a production-ready Docker image.

**Why it is required**

Containers ensure consistent application behavior across development, testing, and production environments.

**Outcome**

- Creates Docker image
- Packages the Spring Boot application
- Produces a deployable container

---

### Stage 8 – Docker Push

**Purpose**

Push the Docker image to Docker Hub.

**Why it is required**

Docker Hub acts as the centralized image registry from which Kubernetes pulls application images.

**Outcome**

- Authenticates with Docker Hub
- Pushes the latest application image
- Makes the image available for deployment

---

### Stage 9 – Kubernetes Deployment

**Purpose**

Deploy the latest application version to the Kubernetes cluster.

**Why it is required**

Kubernetes manages application lifecycle, scaling, and availability.

**Outcome**

- Applies Kubernetes manifests
- Updates Deployment
- Creates or updates Pods
- Maintains desired state

---

### Stage 10 – Rollout Verification

**Purpose**

Verify that Kubernetes successfully completed the deployment.

**Why it is required**

Ensures the application starts correctly before marking the deployment successful.

**Outcome**

- Waits for rollout completion
- Detects deployment failures
- Validates Pod readiness

---

### Stage 11 – Health Check

**Purpose**

Verify application availability after deployment.

**Why it is required**

A successful deployment should also produce a healthy, responsive application.

**Outcome**

- Executes Spring Boot Actuator health endpoint
- Confirms application readiness
- Marks the pipeline successful

---

## Pipeline Summary

The complete CI/CD pipeline performs the following automated workflow:

```text
Developer
     │
     ▼
GitHub Repository
     │
     ▼
Jenkins Pipeline
     │
     ├── Checkout
     ├── Maven Verify
     ├── JUnit Tests
     ├── JaCoCo
     ├── SonarQube
     ├── Trivy Filesystem Scan
     ├── Docker Build
     ├── Docker Push
     ├── Kubernetes Deployment
     ├── Rollout Verification
     └── Health Check
     │
     ▼
Production-Ready Kubernetes Deployment
```

This automated workflow ensures that every code change is validated, securely packaged, and consistently deployed with minimal manual intervention.

---

# Deployment Guide

This section explains how to deploy the application in different environments.

---

## Prerequisites

Before running the project, install the following software.

| Tool | Version |
|------|----------|
| Java | 17+ |
| Maven | 3.9+ |
| Docker | Latest |
| Docker Compose | v2+ |
| Kubernetes | v1.30+ |
| kubectl | Latest |
| Jenkins | LTS |
| Git | Latest |

---

## Clone Repository

```bash
git clone https://github.com/Nagaraju-209/production-devops-pipeline.git

cd production-devops-pipeline
```

---

## Build the Application

Navigate to the Spring Boot project.

```bash
cd app/springboot-app
```

Compile and verify the project.

```bash
mvn clean verify
```

Generate the application package.

```bash
mvn package
```

---

## Docker Deployment

Build the Docker image.

```bash
docker build \
-t production-devops-pipeline:latest \
-f docker/Dockerfile .
```

Run the container.

```bash
docker run -d \
-p 8082:8082 \
--name production-devops-pipeline \
production-devops-pipeline:latest
```

Verify.

```bash
docker ps
```

Application:

```
http://localhost:8082
```

---

## Docker Compose Deployment

Deploy the complete application stack.

```bash
docker compose \
-f docker-compose/docker-compose.yml up -d
```

Verify containers.

```bash
docker compose ps
```

Stop the environment.

```bash
docker compose down
```

---

## Kubernetes Deployment

Deploy all Kubernetes resources.

```bash
kubectl apply -f kubernetes/
```

Verify resources.

```bash
kubectl get all
```

Check Pods.

```bash
kubectl get pods
```

Check Services.

```bash
kubectl get svc
```

---

## Verify Deployment

Check rollout status.

```bash
kubectl rollout status deployment/springboot-app
```

Check application health.

```bash
kubectl port-forward svc/springboot-service 8082:8082
```

Open

```
http://localhost:8082/actuator/health
```

Expected response

```json
{
  "status":"UP"
}
```

---

## Cleanup

Delete Kubernetes resources.

```bash
kubectl delete -f kubernetes/
```

Stop Docker Compose.

```bash
docker compose down
```

Remove Docker container.

```bash
docker rm -f production-devops-pipeline
```

---

# Project Screenshots

The following screenshots demonstrate the complete CI/CD pipeline and deployment workflow.

## GitHub Repository

<p align="center">
    <img src="docs/screenshots/github-home.png" width="100%">
</p>

---

## Jenkins Pipeline

<p align="center">
    <img src="docs/screenshots/jenkins-pipeline-success.png" width="100%">
</p>

---

## SonarQube Analysis

<p align="center">
    <img src="docs/screenshots/sonarqube-dashboard.png" width="100%">
</p>

---

## Docker Hub Repository

<p align="center">
    <img src="docs/screenshots/dockerhub-repository.png" width="100%">
</p>

---

## Kubernetes Deployment

<p align="center">
    <img src="docs/screenshots/kubernetes-pods.png" width="100%">
</p>

---

## Swagger API Documentation

<p align="center">
    <img src="docs/screenshots/swagger-ui.png" width="100%">
</p>

---

## Spring Boot Actuator

<p align="center">
    <img src="docs/screenshots/actuator-health.png" width="100%">
</p>

---

# API Documentation

The application exposes REST APIs for Employee Management.

Swagger UI is available at:

```
http://localhost:8082/swagger-ui/index.html
```

Example endpoints:

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | `/api/employees` | Get all employees |
| GET | `/api/employees/{id}` | Get employee by ID |
| POST | `/api/employees` | Create employee |
| PUT | `/api/employees/{id}` | Update employee |
| DELETE | `/api/employees/{id}` | Delete employee |

---

# Security & Code Quality

This project follows secure software delivery practices.

## Code Quality

- Static code analysis using SonarQube
- Detection of bugs and code smells
- Maintainability analysis

## Security Scanning

Filesystem Scan

- Trivy Filesystem Scan

Container Image Scan

- Trivy Docker Image Scan

Reports are available under:

```
reports/
```

---

# Future Enhancements

- Implement GitHub Actions alongside Jenkins
- Add Helm charts for Kubernetes deployment
- Integrate Prometheus & Grafana monitoring
- Deploy to AWS EKS
- Add Ingress Controller
- Implement ArgoCD GitOps deployment
- Add Terraform infrastructure provisioning
- Configure Horizontal Pod Autoscaler (HPA)

---

# Author

**Dandu Rama Siva Naga Raju**

- GitHub: https://github.com/Nagaraju-209
- LinkedIn: https://www.linkedin.com/in/dandu-rama-siva-naga-raju/

If you found this project useful, consider giving it a ⭐ on GitHub.

---