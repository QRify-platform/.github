# 🚀 QRify Platform

Welcome to the GitHub organization for the **QRify Platform** — a cloud-native system for generating and storing QR codes from user-submitted URLs. QRify is built using modern **DevOps** and **GitOps** principles, and is designed to run securely and scalably in the cloud using **Kubernetes**.

---

## 🧱 Architecture Overview

The platform is composed of containerized microservices and supporting infrastructure:

- **Frontend**: A Next.js web app that lets users submit URLs and view QR codes
- **API**: A FastAPI service that generates QR codes and uploads them to AWS S3
- **Storage**: AWS S3 for storing and serving generated QR images
- **Secrets Management**: Sealed Secrets used for encrypted secret delivery via Git

All services are deployed to **Amazon EKS**, managed via **ArgoCD** (App of Apps pattern), and integrated with Prometheus and Loki for observability. Infrastructure is provisioned using **Terraform**, and deployments are environment-specific (`dev`, `prod`) using **Helm**.  **Blue-Green Deployments** are implemented to ensure zero-downtime rollouts and safe rollbacks.

---

## 🔧 Technologies Used

* **Next.js** – Frontend application
* **FastAPI** – Backend API service
* **Docker** – Containerization for all components
* **Kubernetes (EKS)** – Container orchestration
* **ArgoCD** – GitOps-based continuous delivery
* **Helm** – Reusable and environment-specific chart management
* **GitHub Actions** – CI pipelines for builds and image pushes
* **Terraform** – Infrastructure as Code for VPC, EKS, S3, IAM, etc.
* **AWS Services**
  * 🧠 EKS – Manages Kubernetes clusters to run containerized applications.

  * 📦 S3 – Stores and serves generated QR code images.

  * 🌐 Route 53 – Handles DNS for custom domains.

  * 🔐 IAM – Controls secure access to AWS resources using IRSA.

  * 📥 ECR – Stores and manages Docker container images.


  
* **Sealed Secrets** – GitOps-compatible secrets encryption and delivery
* **IRSA (IAM Roles for Service Accounts)** – Secure cloud permissions for API pods
* **Prometheus** – Metrics collection and alerting
* **Grafana** – Dashboards and visualizations for app health, performance, and logs
* **Loki** – Centralized log aggregation for Kubernetes workloads

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

QRify runs in two separate environments, each with its corresponding services.

- **Development**
  - Triggered on push to `main`
  - Uses `values.dev.yaml`
  - Lower resources and debug logging

- **Production**
  - Triggered via Git tag or manual promotion
  - Uses `values.prod.yaml`
  - Higher replicas, optimized logging, restricted accesss

Environment configs are separated using Helm value overrides, and deployed independently through ArgoCD.

<img width="700" alt="Screenshot 2025-06-20 at 1 01 20 AM" src="https://github.com/user-attachments/assets/793cc2cc-b275-4487-8adf-dcd469de14e5" />

---

## 🔄 Blue-Green Deployments 🟦 🟩

To minimize downtime and enable safe, reliable releases, the platform uses **Blue-Green Deployments** via ArgoCD and Kubernetes. This approach maintains two parallel environments (`blue` and `green`) and routes traffic only to the healthy one. Benefits include:

* **Zero-downtime deployments**: Switch traffic seamlessly between versions.
* **Safe rollbacks**: Instantly revert to the previous version if needed.
* **Visual diffing**: ArgoCD UI makes version comparison easy before promotion.

Routing between environments can be managed using Kubernetes services or ingress rules, and is fully automated in the CI/CD pipeline.

<img width="700" alt="Screenshot 2025-06-20 at 1 33 01 AM" src="https://github.com/user-attachments/assets/5cdb0b09-c311-4fe9-a727-3cd6adb6de87" />

---
## 📈 Observability and Logging

* **Prometheus** – Collects metrics from Kubernetes workloads and application endpoints, enabling real-time monitoring of resource usage, request durations, and system health.
* **Grafana** – Visualizes metrics and logs through custom dashboards. Used to track application performance, error rates, request patterns, and namespace-level insights.
* **Loki** – Aggregates logs from all containers in the cluster using Promtail. Enables efficient querying of logs based on labels like `namespace`, `pod`, and `container` for debugging and auditing.
  
* **Custom Dashboards** – Built in Grafana to monitor:
  * Total logs per namespace
  * Error rates and top error messages
  * HTTP status code distribution (500s, 404s, etc.)
  * Top API endpoints and latency patterns (if logs include duration)
 
    
<p float="left">
  <img src="https://github.com/user-attachments/assets/02a8fddf-f9b7-45e0-ad3c-d4ec5d583151" style="margin-right: 5%;" width="45%" />
  <img src="https://github.com/user-attachments/assets/f67135bc-d958-46f8-a24e-c8ac3f3ca672" width="45%" />
</p>


---

## 🔒 Security & Cloud Best Practices

- **IRSA** (IAM Roles for Service Accounts) ensures secure access from the API to S3 without exposing credentials
- **Sealed Secrets** enable encrypted secret storage in Git, decrypted only in the target Kubernetes cluster
- TLS termination via **ACM** and DNS with **Route 53**
- Supports both **ALB** and **NGINX** Ingress Controllers
- All infrastructure is provisioned using Terraform modules and AWS security best practices
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
- Multi-environment Blue-Green Helm deployment strategy
- Modular, reusable Terraform infrastructure
- Encrypted secret management using Sealed Secrets

Designed for performance, security, and maintainability in modern cloud-native ecosystems.

---

## 📬 Contact

Built and maintained by Bryan Ramos  
Feel free to reach out for questions, ideas, or collaboration.
LinkedIn: https://www.linkedin.com/in/bryan-ramos-174826279/
