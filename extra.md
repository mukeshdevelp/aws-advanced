# AWS Study Roadmap (Cloud Engineer / DevOps Engineer)

This roadmap covers the most important AWS services for learning cloud computing, preparing for interviews, and earning AWS Associate certifications.

---

# 1. Compute Services ⭐⭐⭐⭐⭐

Core services used to run applications.

## Topics
- Amazon EC2
- AWS Lambda
- AWS Elastic Beanstalk
- AWS Fargate
- Amazon ECS
- Amazon EKS
- Auto Scaling

**Learn**
- EC2 Instance Types
- AMIs
- EBS Volumes
- User Data
- Launch Templates
- Auto Scaling Groups

---

# 2. Storage Services ⭐⭐⭐⭐⭐

Learn how AWS stores different kinds of data.

## Topics
- Amazon S3
- Amazon EBS
- Amazon EFS
- Amazon FSx
- AWS Storage Gateway

**Learn**
- S3 Storage Classes
- Versioning
- Lifecycle Policies
- Cross-Region Replication (CRR)
- Multipart Upload
- Presigned URLs

---

# 3. Database Services ⭐⭐⭐⭐⭐

Managed databases in AWS.

## Topics
- Amazon RDS
- Amazon Aurora
- Amazon DynamoDB
- Amazon ElastiCache
- Amazon Redshift

**Learn**
- Read Replicas
- Multi-AZ
- Backups
- Scaling
- Caching

---

# 4. Networking ⭐⭐⭐⭐⭐

The most important topic for cloud engineers.

## Topics
- VPC
- Subnets
- Route Tables
- Internet Gateway
- NAT Gateway
- Security Groups
- Network ACLs
- Elastic IP
- VPC Peering
- Transit Gateway
- VPC Endpoints
- AWS PrivateLink
- Site-to-Site VPN
- Direct Connect
- AWS Cloud WAN
- AWS Global Accelerator

**Learn**
- Public vs Private Subnets
- CIDR Blocks
- Routing
- DNS
- Hybrid Networking

---

# 5. Load Balancing ⭐⭐⭐⭐⭐

Distribute traffic across servers.

## Topics
- Application Load Balancer (ALB)
- Network Load Balancer (NLB)
- Gateway Load Balancer (GWLB)
- Classic Load Balancer

**Learn**
- Listener
- Target Group
- Health Checks
- Sticky Sessions

---

# 6. DNS & CDN ⭐⭐⭐⭐

Deliver applications globally.

## Topics
- Amazon Route 53
- Amazon CloudFront

**Learn**
- Hosted Zones
- Routing Policies
- Health Checks
- Edge Locations
- Caching

---

# 7. Security ⭐⭐⭐⭐⭐

One of the most frequently asked interview topics.

## Topics
- IAM
- IAM Roles
- IAM Policies
- IAM Groups
- MFA
- AWS KMS
- Secrets Manager
- Systems Manager Parameter Store
- AWS WAF
- AWS Shield
- Amazon GuardDuty
- Amazon Inspector
- Amazon Macie
- AWS Security Hub
- AWS Certificate Manager (ACM)

**Learn**
- Least Privilege
- Encryption
- Authentication
- Authorization

---

# 8. Monitoring & Logging ⭐⭐⭐⭐

Monitor applications and infrastructure.

## Topics
- Amazon CloudWatch
- AWS CloudTrail
- AWS Config
- AWS X-Ray
- VPC Flow Logs
- AWS Trusted Advisor
- AWS Health Dashboard

**Learn**
- Metrics
- Logs
- Alarms
- Dashboards
- Tracing

---

# 9. DevOps Services ⭐⭐⭐⭐⭐

Automation and CI/CD.

## Topics
- AWS CodeCommit
- AWS CodeBuild
- AWS CodeDeploy
- AWS CodePipeline
- AWS CloudFormation
- AWS Systems Manager

**Learn**
- Infrastructure as Code (IaC)
- Continuous Integration
- Continuous Deployment

---

# 10. Messaging Services ⭐⭐⭐⭐

Event-driven architecture.

## Topics
- Amazon SQS
- Amazon SNS
- Amazon EventBridge
- Amazon MSK (Managed Kafka)

**Learn**
- Queue
- Topic
- Pub/Sub
- FIFO Queue
- Dead Letter Queue

---

# 11. Migration Services ⭐⭐⭐

Move applications and databases to AWS.

## Topics
- AWS Application Migration Service (MGN)
- AWS Database Migration Service (DMS)
- AWS DataSync
- AWS Snowball

---

# 12. Governance ⭐⭐⭐⭐

Manage multiple AWS accounts.

## Topics
- AWS Organizations
- AWS Control Tower
- AWS Resource Access Manager (RAM)
- AWS Config
- Service Control Policies (SCP)
- AWS Budgets
- AWS Cost Explorer

---

# 13. Containers ⭐⭐⭐⭐⭐

Container technologies in AWS.

## Topics
- Docker
- Amazon ECR
- Amazon ECS
- Amazon EKS
- AWS Fargate

**Learn**
- Task
- Service
- Cluster
- Pod
- Deployment
- Service Discovery

---

# 14. Serverless ⭐⭐⭐⭐

Applications without managing servers.

## Topics
- AWS Lambda
- API Gateway
- DynamoDB
- EventBridge
- SNS
- SQS
- AWS Step Functions

---

# 15. Identity Services ⭐⭐⭐⭐

Authentication and user management.

## Topics
- IAM
- IAM Identity Center (AWS SSO)
- Amazon Cognito
- AWS Directory Service

---

# Recommended Learning Order

| Step | Topic |
|------|-------|
| 1 | EC2 |
| 2 | VPC |
| 3 | IAM |
| 4 | S3 |
| 5 | RDS |
| 6 | Auto Scaling |
| 7 | Elastic Load Balancer |
| 8 | Route 53 |
| 9 | CloudFront |
| 10 | Lambda |
| 11 | API Gateway |
| 12 | CloudWatch & CloudTrail |
| 13 | ECR |
| 14 | ECS |
| 15 | EKS |
| 16 | Fargate |
| 17 | SQS |
| 18 | SNS |
| 19 | EventBridge |
| 20 | CloudFormation |
| 21 | Systems Manager |
| 22 | Security Services |
| 23 | Organizations |
| 24 | Control Tower |
| 25 | Cost Optimization |
| 26 | Migration Services |

---

# Interview Priority

## Must Know ⭐⭐⭐⭐⭐
- EC2
- IAM
- VPC
- S3
- RDS
- Route 53
- ELB
- Auto Scaling
- CloudWatch
- CloudTrail
- Lambda
- API Gateway
- ECS
- EKS
- ECR
- Fargate

---

## Important ⭐⭐⭐⭐
- CloudFront
- ACM
- WAF
- Transit Gateway
- VPC Endpoints
- PrivateLink
- VPN
- Direct Connect
- SNS
- SQS
- EventBridge
- CloudFormation
- Systems Manager
- Organizations
- Control Tower

---

## Good to Know ⭐⭐⭐
- FSx
- Storage Gateway
- Snowball
- DMS
- MGN
- Macie
- Inspector
- GuardDuty
- Cloud WAN
- Global Accelerator

---

# AWS Learning Path

```
Linux
    ↓
Networking
    ↓
IAM
    ↓
EC2
    ↓
Storage (S3, EBS, EFS)
    ↓
Database (RDS, DynamoDB)
    ↓
VPC Networking
    ↓
Load Balancer
    ↓
Auto Scaling
    ↓
Monitoring
    ↓
Serverless
    ↓
Containers (Docker → ECR → ECS → EKS → Fargate)
    ↓
CI/CD
    ↓
Infrastructure as Code
    ↓
Security
    ↓
Advanced Networking
```

---

# Goal

After completing this roadmap, you should be comfortable with:

- Designing AWS architectures
- Deploying scalable applications
- Securing AWS resources
- Managing networking
- Building CI/CD pipelines
- Running containerized applications
- Working with serverless architectures
- Preparing for AWS Solutions Architect Associate and DevOps Engineer interviews
