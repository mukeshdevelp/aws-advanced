# AWS Networking & Security Services - Quick Reference

This README provides a **short interview-oriented summary** of important AWS networking, security, governance, and container services.

---

# 1. AWS CloudWatch

**Purpose:** Monitoring and observability service.

### Used For
- Monitor AWS resources
- Collect metrics
- Store logs
- Create alarms
- Build dashboards

**Example:** Alert when EC2 CPU exceeds 80%.

---

# 2. AWS CloudTrail

**Purpose:** Records AWS API activity.

### Used For
- Audit AWS account activity
- Track who created/deleted resources
- Security investigations
- Compliance

**Example:** Find who deleted an EC2 instance.

---

# 3. AWS WAF (Web Application Firewall)

**Purpose:** Protects web applications from common web attacks.

### Protects Against
- SQL Injection (SQLi)
- Cross-Site Scripting (XSS)
- Bots
- IP blocking
- Rate limiting

### Works With
- CloudFront
- Application Load Balancer (ALB)
- API Gateway

---

# 4. AWS Certificate Manager (ACM)

**Purpose:** Manages SSL/TLS certificates.

### Features
- Free public certificates
- Automatic renewal
- HTTPS encryption
- Easy integration with AWS services

### Used For
- Secure websites
- HTTPS APIs

---

# 5. AWS Transit Gateway

**Purpose:** Central hub connecting multiple VPCs and on-premises networks.

### Connects
- VPCs
- Site-to-Site VPN
- Direct Connect

**Think:** Hub-and-Spoke Networking

---

# 6. VPC Endpoints

**Purpose:** Private access to AWS services without using the internet.

### Types
- **Gateway Endpoint** → S3, DynamoDB
- **Interface Endpoint** → Most AWS services

### Benefits
- Private communication
- No NAT Gateway required (for supported services)
- Improved security

---

# 7. VPC Flow Logs

**Purpose:** Records network traffic metadata.

### Captures
- Source IP
- Destination IP
- Port
- Protocol
- ACCEPT / REJECT

### Does NOT Capture
- Packet contents (payload)

---

# 8. AWS PrivateLink

**Purpose:** Privately expose services across VPCs or AWS accounts.

### Uses
- Interface Endpoint
- Network Load Balancer (NLB)

### Best For
- SaaS applications
- Shared APIs
- Internal enterprise services

---

# 9. AWS Site-to-Site VPN

**Purpose:** Securely connects on-premises networks to AWS.

### Uses
- IPsec encryption
- Two VPN tunnels
- Customer Gateway
- Virtual Private Gateway (VGW) or Transit Gateway

### Best For
- Hybrid Cloud

---

# 10. AWS Control Tower

**Purpose:** Automates multi-account AWS setup and governance.

### Features
- Landing Zone
- Guardrails
- Account Factory
- AWS Organizations integration

### Best For
- Enterprise environments

---

# 11. AWS API Gateway

**Purpose:** Creates, secures, publishes, and manages APIs.

### Features
- Authentication
- Authorization
- Rate limiting
- Monitoring
- REST APIs
- HTTP APIs
- WebSocket APIs

### Backend Support
- Lambda
- EC2
- Containers
- HTTP endpoints

---

# 12. AWS Cloud WAN

**Purpose:** Centralized management of a global enterprise network.

### Connects
- AWS Regions
- Branch offices
- Data centers
- VPCs

### Best For
- Global organizations

---

# 13. AWS Global Accelerator

**Purpose:** Improves global application performance and availability.

### Features
- Static Anycast IPs
- Health checks
- Automatic failover
- Uses AWS Global Network

### Works With
- ALB
- NLB
- EC2

---

# 14. AWS Fargate

**Purpose:** Serverless compute engine for containers.

### Works With
- Amazon ECS
- Amazon EKS

### Benefits
- No server management
- Automatic scaling
- Pay only for CPU and memory used

---

# 15. AWS Resource Access Manager (RAM)

**Purpose:** Share AWS resources across multiple AWS accounts.

### Can Share
- Subnets
- Transit Gateway
- Route 53 Resolver Rules
- IPAM Pools
- Other supported resources

### Best For
- Multi-account environments

---

# Important Interview Comparisons

## CloudWatch vs CloudTrail

| CloudWatch | CloudTrail |
|------------|------------|
| Monitoring | Auditing |
| Metrics & Logs | API Calls |
| CPU, Memory, Logs | Who created/deleted resources |

---

## WAF vs Security Group vs NACL

| WAF | Security Group | NACL |
|-----|----------------|------|
| Layer 7 | Layer 4 | Layer 4 |
| Protects web apps | Instance firewall | Subnet firewall |
| SQLi/XSS Protection | Port filtering | Subnet traffic filtering |

---

## Transit Gateway vs VPC Peering

| Transit Gateway | VPC Peering |
|-----------------|-------------|
| Central hub | Direct connection |
| Highly scalable | Best for few VPCs |
| Supports transitive routing | No transitive routing |

---

## VPC Endpoint vs PrivateLink

| VPC Endpoint | PrivateLink |
|--------------|-------------|
| Private access to AWS services | Private access to your own services |
| Gateway & Interface Endpoints | Uses Interface Endpoints |

---

## Site-to-Site VPN vs Direct Connect

| Site-to-Site VPN | Direct Connect |
|------------------|----------------|
| Internet-based | Dedicated private connection |
| Encrypted | Private connectivity |
| Lower cost | Higher bandwidth & consistency |

---

## Cloud WAN vs Transit Gateway

| Cloud WAN | Transit Gateway |
|-----------|-----------------|
| Global | Regional |
| Enterprise WAN | Regional network hub |

---

## Global Accelerator vs CloudFront

| Global Accelerator | CloudFront |
|--------------------|------------|
| Improves routing | Caches content |
| TCP/UDP | HTTP/HTTPS |
| No caching | Edge caching |

---

## ECS vs Fargate

| ECS | Fargate |
|-----|----------|
| Container orchestration | Serverless compute for containers |
| Uses EC2 or Fargate | No server management |

---

## IAM vs RAM

| IAM | RAM |
|-----|-----|
| User & Role permissions | Resource sharing across accounts |

---

# One-Line Definitions

| Service | Definition |
|----------|------------|
| CloudWatch | Monitors AWS resources using metrics, logs, dashboards, and alarms. |
| CloudTrail | Records AWS API calls for auditing and compliance. |
| WAF | Protects web applications from Layer 7 attacks. |
| ACM | Manages SSL/TLS certificates for AWS services. |
| Transit Gateway | Central networking hub connecting VPCs and on-premises networks. |
| VPC Endpoint | Private access to AWS services without the public internet. |
| VPC Flow Logs | Records metadata about VPC network traffic. |
| PrivateLink | Securely shares services between VPCs and AWS accounts. |
| Site-to-Site VPN | Secure IPsec connection between AWS and on-premises networks. |
| Control Tower | Automates secure multi-account AWS environments. |
| API Gateway | Creates, secures, and manages APIs. |
| Cloud WAN | Centrally manages global enterprise networking. |
| Global Accelerator | Improves global application performance and availability. |
| Fargate | Serverless compute engine for containers. |
| RAM | Securely shares AWS resources across accounts. |

---

# Category-wise Services

| Category | AWS Services |
|----------|--------------|
| **Monitoring** | CloudWatch, CloudTrail, VPC Flow Logs |
| **Security** | WAF, ACM |
| **Networking** | Transit Gateway, VPC Endpoints, PrivateLink, Site-to-Site VPN, Cloud WAN, Global Accelerator |
| **Governance** | Control Tower, RAM |
| **Application** | API Gateway |
| **Containers** | Fargate |

---

# Interview Keywords

| Service | Remember This |
|----------|---------------|
| CloudWatch | **Monitor** |
| CloudTrail | **Audit** |
| WAF | **Web Protection** |
| ACM | **SSL Certificates** |
| Transit Gateway | **Network Hub** |
| VPC Endpoint | **Private AWS Access** |
| Flow Logs | **Network Traffic Logs** |
| PrivateLink | **Private Service Sharing** |
| Site-to-Site VPN | **Hybrid Connectivity** |
| Control Tower | **Multi-Account Governance** |
| API Gateway | **API Management** |
| Cloud WAN | **Global Enterprise Network** |
| Global Accelerator | **Global Traffic Optimization** |
| Fargate | **Serverless Containers** |
| RAM | **Resource Sharing** |
