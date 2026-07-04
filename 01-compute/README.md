# 1. Compute Services ⭐⭐⭐⭐⭐

Core services used to run applications on AWS.

**Topics:**
1. [EC2](#1-amazon-ec2)
2. [Lambda](#2-aws-lambda)
3. [Elastic Beanstalk](#3-aws-elastic-beanstalk)
4. [Fargate](#4-aws-fargate)
5. [ECS](#5-amazon-ecs)
6. [EKS](#6-amazon-eks)
7. [Auto Scaling](#7-auto-scaling)

---

# 1. Amazon EC2

## What is it?

Resizable virtual machines (instances) in AWS with full control over OS, CPU, memory, and networking.

## Architecture

```
Users → ALB → EC2 (AZ-a) + EC2 (AZ-b) → EBS Volumes
                  ↓
           Security Group + IAM Role
```

## Components

| Component | Notes |
|-----------|-------|
| **AMI** | OS/app template used to launch instances |
| **Instance Type** | vCPU, RAM, network profile (t, m, c, r, i) |
| **EBS** | Persistent block storage attached to instance |
| **Security Group** | Stateful firewall at instance/ENI level |
| **Key Pair** | SSH/RDP access credentials |
| **Elastic IP** | Static public IPv4 address |
| **User Data** | Bootstrap script run at launch |
| **Launch Template** | Reusable launch config for ASG/instances |

## Comparison

| EC2 | Lambda |
|-----|--------|
| VM, always on | Function, event-driven |
| Full OS control | 15-min max runtime |

## Related AWS Services

- [Auto Scaling](#7-auto-scaling)
- [ALB](../05-load-balancing/README.md#1-application-load-balancer-alb)
- [CloudWatch](../08-monitoring/README.md#1-amazon-cloudwatch)
- [IAM](../15-identity/README.md#1-iam)
- [EBS](../02-storage/README.md#2-amazon-ebs)

## One Line Definition

**EC2 = resizable virtual servers in the cloud.**

---

# 2. AWS Lambda

## What is it?

Serverless compute that runs code in response to events without managing servers.

## Architecture

```
Event → Lambda Function → DynamoDB/S3/SNS → CloudWatch Logs
```

## Components

| Component | Notes |
|-----------|-------|
| **Function** | Your code plus memory, timeout, env vars |
| **Handler** | Entry point called by Lambda runtime |
| **Runtime** | Language environment like Python/Node/Java |
| **Execution Role** | IAM role used by the function |
| **Trigger** | Event source that invokes Lambda |
| **Layer** | Shared library/dependency package |
| **DLQ** | Stores failed async events |

## Related AWS Services

- [API Gateway](../14-serverless/README.md#2-api-gateway)
- [DynamoDB](../03-database/README.md#3-amazon-dynamodb)
- [SQS](../10-messaging/README.md#1-amazon-sqs)

## One Line Definition

**Lambda = run code without provisioning servers.**

---

# 3. AWS Elastic Beanstalk

## What is it?

PaaS that deploys web apps; upload code and Beanstalk provisions EC2, ALB, scaling, and monitoring.

## Architecture

```
Developer → Upload App → Beanstalk → EC2 + ALB + Auto Scaling
```

## Components

| Component | Notes |
|-----------|-------|
| **Application** | Logical container for app versions |
| **Environment** | Running AWS resource stack |
| **Application Version** | Deployable code artifact in S3 |
| **Platform** | OS and runtime stack |
| **Web/Worker Tier** | HTTP app vs SQS background worker |
| **.ebextensions** | Config files for customization |

## Related AWS Services

- [EC2](#1-amazon-ec2)
- [ALB](../05-load-balancing/README.md)
- [RDS](../03-database/README.md#1-amazon-rds)

## One Line Definition

**Beanstalk = easy app deployment platform on AWS.**

---

# 4. AWS Fargate

## What is it?

Serverless container engine for running ECS/EKS workloads without managing EC2 instances.

## Architecture

```
ECR Image → Fargate Task → ALB → Users
```

## Components

| Component | Notes |
|-----------|-------|
| **Task Definition** | CPU, memory, image, ports, roles |
| **Task** | Running copy of a task definition |
| **Service** | Maintains desired task count |
| **Fargate Profile** | Selects EKS pods to run on Fargate |
| **Task ENI** | Dedicated network interface per task |

## Related AWS Services

- [ECS](#3-amazon-ecs)
- [EKS](#4-amazon-eks)
- [ECR](../13-containers/README.md#2-amazon-ecr)

## One Line Definition

**Fargate = serverless engine for containers.**

---

# 5. Amazon ECS

## What is it?

AWS-native container orchestration on EC2 or Fargate.

## Architecture

```
ECR → Task Definition → ECS Service → Tasks → ALB
```

## Components

| Component | Notes |
|-----------|-------|
| **Cluster** | Logical grouping of services/tasks |
| **Task Definition** | Blueprint for containers and resources |
| **Task** | Running instance of a task definition |
| **Service** | Maintains desired tasks and deployments |
| **Capacity Provider** | Connects Fargate/ASG capacity to ECS |

## Related AWS Services

- [ECR](../13-containers/README.md#2-amazon-ecr)
- [Fargate](#5-aws-fargate)
- [ALB](../05-load-balancing/README.md)

## One Line Definition

**ECS = AWS-managed Docker orchestration.**

---

# 6. Amazon EKS

## What is it?

Managed Kubernetes; AWS runs the control plane, you manage EC2 or Fargate worker capacity.

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
| **IRSA** | IAM roles for Kubernetes service accounts |

## Related AWS Services

- [ECR](../13-containers/README.md#2-amazon-ecr)
- [Fargate](#5-aws-fargate)
- [VPC](../04-networking/README.md#1-vpc)

## One Line Definition

**EKS = managed Kubernetes on AWS.**

---

# 7. Auto Scaling

## What is it?

Automatically adjusts EC2 capacity using Auto Scaling Groups and scaling policies.

## Architecture

```
CloudWatch Alarm → Scaling Policy → ASG → Launch/Terminate EC2
```

## Components

| Component | Notes |
|-----------|-------|
| **Auto Scaling Group** | EC2 group with min/max/desired capacity |
| **Launch Template** | AMI, instance type, SG, user data config |
| **Scaling Policy** | Target tracking, step, or scheduled scaling |
| **Health Check** | EC2 or ELB health signal |
| **Warm Pool** | Pre-initialized capacity for faster scale-out |

## Related AWS Services

- [EC2](#1-amazon-ec2)
- [ALB](../05-load-balancing/README.md)
- [CloudWatch](../08-monitoring/README.md#1-amazon-cloudwatch)

## One Line Definition

**Auto Scaling = automatically adjust EC2 capacity based on demand.**
