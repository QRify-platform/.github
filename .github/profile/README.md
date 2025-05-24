# 🚀 QRify Platform

Welcome to the GitHub organization for the **QRify Platform** — a cloud-native system for generating and storing QR codes from user-submitted URLs. QRify is built using modern DevOps and GitOps principles, and is designed to run securely and scalably in the cloud using Kubernetes.

---

## 🧱 Architecture Overview

The platform is composed of containerized microservices:

- **Frontend**: A Next.js web app that lets users submit URLs and view QR codes
- **API**: A FastAPI service that generates QR codes and uploads them to AWS S3
- **Storage**: AWS S3 for storing and serving generated QR images

All services are deployed to **Amazon EKS**, managed via **ArgoCD** (App of Apps pattern), and integrated with **CloudWatch** for observability. Infrastructure is provisioned using **Terraform**, and deployments are environment-specific (`dev`, `prod`) using Helm.

---

## 🔧 Technologies Used

- **Next.js** – Frontend application
- **FastAPI** – Backend API service
- **Docker** – Containerization for all components
- **Kubernetes (EKS)** – Container orchestration
- **ArgoCD** – GitOps-based continuous delivery
- **Helm** – Reusable and environment-specific chart management
- **GitHub Actions** – CI pipelines for builds and image pushes
- **Terraform** – Infrastructure as Code for VPC, EKS, S3, IAM, etc.
- **AWS Services** – EKS, S3, Route 53, ACM, CloudWatch
- **IRSA** – Secure cloud permissions for API pods

---

## 📁 Repository Overview

| Repository     | Description                                           |
|----------------|-------------------------------------------------------|
| `frontend`     | Next.js web UI with Docker and GitHub Actions         |
| `api`          | FastAPI backend with S3 integration                   |
| `infra`        | Terraform code for AWS infrastructure                 |
| `helm-charts`  | Shared Helm chart templates for service deployments   |
| `app-state`    | ArgoCD Applications and Helm values for dev/prod      |
| `.github`      | This organization profile README                      |

---

## 🌐 Environments

QRify runs in two separate environments, each with its own ArgoCD App of Apps:

- **Development**
  - Triggered on push to `main`
  - Uses `values.dev.yaml`
  - Lower resources and debug logging

- **Production**
  - Triggered via Git tag or manual promotion
  - Uses `values.prod.yaml`
  - Higher replicas, optimized logging, restricted access

Environment configs are separated using Helm value overrides, and deployed independently through ArgoCD.

---

## 🔒 Security & Cloud Best Practices

- **IRSA** (IAM Roles for Service Accounts) ensures secure access from the API to S3 without exposing credentials
- TLS termination via **ACM** and DNS with **Route 53**
- Option to use **ALB or NGINX** Ingress Controllers
- All infrastructure is provisioned using Terraform modules and best practices

---

## 📈 Observability

- **CloudWatch Container Insights** for real-time metrics
- **Fluent Bit** used to stream logs to CloudWatch Logs
- Supports optional dashboards and alarms for system health monitoring

---

## ⚙️ CI/CD Workflows

- **GitHub Actions** build and push Docker images to Amazon ECR
- **ArgoCD** watches the `app-state` repo and syncs apps using the App of Apps pattern
- Separate `values.dev.yaml` and `values.prod.yaml` enable safe multi-environment releases

---

## ✅ Summary

The QRify Platform is a real-world demonstration of production-grade DevOps practices, including:

- GitOps-based Kubernetes application delivery
- CI/CD automation across environments
- Cloud-native observability and security
- Multi-environment Helm deployment strategy
- Modular, reusable Terraform infrastructure

Designed for performance, security, and maintainability in modern cloud-native ecosystems.

---

## 📬 Contact

Built and maintained by Bryan Ramos.  
Feel free to reach out for questions or collaboration.
