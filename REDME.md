# Enterprise DevSecOps Cloud-Native Platform

## Overview

This project demonstrates a complete enterprise-grade **DevSecOps pipeline** implementation for a cloud-native platform. It replaces traditional manual deployment with a fully automated **GitOps-based Kubernetes architecture** integrating CI/CD, continuous security scanning, and full-stack observability.

The platform consists of **13 microservices** deployed on **Azure Kubernetes Service (AKS)**, provisioned through Terraform and managed through a centralized **GitOps repository**, with ArgoCD continuously syncing the desired state from Git to the cluster and a centrelized **Trade-Template repository** that holds all the security , scripts and template files reusable for all the microservices . 
> **Confidentiality note:** the implementation source code is not published due to confidentiality agreements. This repository documents the architecture, workflows, dashboards, and engineering practices behind the platform.

---

## Infrastructure Components

| Component | Purpose |
|---|---|
| GitLab CI/CD | CI/CD automation (build, test, scan, sign, deploy) |
| SonarQube | SAST (code quality & security analysis) |
| Trivy | Filesystem & container vulnerability scanning |
| OWASP Dependency Check | Dependency vulnerability analysis (SCA) |
| Cosign | Container image signing & integrity verification |
| Kaniko | Rootless container image builds |
| Azure Container Registry (ACR) | Primary Docker image storage |
| GitLab Registry | Internal Docker image registry |
| GitOps Repository | Single source of truth (Helm + K8s manifests) |
| ArgoCD | GitOps deployment controller (auto-sync cluster state) |
| Azure Kubernetes Service (AKS) | Container orchestration |
| Terraform | Infrastructure-as-Code provisioning (Azure) |
| Prometheus + Grafana | Monitoring & visualization |
| Alertmanager | Alert routing & notifications (email) |
| Wazuh | Centralized SIEM & threat detection |

---

## 🔁 GitOps Strategy

The **GitOps repository** is the single source of truth for the entire platform.

It contains:

- Helm charts for all 13 microservices
- Environment-specific configuration values
- ArgoCD Application manifests
- Prometheus alerting rules

**Advantages:**

- No configuration drift
- Fully version-controlled infrastructure
- Full audit trail of every change
- Automated deployments driven entirely by Git commits
- Easy rollback to any previous known-good state

---

## Tech Stack

**DevOps / GitOps**

- Docker (multi-stage builds, optimized images)
- Kaniko (rootless, daemonless image builds)
- Kubernetes (AKS)
- ArgoCD (GitOps continuous deployment)
- Helm (templated Kubernetes manifests)
- GitLab CI/CD (pipeline automation)
- Terraform (Infrastructure as Code)
- Azure Container Registry / GitLab Registry

---

## 🔐 Security (Shift-Left + Shift-Right)

| Tool | Purpose | Stage |
|---|---|---|
| SonarQube | SAST (code vulnerabilities, code smells) | CI |
| OWASP Dependency Check | Dependency vulnerability analysis | CI |
| Trivy (Filesystem) | Filesystem & dependency scanning | CI |
| Trivy (Image) | Container image vulnerability scanning | Post-build |
| Cosign | Image signing & integrity verification | Post-build |
| Wazuh | Runtime threat detection & log correlation | Runtime |

---

## 🔄 CI/CD Pipeline Stages

**1. Validation**
- Manifest & configuration validation
- Helm Lint

**2. Build & Test**
- Unit testing
- Dependency installation

**3. Static & Dependency Security Analysis**
- SonarQube static code analysis
- OWASP Dependency Check

**4. Filesystem Security Scanning**
- Trivy filesystem scan (source & dependencies)

**5. Container Build**
- Multi-stage, rootless image build via Kaniko

**6. Container Security Scanning**
- Trivy image scan (vulnerabilities in built image)

**7. Image Signing**
- Cosign signs the verified image

**8. Artifact Publishing**
- Push signed images to Azure Container Registry and GitLab Registry

**9. GitOps Deployment**
- Update GitOps repository with the new image tag
- ArgoCD detects the change and auto-syncs the cluster

**10. Continuous Runtime Monitoring**
- Wazuh, Prometheus, and Grafana monitor the deployed workload

---

## 🔒 Security Highlights

- No secrets committed to Git
- SAST (SonarQube) enforced before merge
- Dependency vulnerabilities blocked via OWASP Dependency Check
- Trivy scans both filesystem and container images
- Only Cosign-signed images are eligible for deployment
- Wazuh provides continuous runtime threat detection
- Full separation between local development and Azure production infrastructure

---

## Monitoring & Observability

**Prometheus**

Used for:
- Cluster metrics collection
- Node CPU, memory, and disk monitoring
- Kubernetes workload observability
- Alert rule evaluation

**Grafana**

Provides centralized visualization of:
- Cluster health
- Node resource usage
- Namespace-level metrics
- Pod status and restarts
- Application performance

**Alerting (Alertmanager)**

| Alert | Condition | Severity |
|---|---|---|
| High CPU Usage | > 85% (3 min) | Warning |
| High Memory Usage | > 85% (3 min) | Warning |
| Disk Pressure | > 80% (2 min) | Warning |
| CrashLoopBackOff | Pod restarts detected | Critical |
| Deployment Mismatch | Desired ≠ Available pods | Critical |

Alerts are routed via email through Alertmanager.

**Wazuh (SIEM)**

Provides:
- Log aggregation across the cluster
- Threat detection
- Vulnerability monitoring
- Security event correlation
- Incident investigation support

---

## Demo

👉 [Watch Demo Video](https://polytechintl-my.sharepoint.com/:v:/g/personal/m_souadi21155_pi_tn/IQBpKZDVJ2ifSo6oesYEy8pNARmTOFKXEbesATfYH130ydw?e=eYKA7q&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D)

---

## Confidentiality Notice

The implementation source code is not publicly available due to confidentiality and intellectual property restrictions. This repository exists to showcase the architecture, engineering practices, infrastructure design, and DevSecOps workflow developed during this graduation project.