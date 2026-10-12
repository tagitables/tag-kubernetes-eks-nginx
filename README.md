# Amazon EKS | Kubernetes Nginx Deployment

A hands-on cloud infrastructure engineering project demonstrating Kubernetes container orchestration, AWS load balancing, workload identity and HTTP endpoint verification on Amazon Elastic Kubernetes Service (EKS).

## Technology Stack

AWS EKS · Kubernetes · kubectl · Nginx · AWS Load Balancer · Linux (WSL2 Ubuntu) · Git · GitHub

## Architecture

```text
HTTP Client (curl / Browser)
          |
          v
   AWS Load Balancer
          |
          v
Kubernetes LoadBalancer Service
     tag-nginx-service
          |
          v
    Nginx Deployment
   tag-nginx-deployment
          |
          v
       Nginx Pod
       Port 80

Workload identity:
tag-nginx-service-account
```

## Kubernetes Configuration

| Resource | Configuration |
|---|---|
| Cluster | shared-eks-cluster |
| Region | us-east-1 |
| Namespace | tag-eks-activity |
| Deployment | tag-nginx-deployment |
| Replicas | 1 |
| Container Image | nginx:latest |
| Container Port | 80 |
| Service | tag-nginx-service |
| Service Type | LoadBalancer |
| ServiceAccount | tag-nginx-service-account |

## Implementation

1. Connected to the shared EKS cluster using AWS CLI and kubeconfig.
2. Created a dedicated Kubernetes namespace.
3. Deployed a containerized Nginx application using a Deployment manifest.
4. Exposed the application through an AWS Load Balancer using a Kubernetes Service.
5. Configured a dedicated Kubernetes ServiceAccount.
6. Verified the deployed application through HTTP requests.

## Deployment Commands

```bash
kubectl apply -f kubernetes/service-account.yaml
kubectl apply -f kubernetes/nginx-deployment.yaml
kubectl apply -f kubernetes/nginx-service.yaml
```

The manifests assume the namespace already exists and that kubectl is authenticated to the intended EKS cluster.

## Operational Verification

```bash
kubectl get pods -n tag-eks-activity
kubectl get svc -n tag-eks-activity
kubectl get deployment tag-nginx-deployment -n tag-eks-activity
```

## Verified Results

**Pod readiness**

```text
READY   STATUS    RESTARTS
1/1     Running   0
```

**ServiceAccount**

```text
tag-nginx-service-account
```

**HTTP response**

```text
HTTP/1.1 200 OK
<title>Welcome to nginx!</title>
<h1>Welcome to nginx!</h1>
```

## Deployment Evidence

Recorded terminal outputs are available in the `demo/` directory:

- `pod-status.txt` — Nginx Pod readiness
- `service-status.txt` — Kubernetes LoadBalancer Service
- `service-account-status.txt` — workload ServiceAccount
- `curl-response.txt` — HTTP response and Nginx welcome page

## Learning Outcomes

- Kubernetes declarative infrastructure configuration
- Container deployment and Pod lifecycle management
- AWS EKS cluster connectivity
- Kubernetes service networking and external load balancing
- Kubernetes ServiceAccount configuration
- CLI-based operational testing and deployment evidence

## Project Context

Cloud Infrastructure Engineering — Assignment 3.7, Kubernetes Part 1.

This implementation uses an existing shared Amazon EKS cluster. Cluster provisioning, automated CI/CD deployment and infrastructure creation with Terraform are outside the scope of this exercise.
