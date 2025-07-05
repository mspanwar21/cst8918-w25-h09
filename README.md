
# CST8918 - W25 - H09: AKS Cluster with Terraform

This project provisions an **Azure Kubernetes Service (AKS)** cluster using **Terraform**, and deploys a sample multi-tier web application including:

- **Frontend:** Vue.js store front
- **Backend:** Node.js order service, Rust product service
- **Message broker:** RabbitMQ

## 🚀 Features

- Infrastructure as Code (IaC) using Terraform
- AKS cluster with:
  - 1-3 node auto-scaling pool
  - `Standard_B2s` VM size
  - System assigned managed identity
- Outputs kubeconfig for `kubectl`
- Kubernetes deployment of sample app with services and deployments

## 📂 Project structure

```
.
├── main.tf               # Terraform configuration for AKS cluster
├── sample-app.yaml        # Kubernetes manifest for multi-tier app
└── .gitignore             # Excludes .terraform and other files from Git
```

## ⚙️ How to deploy

### 1️⃣ Initialize Terraform

```bash
terraform init
```

### 2️⃣ Plan and apply

```bash
terraform plan
terraform apply
```
👉 Type `yes` when prompted.

### 3️⃣ Extract kubeconfig

```bash
echo "$(terraform output kube_config)" > kubeconfig
export KUBECONFIG=./kubeconfig
kubectl get nodes
```
✅ Remove `<<<eof` and `eof` if they appear in the kubeconfig file.

### 4️⃣ Deploy application

```bash
kubectl apply -f sample-app.yaml
kubectl get pods
kubectl get svc
```
👉 Access the app using the external IP of the `store-front` service.

## 🧹 Cleanup

```bash
terraform destroy
```
👉 This will remove all Azure resources created by Terraform.

## 📌 Notes

- Ensure `Microsoft.ContainerService` provider is registered in your Azure subscription:
  ```bash
  az provider register --namespace Microsoft.ContainerService
  ```
- `.terraform/` is excluded from Git to prevent pushing large provider binaries.

