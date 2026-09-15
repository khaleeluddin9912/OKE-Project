# Deploy 2048 Game on Oracle Kubernetes Engine (OKE)

![Screenshot 2023-08-03 at 7 57 15 PM](https://github.com/iam-veeramalla/aws-devops-zero-to-hero/assets/43399466/93b06a9f-67f9-404f-b0ad-18e3095b7353

This project is the OCI equivalent of the AWS DevOps Zero-to-Hero Day-22 Kubernetes 2048 application.

The application is deployed on **Oracle Kubernetes Engine (OKE)** and exposed to the internet using the **OCI Native Ingress Controller**.

## Architecture

```text
User
  |
  v
OCI Load Balancer
  |
  v
OCI Native Ingress Controller
  |
  v
Kubernetes Ingress
  |
  v
Kubernetes Service (NodePort)
  |
  v
2048 Pods
```

## Project Structure

```text
day-22/
├── README.md
├── prerequisites.md
├── installing-oke.md
├── oci-native-ingress-controller.md
└── 2048-app-deploy-ingress.md
```

## Technologies Used

* Oracle Cloud Infrastructure (OCI)
* Oracle Kubernetes Engine (OKE)
* Kubernetes
* OCI Native Ingress Controller
* OCI Load Balancer
* Helm
* kubectl
* OCI CLI
* Docker

## Project Steps

### 1. Prerequisites

Install and configure:

* OCI CLI
* kubectl
* Helm

See:

`prerequisites.md`

### 2. Install OKE

Create:

* OKE cluster
* OKE node pool
* Kubernetes kubeconfig

See:

`installing-oke.md`

### 3. Install OCI Native Ingress Controller

Install the OCI Native Ingress Controller using Helm.

See:

`oci-native-ingress-controller.md`

### 4. Deploy the 2048 Application

Deploy:

* 2048 Deployment
* Kubernetes Service
* OCI Ingress

See:

`2048-app-deploy-ingress.md`

## Result

The 2048 application is accessible through the public IP created by the OCI Load Balancer.

```text
http://<INGRESS-PUBLIC-IP>
```

## Cleanup

Delete the Kubernetes application:

```powershell
kubectl delete namespace game-2048
```

Delete the OKE cluster when the lab is no longer required:

```powershell
oci ce cluster delete `
  --cluster-id <CLUSTER_OCID> `
  --force
```

This project demonstrates how the AWS EKS/Fargate + AWS Load Balancer Controller approach can be implemented using OKE and the OCI Native Ingress Controller.
