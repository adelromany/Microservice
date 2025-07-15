# AWS Microservices CI/CD Pipeline Project

![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Jenkins](https://img.shields.io/badge/jenkins-%232C5263.svg?style=for-the-badge&logo=jenkins&logoColor=white)
![Terraform](https://img.shields.io/badge/terraform-%235835CC.svg?style=for-the-badge&logo=terraform&logoColor=white)
![Ansible](https://img.shields.io/badge/ansible-%231A1918.svg?style=for-the-badge&logo=ansible&logoColor=white)
![Microservices](https://img.shields.io/badge/Microservices-Architecture-blue)

A complete CI/CD pipeline implementation for microservices on AWS, featuring Jenkins automation, Docker containerization, and multiple deployment strategies with both self-managed Kubernetes and EKS.

## Table of Contents
- [Project Overview](#project-overview)
- [Architecture](#architecture)
- [Implementation Highlights](#implementation-highlights)
- [Getting Started](#getting-started)
- [Pipeline Workflow](#pipeline-workflow)
- [Infrastructure Options](#infrastructure-options)
- [Roadmap](#roadmap)
- [License](#license)

## Project Overview

This project implements a production-grade CI/CD pipeline for microservices with:

- **Branch-per-service** strategy (12 microservices in separate branches)
- **Automated Jenkins pipelines** for each service
- **Docker Hub integration** with versioned tagging
- **Dual deployment options**:
  - Self-managed Kubernetes (Terraform + Ansible)
  - AWS EKS with Fargate
- **Extensible architecture** for additional cloud providers

## Architecture

```mermaid
graph TD
    A[Jenkins] -->|WebhookTriggers| B[Git Repository]
    A -->|Builds/Pushes| C[Docker Hub]
    B --> D[Branch-per-service]
    C --> E[Versioned Container Images]
    E --> F[Self-managed K8s Cluster]
    E --> G[EKS Fargate]
    F & G --> H[Production Deployment]
    
    style A fill:#D33833,color:white
    style C fill:#2496ED,color:white
    style F fill:#326CE5,color:white
    style G fill:#FF9900,color:white
```

## Implementation Highlights

### Branch Structure
```
main/                   # Baseline application
├── cart-service/       # Branch: cart
├── checkout-service/   # Branch: checkout
├── currency-service/   # Branch: currency
├── email-service/      # Branch: email
├── frontend/           # Branch: frontend
├── payment-service/    # Branch: payment
└── ... (12 services total)
```

### Infrastructure Options

1. **Self-managed Kubernetes**:
   - Terraform-provisioned EC2 instances
   - Ansible-configured K8s cluster
   - Custom networking and storage

2. **AWS EKS with Fargate**:
   - Serverless container execution
   - Auto-scaling built-in
   - Reduced operational overhead

## Getting Started

### Prerequisites
- AWS Account with IAM permissions
- Jenkins 2.346+ with Blue Ocean plugin
- Docker Hub account
- Terraform 1.2+
- Ansible 2.12+

## Pipeline Workflow

### Current Implementation
```mermaid
sequenceDiagram
    participant J as Jenkins
    participant G as GitHub
    participant D as Docker
    participant K as Kubernetes
    
    J->>G: Poll for changes
    G->>J: Commit detected
    J->>D: Build image (service:date)
    J->>D: Push to registry
    D-->>J: Build status
    J->>K: Deploy based on cluster_type
    K-->>J: Deployment status
```

### Tagging Convention
| Environment  | Tag Format              | Example               |
|--------------|-------------------------|-----------------------|
| Development  | `service:YYYYMMDD`      | `frontend:20240531`   |
| Production   | `service:semver`        | `cart:1.2.0`          |

## Infrastructure Options

### Option 1: Self-managed K8s
```mermaid
graph LR
    T[Terraform] -->|Provision| E[EC2 Instances]
    A[Ansible] -->|Configure| K[Kubernetes Cluster]
    K --> D[Docker Runtime]
    D --> R[Running Pods]
```

### Option 2: EKS Fargate
```mermaid
graph LR
    E[EKS Cluster] --> F[Fargate Profile]
    F --> P[Managed Pods]
    P --> A[Auto-scaling]
```

## Roadmap

### Immediate Priorities
- [x] Jenkins pipeline for image building
- [x] Terraform EKS infrastructure
- [x] Self-managed K8s option

### Future Enhancements
- [ ] Image vulnerability scanning (Trivy integration)
- [ ] Unit testing stage in pipeline
- [ ] Blue-green deployment strategy
- [ ] CloudWatch monitoring dashboard
- [ ] Multi-region deployment

## License
Apache License 2.0 (inherited from original GoogleCloudPlatform/microservices-demo)
