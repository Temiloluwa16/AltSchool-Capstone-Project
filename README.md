# Deployment of the Sock-Shop Application and Prometheus on EKS using Terraform and Jenkins 

## Project Overview

This project demostrates the deployment of a microservices-based application, specifically the Sock-Shop, using modern DevOps practices on an Elastic Kubernetes Service (EKS) cluster. The deployment is fully automated using Infrastructure as Code (IaC) tools, including Terraform and Jenkins, with a focus on scalability, monitoring, and security.

## Table of Contents

- [Project Overview](#project-overview)
-[Prerequisites](#prerequisites)
-[Directory Structure](#directory-structure)
- [Setup Instructions](#setup-instructions)
  - [1. Environment Setup](#1-environment-setup)
  - [2. Provisioning Infrastructure](#2-provisioning-infrastructure)
  - [3. Deploying the Application and Services](#deploying-the-application-and-services)
  - [4. Configuring DNS and SSL](#configuring-DNS-and-SSL)
  - [5.Running the CI/CD Pipelines](#CI/CD-pipeline)
  - [6. Accessing the Application](#accessing-the-application)
- [7. Destroying the Setup](#destroying-the-setup)
- [Security Considerations](#security-considerations)
- [Conclusion](#conclusion)

## Prerequisites

- **Operating System**: Windows (using WSL for Linux-based tools)
- **Tools**: Docker, Terraform, Ansible, kubectl, Helm, Jenkins, AWs CLI
- **Cloud Provider**: AWS (for EKS, Route 53 and ACM)
- **Kubernetes**: A running Kubernetes cluster


## Setup Instructions

### 1. Environment Setup

- **Install Docker**: Follow [Docker installation guide](https://docs.docker.com/get-docker/).
- **Install Terraform**: Download and install from the [Terraform website](https://www.terraform.io/downloads).
- **Install Ansible**: Use WSL to install Ansible.
- **Install kubectl**: Download and install `kubectl` from [Kubernetes website](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/).
- **Install Helm**: Download Helm from the [Helm website](https://helm.sh/docs/intro/install/).

### 2. Provisioning Infrastructure

1. **Navigate to the Terraform directory**:
   \`\`\`bash
   cd terraform/
   \`\`\`
2. **Initialize Terraform**:
   \`\`\`bash
   terraform init
   \`\`\`
3. **Apply the Terraform configuration**:
   \`\`\`bash
   terraform apply
   \`\`\`
   This command provisions the infrastructure for the Kubernetes cluster.

### 3. Kubernetes Cluster Setup

1. **Obtain kubeconfig**: Retrieve your Kubernetes configuration file from your cloud provider.
2. **Set up Helm**:
   \`\`\`bash
   helm repo add stable https://charts.helm.sh/stable
   helm repo update
   \`\`\`

### 4. Deploying the Application

1. **Create Helm Charts for Socks Shop**:
   - Navigate to the \`helm/\` directory and customize the charts.
2. **Deploy the application using Helm**:
   \`\`\`bash
   helm install socks-shop ./helm/socks-shop --namespace socks-shop
   \`\`\`

### 5. Setting Up Monitoring and Logging

1. **Install Prometheus**:
   \`\`\`bash
   helm install prometheus stable/prometheus --namespace monitoring
   \`\`\`
2. **Install Grafana**:
   \`\`\`bash
   helm install grafana stable/grafana --namespace monitoring
   \`\`\`

### 6. Enabling HTTPS

1. **Install cert-manager**:
   \`\`\`bash
   helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --version v1.7.1 --set installCRDs=true
   \`\`\`
2. **Configure Let’s Encrypt Issuer and Ingress**:
   - Create the issuer and ingress YAML files and apply them using \`kubectl\`.

### 7. CI/CD Pipeline

1. **Create GitHub Actions workflow**: Set up a \`.github/workflows/ci-cd.yaml\` file to automate the deployment.
2. **Push code to GitHub**: Ensure the repository is public, and all configurations are committed.

## Security Considerations

- **Ansible Vault**: Use Ansible Vault to encrypt sensitive information.
- **Network Security**: Implement network perimeter security rules for the Kubernetes cluster.
- **HTTPS**: Ensure that the application is accessible over HTTPS using Let’s Encrypt.

## Project Structure

\`\`\`
.
├── terraform/           # Terraform configuration files
├── helm/                # Helm charts for the Socks Shop application
├── ansible/             # Ansible playbooks and roles (if applicable)
├── .github/workflows/   # CI/CD pipeline configuration
└── README.md            # Project documentation
\`\`\`

## Conclusion

This project showcases the deployment of a microservices-based application on Kubernetes using modern DevOps practices. By leveraging Infrastructure as Code, we ensure a reliable, secure, and repeatable deployment process. The use of monitoring, logging, and CI/CD further enhances the robustness of the deployment.
