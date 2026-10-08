# 🚀 MediCare GitOps Repository

![Kubernetes](https://img.shields.io/badge/Kubernetes-EKS-blue)
![Argo CD](https://img.shields.io/badge/Argo_CD-GitOps-orange)
![AWS](https://img.shields.io/badge/AWS-EKS-orange)
![GitOps](https://img.shields.io/badge/GitOps-Enabled-purple)
![YAML](https://img.shields.io/badge/Manifests-YAML-blue)

## 📌 Project Overview

This repository contains the **Kubernetes manifests used by Argo CD to deploy and manage the MediCare application on Amazon EKS**.

This repository follows the **GitOps deployment model**, where Git is treated as the source of truth for Kubernetes application configuration.

Instead of manually changing Kubernetes resources, deployment changes are committed to this repository.

Argo CD continuously monitors the repository and synchronizes the desired state with the Kubernetes cluster.

---

# 🔄 GitOps Architecture

```text
       Application Repository
                │
                ▼
        GitHub Actions CI/CD
                │
                ▼
          Amazon ECR
                │
                ▼
       GitOps Repository
                │
                ▼
             Argo CD
                │
                ▼
          Amazon EKS
                │
        ┌───────┴────────┐
        ▼                ▼
   Backend            Frontend
```

---

# 🎯 GitOps Principle

The repository follows:

```text
Git = Source of Truth
```

The desired Kubernetes state is stored in Git.

For example:

```text
GitOps Repository
       │
       ▼
Desired Image Version
       │
       ▼
Argo CD
       │
       ▼
Kubernetes Cluster
       │
       ▼
Running Application
```

---

# 📁 Repository Structure

```text
medicare-gitops-repo/
│
├── environments/
│   │
│   ├── development/
│   │   ├── namespace.yaml
│   │   ├── backend-deploy.yaml
│   │   ├── frontend-deploy.yaml
│   │   └── ingress.yaml
│   │
│   └── production-mock/
│       └── deployment.yaml
│
└── README.md
```

---

# ☸️ Kubernetes Resources

## Namespace

The application runs inside:

```text
medicare
```

Namespace manifest:

```text
environments/development/namespace.yaml
```

---

# 🖥️ Backend Deployment

File:

```text
environments/development/backend-deploy.yaml
```

The backend deployment contains:

```text
2 replicas
```

Container port:

```text
5000
```

Health endpoint:

```text
/api/health
```

The deployment uses:

### Readiness Probe

Determines whether the application is ready to receive traffic.

### Liveness Probe

Determines whether the application is still healthy.

---

# 🌐 Frontend Deployment

File:

```text
environments/development/frontend-deploy.yaml
```

The frontend runs:

```text
2 replicas
```

It is served using Nginx.

Container port:

```text
80
```

---

# 🌍 Kubernetes Ingress

File:

```text
environments/development/ingress.yaml
```

The project uses:

```text
AWS Application Load Balancer
```

with:

```yaml
ingressClassName: alb
```

The routing configuration is:

```text
/api/* → medicare-backend
/*     → medicare-frontend
```

---

# 🔀 Traffic Flow

```text
                  🌍 Internet
                       │
                       ▼
             AWS Application Load Balancer
                       │
              ┌────────┴────────┐
              │                 │
              ▼                 ▼
           /api/*                /*
              │                 │
              ▼                 ▼
      medicare-backend   medicare-frontend
              │                 │
              ▼                 ▼
          Port 5000          Port 80
```

---

# 🚀 Argo CD

Argo CD is responsible for continuously synchronizing this repository with Amazon EKS.

Argo CD application:

```text
medicare-development
```

Repository:

```text
https://github.com/adirai30/medicare-gitops-repo.git
```

Path:

```text
environments/development
```

Destination:

```text
in-cluster
```

Namespace:

```text
medicare
```

---

# 🔄 Automated Synchronization

Argo CD automated synchronization is enabled with:

```text
Self Heal
```

This means Argo CD can detect configuration drift and restore the desired state defined in Git.

---

# 🧩 Complete Deployment Flow

```text
1️⃣ Developer pushes application code
          │
          ▼
2️⃣ GitHub Actions starts
          │
          ▼
3️⃣ Tests execute
          │
          ▼
4️⃣ Security scan executes
          │
          ▼
5️⃣ Docker images are built
          │
          ▼
6️⃣ Images pushed to Amazon ECR
          │
          ▼
7️⃣ GitOps image tag updated
          │
          ▼
8️⃣ Argo CD detects Git change
          │
          ▼
9️⃣ Argo CD synchronizes EKS
          │
          ▼
🔟 New application version runs
```

---

# 🛠️ Kubernetes Verification

Check namespace:

```bash
kubectl get namespace medicare
```

Check pods:

```bash
kubectl get pods -n medicare
```

Check deployments:

```bash
kubectl get deployments -n medicare
```

Check services:

```bash
kubectl get services -n medicare
```

Check ingress:

```bash
kubectl get ingress -n medicare
```

---

# 🔍 Check Application Health

```bash
kubectl get pods -n medicare
```

Expected:

```text
medicare-backend     Running
medicare-frontend    Running
```

Backend health endpoint:

```text
/api/health
```

Expected:

```json
{
  "status": "UP"
}
```

---

# 🔄 Updating an Application Version

When GitHub Actions updates the image tag:

```yaml
image: <ECR-IMAGE>:<COMMIT-SHA>
```

Argo CD detects the Git change.

Then:

```text
Git Change
   ↓
Argo CD
   ↓
Kubernetes Deployment
   ↓
New ReplicaSet
   ↓
New Pods
```

---

# 🔐 GitOps Benefits

### 📝 Auditability

Every deployment change is stored in Git.

### 🔁 Rollback

Previous Git commits can be used to restore an earlier desired state.

### 🔍 Visibility

The desired Kubernetes configuration is visible in the repository.

### 🤖 Automation

Argo CD removes the need for manually applying every application deployment.

### 🛡️ Drift Detection

Argo CD can detect when the cluster differs from the Git-defined desired state.

---

# 🎯 Project Objectives

This repository demonstrates:

* ☸️ Kubernetes manifests
* 🚀 Argo CD
* 🔄 GitOps
* 🌐 AWS ALB
* 🏥 Healthcare application deployment
* 🔁 Automated synchronization
* 🛡️ Self-healing
* 📦 ECR image deployment
* ☁️ Amazon EKS

---

# 👨‍💻 Author

**Aditya Rai**

Cloud / DevOps Engineer

GitHub:

https://github.com/adirai30

---

# ⭐ GitOps Highlights

```text
✅ Git as source of truth
✅ Argo CD
✅ Automated synchronization
✅ Self-healing
✅ Kubernetes deployments
✅ AWS ALB
✅ EKS
✅ Version-controlled manifests
✅ Backend + frontend workloads
```

---

# 📜 License

This project is created for learning, portfolio, demonstration, and DevOps practice purposes.
