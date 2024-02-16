# Legal Term API Application

This repository contains a Legal Term API application that serves a subset of definitions spelled out in the United States Code.

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Run the Application](#run-the-application)
- [Deploy Secrets using SOPS](#deploy-secrets-using-sops)
- [Deployment to Kubernetes](#deployment-to-kubernetes)
- [Monitoring](#monitoring)
- [Contributing](#contributing)
- [License](#license)

## 🛠️ Prerequisites

Before running the application or deploying secrets, ensure you have the following prerequisites installed:

- [Docker](https://www.docker.com/)
- [Helm](https://helm.sh/)
- [SOPS](https://github.com/mozilla/sops)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## 🚀 Installation

Clone the repository:

git clone git@github.com:karinatitov/legal-terms-api .git
cd fastapi

🏃 Run the Application
Build the Docker image: `docker build -t legal-terms-api  . `

Run the Docker container: `docker run -p 8000:8000 legal-terms-api`

The FastAPI application will be accessible at http://localhost:8000.

## 🔐 Deploy Secrets using SOPS
Encrypt your secrets using sops. Create a file, e.g., sensitive_key.yaml:
```
sensitive_key: your_sensitive_value
```
Encrypt the file using sops:
```
sops -e sensitive_key.yaml > sops_secrets/sensitive_key
```
Update the Helm chart values to enable secrets and specify the encrypted values:
```
secrets:
  enabled: true
  sensitive_key: "encrypted:sops_secrets/sensitive_key"
```

Package and deploy the Helm chart:
```
helm package legal-terms-api
helm install legal-terms-api legal-terms-api-0.1.0.tgz
```

## ☸️ Deployment to Kubernetes
To deploy the FastAPI application to Kubernetes, ensure you have a Kubernetes cluster available.

Install the Helm chart:
```
helm install legal-terms-api legal-terms-api-0.1.0.tgz
```
If you included secrets, they will be securely mounted into the application containers.

# 🛠️ Create Resources using Helm
If you want to manually create or modify resources using Helm, you can use the following commands:

Create a ConfigMap:
```
helm upgrade  legal-terms-api legal-terms-api-0.1.0.tgz --set secrets.enabled=false
```
This will disable the secrets part and allow you to create ConfigMaps independently.

Update Deployment:
```
helm upgrade legal-terms-api  legal-terms-api-0.1.0.tgz
```
This will update the deployment with any changes in the Helm chart.

## 📊 Monitoring
Use Kubernetes tools like kubectl for basic monitoring:

```
kubectl get pods
kubectl logs your-pod-name
kubectl describe deployment your-app-deployment
```

## For more advanced monitoring, consider integrating with tools like Prometheus and Grafana.

# legal-term-api

An HTTP API for legal terms.

## Endpoints

* `GET /terms`: list all available terms.
* `GET /definitions?term=<term>`: get the definition of a term.


* i will not create actual sops secrets in this example, but we can talk about this approach, since today i think it's one of the most reliable and convenient ways to deploy secrets

** also the ci/cd is failing because it is a public repo and i wouldn't want to expose any sensitive login info, we can also go over this part