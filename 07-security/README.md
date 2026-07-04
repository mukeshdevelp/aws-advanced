# 7. Security ⭐⭐⭐⭐⭐

Identity, encryption, and threat detection — top interview topic.

**Topics:**
1. [IAM](#1-iam)
2. [KMS](#2-aws-kms)
3. [Secrets Manager](#3-aws-secrets-manager)
4. [WAF](#4-aws-waf)
5. [GuardDuty](#5-amazon-guardduty)
6. [Inspector](#6-amazon-inspector)
7. [Security Hub](#7-aws-security-hub)

---

# 1. IAM

## What is it?

Identity and Access Management — **Users**, **Groups**, **Roles**, **Policies**, and **MFA** for AWS access control.

## Architecture

```
User/Role → Policy Evaluation (Deny > Allow > default Deny) → AWS Resource
```

## Components

| Component | Notes |
|-----------|-------|
| **Users** | Individual identity with permanent credentials |
| **Groups** | Collection of users sharing policies |
| **Roles** | Temporary credentials via STS; for services |
| **Policies** | JSON documents defining Allow/Deny permissions |
| **MFA** | Extra auth factor for users and root |
| **Permission Boundary** | Max permissions a user/role can receive |
| **SCP** | Org-level guardrail (via Organizations) |

## Advantages

- Granular, free, roles for services (no hardcoded keys)

## Limitations

- Policy complexity
- Global service
- 5000 users/account
## Comparison

| IAM User | IAM Role |
|----------|----------|
| Permanent creds | Temporary via STS |
| Humans | Services/cross-account |

## Related AWS Services

- [Identity Center](../15-identity/README.md#2-iam-identity-center)
- [CloudTrail](../08-monitoring/README.md#2-aws-cloudtrail)
- [Organizations](../12-governance/README.md#1-aws-organizations)

## Exam Tips

- Roles for services
- Explicit Deny wins
- IAM is global, not regional

## One Line Definition

**IAM = control who can do what in AWS.**

---

# 2. AWS KMS

## What is it?

Key Management Service — create/manage encryption keys for EBS, S3, RDS, Secrets Manager.

## Architecture

```
Service/App → KMS API → CMK → Encrypt/Decrypt → CloudTrail audit
```

## Components

| Component | Notes |
|-----------|-------|
| **CMK** | Customer Managed Key with full control |
| **AWS Managed Key** | AWS-created key for a specific service |
| **Data Key** | Plaintext key for envelope encryption |
| **Key Policy** | Resource policy controlling key access |
| **Key Rotation** | Automatic annual rotation of CMK material |

## Related AWS Services

- [S3](../02-storage/README.md#1-amazon-s3)
- [Secrets Manager](#3-aws-secrets-manager)

## Exam Tips

Envelope encryption: data key encrypts data, CMK encrypts data key. CMK = $1/month.

## One Line Definition

**KMS = managed encryption keys for AWS.**

---

# 3. AWS Secrets Manager

## What is it?

Store and **automatically rotate** secrets (DB creds, API keys). Encrypted with KMS.

## Components

| Component | Notes |
|-----------|-------|
| **Secret** | Encrypted credential or API key value |
| **Rotation Lambda** | Function that rotates secret on schedule |
| **Version Staging** | AWSCURRENT vs AWSPENDING during rotation |
| **Resource Policy** | Cross-account secret access control |

## Comparison

| Secrets Manager | Parameter Store |
|-----------------|-----------------|
| Auto rotation | Manual |
| Secrets focus | Config parameters |
| Paid | Standard tier free |

## Related AWS Services

- [KMS](#2-aws-kms)
- [RDS](../03-database/README.md#1-amazon-rds)
- [Systems Manager](../09-devops/README.md#6-aws-systems-manager)

## One Line Definition

**Secrets Manager = store and rotate secrets securely.**

---

# 4. AWS WAF

## What is it?

**WAF** — Layer 7 firewall (SQLi, XSS, rate limiting) for ALB/CloudFront/API GW. **Shield** — DDoS protection (Standard free, Advanced $3K/mo).

## Architecture

```
User → Shield → WAF → ALB/CloudFront → App
```

## Components

| Component | Notes |
|-----------|-------|
| **Web ACL** | Container for WAF rules attached to resource |
| **Rule** | Managed or custom match conditions |
| **Shield Standard** | Free automatic DDoS protection |
| **Shield Advanced** | Paid 24/7 DDoS Response Team support |
| **Rate-based Rule** | Block IPs exceeding request threshold |

## Comparison

| WAF | Security Group |
|-----|----------------|
| Layer 7 HTTP | Instance firewall |
| Inspects requests | Port/protocol rules |

## Related AWS Services

- [ALB](../05-load-balancing/README.md)
- [CloudFront](../06-dns-cdn/README.md#2-amazon-cloudfront)

## Exam Tips

WAF attaches to ALB, CloudFront, API Gateway. Shield Standard = automatic, free.

## One Line Definition

**WAF = web firewall; Shield = DDoS protection.**

---

# 5. Amazon GuardDuty

## What is it?

ML-powered threat detection analyzing CloudTrail, VPC Flow Logs, and DNS logs.

## Architecture

```
Logs → GuardDuty ML → Findings → SNS/EventBridge → Security Hub
```

## Components

| Component | Notes |
|-----------|-------|
| **Detector** | GuardDuty instance per Region/account |
| **Finding** | Detected threat with severity level |
| **Threat Intel** | AWS and third-party malicious IP feeds |
| **Suppression Rule** | Filter known benign findings |

## Comparison

| GuardDuty | Inspector |
|-----------|-----------|
| Threat detection | Vulnerability scanning |
| ML on logs | CVE scanning |

## Related AWS Services

- [Security Hub](#7-aws-security-hub)
- [CloudTrail](../08-monitoring/README.md#2-aws-cloudtrail)

## One Line Definition

**GuardDuty = intelligent threat detection.**

---

# 6. Amazon Inspector

## What is it?

**Inspector** — automated CVE scanning for EC2, ECR, Lambda. **Macie** — ML discovery of PII in S3.

## Components

| Component | Notes |
|-----------|-------|
| **Inspector Assessment** | Scan EC2, ECR, Lambda for CVEs |
| **Finding** | Vulnerability with severity and remediation |
| **Macie Job** | Scheduled S3 bucket classification scan |
| **Custom Data Identifier** | Regex pattern for sensitive data types |

## Comparison

| Inspector | Macie |
|-----------|-------|
| Vulnerabilities | S3 data classification |
| EC2/ECR/Lambda | PII/financial data |

## Related AWS Services

- [ECR](../13-containers/README.md#2-amazon-ecr)
- [S3](../02-storage/README.md#1-amazon-s3)

## One Line Definition

**Inspector = vulnerability scanning; Macie = S3 sensitive data discovery.**

---

# 7. AWS Security Hub

## What is it?

**Security Hub** — aggregate findings from GuardDuty, Inspector, Macie, third-party tools. **ACM** — free TLS certs for ALB/CloudFront (auto-renew, not exportable).

## Architecture

```
GuardDuty + Inspector + Macie → Security Hub → Compliance Score
ACM → DNS Validation → Attach to ALB/CloudFront → HTTPS
```

## Components

| Component | Notes |
|-----------|-------|
| **Security Hub** | Aggregates findings from AWS security services |
| **Compliance Standard** | CIS, PCI DSS benchmark checks |
| **ACM Certificate** | Free TLS cert with auto-renewal |
| **DNS Validation** | Prove domain ownership via Route 53 record |

## Related AWS Services

- [GuardDuty](#5-amazon-guardduty)
- [CloudFront](../06-dns-cdn/README.md#2-amazon-cloudfront)

## Exam Tips

Security Hub = aggregator, not detector. ACM certs cannot be exported. DNS validation preferred.

## One Line Definition

**Security Hub = centralized security dashboard; ACM = free TLS certificates.**
