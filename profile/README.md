# 🚀 QRify Platform

Welcome to the GitHub organization for the **QRify Platform** — a cloud-native system for generating and storing QR codes from user-submitted URLs. QRify is built using modern **DevOps** and **GitOps** principles, and is designed to run securely and scalably in the cloud using **Kubernetes**.

---

## 🧱 Architecture Overview

The platform is composed of containerized microservices and supporting infrastructure:

- **Frontend**: A Next.js web app that lets users submit URLs and view QR codes
- **API**: A FastAPI service that generates QR codes and uploads them to AWS S3
- **Storage**: AWS S3 for storing and serving generated QR images
- **Secrets Management**: Sealed Secrets used for encrypted secret delivery via Git

All services are deployed to **Amazon EKS**, managed via **ArgoCD** (App of Apps pattern), and integrated with **CloudWatch** for observability. Infrastructure is provisioned using **Terraform**, and deployments are environment-specific (`dev`, `prod`) using **Helm**.

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
- **Sealed Secrets** – GitOps-compatible secrets encryption and delivery
- **IRSA** – Secure cloud permissions for API pods

---

## 📁 Repository Overview

| Repository           | Description                                                      |
|----------------------|------------------------------------------------------------------|
| `qrify-web`          | Next.js web UI with Docker and GitHub Actions                   |
| `qrify-web-api`      | FastAPI backend with S3 integration                             |
| `infra`              | Terraform code for AWS infrastructure                           |
| `helm-charts`        | Shared Helm chart templates for service deployments             |
| `cluster-state`      | ArgoCD Applications and Helm values for `dev`/`prod`            |
| `sealed-secrets`     | Encrypted Kubernetes Secrets for GitOps delivery via Bitnami Sealed Secrets |
| `github-actions`     | Reusable GitHub composite actions (e.g., Docker build, tag update) |
| `.github`            | Org-wide metadata and documentation                             |

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
- **Sealed Secrets** enable encrypted secret storage in Git, decrypted only in the target Kubernetes cluster
- TLS termination via **ACM** and DNS with **Route 53**
- Supports both **ALB** and **NGINX** Ingress Controllers
- All infrastructure is provisioned using Terraform modules and AWS security best practices

---

## 📈 Observability

- Prometheus and Grafana

---

## ⚙️ CI/CD Workflows

- **GitHub Actions** build and push Docker images to Amazon ECR
- **ArgoCD** watches the `cluster-state` repo and syncs Helm apps using the App of Apps pattern
- Composite actions like `update-app-tag` are used to automate value file updates after each build
- Charts are versioned and published to GitHub Pages from the `helm-charts` repo on every push to `main`

---

## ✅ Summary

The QRify Platform is a real-world demonstration of production-grade DevOps and GitOps practices, including:

- GitOps-based Kubernetes application delivery
- CI/CD automation across environments
- Cloud-native observability and security
- Multi-environment Helm deployment strategy
- Modular, reusable Terraform infrastructure
- Encrypted secret management using Sealed Secrets

Designed for performance, security, and maintainability in modern cloud-native ecosystems.

---

## 📬 Contact

Built and maintained by Bryan Ramos  
Feel free to reach out for questions, ideas, or collaboration.
