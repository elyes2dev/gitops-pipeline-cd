# Pipeline-CICD 🔁

This repository contains the **application code** (frontend + backend) for a full-stack CI/CD demo built with Spring Boot, Angular, and MySQL. It integrates with a GitOps-based CD workflow defined in a separate manifests repository.

---

## 🚀 Project Structure

- **backend/**  
  A Spring Boot application that connects to MySQL and provides REST APIs.
- **frontend/**  
  An Angular SPA that communicates with the backend.

---

## ⚙️ CI Pipeline (Jenkins)

The **Jenkinsfile** automates:

- Cloning the application repo from GitHub
- Building and testing backend (Maven) and frontend (npm)
- Static code analysis with SonarQube
- Building and pushing Docker images to Docker Hub (frontend & backend)
- Vulnerability scanning using Trivy
- Triggering the CD pipeline to update manifests via a Jenkins job for GitOps

---

## 🔄 CD Pipeline (GitOps - manifests repo)

The CD pipeline runs separately and:

- Clones the GitOps repository
- Updates Docker image tags in Kubernetes manifest YAMLs
- Pushes changes back to GitHub
- ArgoCD detects updates and automatically deploys to **Amazon EKS**

---

## 🛠 Tech Stack

| Component         | Tools / Technologies                                 |
|------------------|------------------------------------------------------|
| **Backend**      | Spring Boot, Maven, MySQL                            |
| **Frontend**     | Angular, npm                                          |
| **CI**           | Jenkins, GitHub, SonarQube, Trivy                    |
| **Containerization** | Docker, Docker Hub                               |
| **CD / GitOps**  | Kubernetes manifests, GitOps repo, ArgoCD, Amazon EKS |
| **Infrastructure** | AWS EC2 (for Jenkins and code analysis tools)       |

---

## 📂 Repositories

- **App Code (CI):** [elyes2dev/pipeline-cicd](https://github.com/elyes2dev/pipeline-cicd)  
- **GitOps Manifests (CD):** [elyes2dev/gitops-pipeline-cd](https://github.com/elyes2dev/gitops-pipeline-cd)

---

## ✅ How it works (overview)

1. Developer pushes code to GitHub → triggers Jenkins.
2. Jenkins builds, tests, analyzes, and creates Docker images.
3. Trivy scans images; pipeline triggers CD job on success.
4. CD pipeline updates Kubernetes manifests and ArgoCD deploys to EKS.

---

## 📌 Why this matters

Using this pipeline, you get end‑to‑end CI/CD automation with integrated quality checks and security scanning, following GitOps best practices for cloud-native deployment.

---

## 🔍 Next steps

- Check out the CD repo to explore manifest templating and GitOps logic.
- Run pipelines locally or adapt Jenkinsfile for GitHub Actions.
- Extend with integration tests, Helm charts, or production-ready features.

---

Enjoy building and deploying with confidence! 🚢
