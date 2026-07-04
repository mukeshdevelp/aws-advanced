# AWS Study Roadmap

Comprehensive AWS learning guide for Cloud Engineers and DevOps Engineers — interview prep, hands-on practice, and Associate-level certification readiness.

Based on the full topic list in [extra.md](./extra.md).

---

## Sections

| # | Section | Topics | Priority |
|---|---------|--------|----------|
| 1 | [Compute Services](./01-compute/README.md) | EC2, Lambda, Beanstalk, Fargate, ECS, EKS, Auto Scaling | ⭐⭐⭐⭐⭐ |
| 2 | [Storage Services](./02-storage/README.md) | S3, EBS, EFS, FSx, Storage Gateway | ⭐⭐⭐⭐⭐ |
| 3 | [Database Services](./03-database/README.md) | RDS, Aurora, DynamoDB, ElastiCache, Redshift | ⭐⭐⭐⭐⭐ |
| 4 | [Networking](./04-networking/README.md) | VPC, Subnets, NAT, VPN, Transit Gateway, and more | ⭐⭐⭐⭐⭐ |
| 5 | [Load Balancing](./05-load-balancing/README.md) | ALB, NLB, GWLB, Classic LB | ⭐⭐⭐⭐⭐ |
| 6 | [DNS & CDN](./06-dns-cdn/README.md) | Route 53, CloudFront | ⭐⭐⭐⭐ |
| 7 | [Security](./07-security/README.md) | IAM, KMS, WAF, GuardDuty, ACM, and more | ⭐⭐⭐⭐⭐ |
| 8 | [Monitoring & Logging](./08-monitoring/README.md) | CloudWatch, CloudTrail, Config, X-Ray | ⭐⭐⭐⭐ |
| 9 | [DevOps Services](./09-devops/README.md) | CodePipeline, CloudFormation, Systems Manager | ⭐⭐⭐⭐⭐ |
| 10 | [Messaging Services](./10-messaging/README.md) | SQS, SNS, EventBridge, MSK | ⭐⭐⭐⭐ |
| 11 | [Migration Services](./11-migration/README.md) | MGN, DMS, DataSync, Snowball | ⭐⭐⭐ |
| 12 | [Governance](./12-governance/README.md) | Organizations, Control Tower, SCPs, Budgets | ⭐⭐⭐⭐ |
| 13 | [Containers](./13-containers/README.md) | Docker, ECR, ECS, EKS, Fargate | ⭐⭐⭐⭐⭐ |
| 14 | [Serverless](./14-serverless/README.md) | Lambda, API Gateway, Step Functions | ⭐⭐⭐⭐ |
| 15 | [Identity Services](./15-identity/README.md) | IAM, Identity Center, Cognito | ⭐⭐⭐⭐ |

---

## Recommended Learning Order

1. [EC2](./01-compute/README.md#1-amazon-ec2) → [VPC](./04-networking/README.md#1-vpc) → [IAM](./15-identity/README.md#1-iam)
2. [S3](./02-storage/README.md#1-amazon-s3) → [RDS](./03-database/README.md#1-amazon-rds)
3. [Auto Scaling](./01-compute/README.md#7-auto-scaling) → [Load Balancer](./05-load-balancing/README.md#1-application-load-balancer-alb)
4. [Route 53](./06-dns-cdn/README.md#1-amazon-route-53) → [CloudFront](./06-dns-cdn/README.md#2-amazon-cloudfront)
5. [Lambda](./14-serverless/README.md#1-aws-lambda) → [API Gateway](./14-serverless/README.md#2-api-gateway)
6. [CloudWatch](./08-monitoring/README.md#1-amazon-cloudwatch) → [CloudTrail](./08-monitoring/README.md#2-aws-cloudtrail)
7. [ECR](./13-containers/README.md#2-amazon-ecr) → [ECS](./13-containers/README.md#3-amazon-ecs) → [EKS](./13-containers/README.md#4-amazon-eks)
8. [SQS](./10-messaging/README.md#1-amazon-sqs) → [SNS](./10-messaging/README.md#2-amazon-sns) → [EventBridge](./10-messaging/README.md#3-amazon-eventbridge)
9. [CloudFormation](./09-devops/README.md#5-aws-cloudformation) → [Organizations](./12-governance/README.md#1-aws-organizations)

---

## Interview Priority

**Must Know ⭐⭐⭐⭐⭐**
- [EC2](./01-compute/README.md#1-amazon-ec2)
- [IAM](./15-identity/README.md#1-iam)
- [VPC](./04-networking/README.md#1-vpc)
- [S3](./02-storage/README.md#1-amazon-s3)
- [RDS](./03-database/README.md#1-amazon-rds)
- [Route 53](./06-dns-cdn/README.md#1-amazon-route-53)
- [ELB](./05-load-balancing/README.md)
- [Lambda](./14-serverless/README.md#1-aws-lambda)
- [ECS/EKS](./13-containers/README.md)

**Important ⭐⭐⭐⭐**
- [CloudFront](./06-dns-cdn/README.md#2-amazon-cloudfront)
- [WAF](./07-security/README.md#4-aws-waf)
- [Transit Gateway](./04-networking/README.md#5-transit-gateway)
- [CloudFormation](./09-devops/README.md#5-aws-cloudformation)

**Good to Know ⭐⭐⭐**
- [Migration](./11-migration/README.md)
- [GuardDuty](./07-security/README.md#5-amazon-guardduty)
- [Global Accelerator](./04-networking/README.md#8-aws-global-accelerator)

---

## Learning Path

```
Linux → Networking → IAM → EC2 → Storage → Database → VPC
  → Load Balancer → Auto Scaling → Monitoring → Serverless
  → Containers → CI/CD → IaC → Security → Advanced Networking
```

---

## Goal

After completing this roadmap, you should be comfortable with:

- Designing AWS architectures
- Deploying scalable applications
- Securing AWS resources
- Managing networking
- Building CI/CD pipelines
- Running containerized applications
- Working with serverless architectures
- Preparing for AWS Solutions Architect Associate and DevOps Engineer interviews
