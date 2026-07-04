# 13. Containers ⭐⭐⭐⭐⭐

Docker fundamentals through AWS container orchestration.

**Topics:**
1. [Docker](#1-docker)
2. [ECR](#2-amazon-ecr)
3. [ECS](#3-amazon-ecs)
4. [EKS](#4-amazon-eks)
5. [Fargate](#5-aws-fargate)

---

# 1. Docker

## What is it?

Platform to build, ship, and run apps in **containers** — portable units packaging code with dependencies.

## Architecture

```
Dockerfile → docker build → Image → Registry → docker run → Container
```

## Components

| Component | Notes |
|-----------|-------|
| **Dockerfile** | Text recipe to build an image |
| **Image** | Read-only template for containers |
| **Container** | Running instance of an image |
| **Registry** | Store/share images (ECR, Docker Hub) |
| **Volume** | Persistent storage for containers |
| **Compose** | Multi-container app definition tool

## Comparison

| Container | VM |
|-----------|-----|
| Shares OS kernel | Full OS per VM |
| Seconds to start | Minutes to start |
| Lightweight | Heavy |

## Related AWS Services

- [ECR](#2-amazon-ecr)
- [ECS](#3-amazon-ecs)
- [CodeBuild](../09-devops/README.md#2-aws-codebuild)

## One Line Definition

**Docker = platform to build and run containers.**

---

# 2. Amazon ECR

## What is it?

Managed Docker **registry** — store images for ECS/EKS with scanning and lifecycle policies.

## Architecture

```
docker build → docker push → ECR Repo → ECS/EKS pull → Run
                                  ↓
                           Inspector Scan
```

## Components

| Component | Notes |
|-----------|-------|
| **Repository** | Named container image store |
| **Image Tag** | Version label for an image |
| **Lifecycle Policy** | Auto-delete old/unused images |
| **Scan on Push** | Vulnerability scan when image uploaded |
| **Cross-Region Replication** | Copy images to other Regions

## Related AWS Services

- [ECS](#3-amazon-ecs)
- [EKS](#4-amazon-eks)
- [Inspector](../07-security/README.md#6-amazon-inspector)

## Exam Tips

- URI: `{account}.dkr.ecr.{region}.amazonaws.com/{repo}:{tag}`

## One Line Definition

**ECR = managed Docker container image registry.**

---

# 3. Amazon ECS

## What is it?

AWS-native container orchestration — run Docker on **EC2** or **Fargate** without Kubernetes.

## Architecture

```
ECR → Task Definition → ECS Service → Tasks → ALB
```

## Components

| Component | Notes |
|-----------|-------|
| **Cluster** | Logical grouping of ECS resources |
| **Task Definition** | Blueprint for containers and resources |
| **Task** | Running instance of a task definition |
| **Service** | Maintains desired task count |
| **Capacity Provider** | Links Fargate/ASG capacity to ECS |
| **Service Discovery** | DNS-based service lookup in ECS

## Comparison

| ECS | EKS |
|-----|-----|
| AWS-native, simpler | Kubernetes standard |
| Less portable | Rich ecosystem |

## Related AWS Services

- [ECR](#2-amazon-ecr)
- [Fargate](#5-aws-fargate)
- [Compute ECS](../01-compute/README.md#3-amazon-ecs)

## One Line Definition

**ECS = AWS-managed Docker orchestration.**

---

# 4. Amazon EKS

## What is it?

Managed **Kubernetes** — AWS runs control plane; you manage worker nodes (EC2/Fargate).

## Architecture

```
kubectl → EKS Control Plane → Worker Nodes → Pods → ALB Ingress
```

## Components

| Component | Notes |
|-----------|-------|
| **Control Plane** | Managed Kubernetes API server |
| **Node Group** | EC2 workers running pods |
| **Pod** | Smallest Kubernetes deployable unit |
| **Deployment** | Manages pod replicas and updates |
| **IRSA** | IAM roles for service accounts |
| **Fargate Profile** | Runs selected pods on Fargate

## Related AWS Services

- [ECR](#2-amazon-ecr)
- [Fargate](#5-aws-fargate)
- [Compute EKS](../01-compute/README.md#4-amazon-eks)

## Exam Tips

IRSA = IAM Roles for Service Accounts (pod-level permissions). Control plane = $0.10/hr.

## One Line Definition

**EKS = managed Kubernetes on AWS.**

---

# 5. AWS Fargate

## What is it?

Serverless container engine for **ECS/EKS** — no EC2 instances to manage.

## Architecture

```
ECR → Task Definition (CPU/RAM) → Fargate runs Task → ALB
```

## Components

| Component | Notes |
|-----------|-------|
| **Task Definition** | CPU/memory and container settings |
| **Task** | One running serverless container unit |
| **Service** | Keeps desired Fargate task count |
| **Platform Version** | Fargate runtime/feature version |
| **Task ENI** | Dedicated network interface per task |

## Comparison

| Fargate | EC2 Launch Type |
|---------|-----------------|
| Serverless | Manage nodes |
| Per-task billing | Instance billing |
| No DaemonSets | Full K8s features |

## Related AWS Services

- [ECS](#3-amazon-ecs)
- [EKS](#4-amazon-eks)
- [Compute Fargate](../01-compute/README.md#5-aws-fargate)

## One Line Definition

**Fargate = serverless engine for running containers.**
