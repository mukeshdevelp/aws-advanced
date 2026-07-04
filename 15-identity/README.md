# 15. Identity Services ⭐⭐⭐⭐

Authentication and user management — who can access your applications and AWS resources.

**Topics:**
1. [IAM](#1-iam)
2. [IAM Identity Center](#2-iam-identity-center)
3. [Cognito](#3-amazon-cognito)
4. [Directory Service](#4-aws-directory-service)

---

# 1. IAM

## What is it?

Identity and Access Management — control access to AWS resources. Define users, groups, roles, and policies for authorization.

## Why do we use it?

- Least privilege access for humans and services
- IAM Roles for EC2/Lambda (no hardcoded credentials)
- Cross-account access and federation

## Architecture

```
User/Role → Policy Evaluation → Allow/Deny → AWS Resource
                ↓
         Identity + Resource + SCP + Permission Boundary
```

## Components

| Component | Notes |
|-----------|-------|
| **Users** | Human identities with credentials |
| **Groups** | Collection of users with shared policies |
| **Roles** | Temporary credentials for services/users |
| **Policies** | JSON permissions (managed/inline) |
| **MFA** | Extra authentication factor |
| **Access Keys** | Programmatic access for users |
| **Permission Boundary** | Max permissions cap for entity |
| **STS** | Issues temporary security credentials |

## Workflow

1. Create Role/User
2. Attach Policy
3. Enable MFA
4. Entity Accesses AWS
5. CloudTrail Audits

## Advantages

- Granular permissions, free, role-based access, temporary credentials via STS, global service.

## Limitations

- Policy complexity at scale. 5000 IAM users per account
- Does not manage application users (use Cognito)

## Comparison

| IAM | Cognito |
|-----|---------|
| AWS resource access | Application user pools |
| Admin/developer access | End-user authentication |
| Policies | JWT tokens, social login |

## Related AWS Services

- [Security IAM](../07-security/README.md#1-iam)
- [CloudTrail](../08-monitoring/README.md#2-aws-cloudtrail)
- [Organizations](../12-governance/README.md#1-aws-organizations)

## Exam Tips

- See full details in [Security section](../07-security/README.md#1-iam)
- Roles for services
- Explicit Deny > Allow > default Deny

## One Line Definition

**IAM = control who can access AWS resources.**

---

# 2. IAM Identity Center

## What is it?

Successor to AWS SSO. Centralized access management for multiple AWS accounts and cloud applications (SAML 2.0).

## Why do we use it?

- Single sign-on to AWS accounts and business apps
- Multi-account permission sets
- Integration with Active Directory / external IdP

## Architecture

```
User → Identity Center Portal → Permission Set → AWS Account Access
                    ↓
              External IdP (AD/Okta/Google)
```

## Components

| Component | Notes |
|-----------|-------|
| **Identity Center Instance** | SSO service in the account/Region |
| **Permission Set** | Template of AWS account permissions |
| **Account Assignment** | Maps user/group to account + permission set |
| **Identity Source** | Built-in, AD, or external IdP users |
| **Application Assignment** | SSO access to SaaS applications |

## Workflow

Enable Identity Center → Configure Identity Source → Create Permission Sets → Assign Users to Accounts → SSO Login

## Advantages

Centralized multi-account SSO, no IAM users in each account, MFA support, application SSO.

## Limitations

Requires Organizations. Permission sets map to IAM roles (indirect). Learning curve for setup.

## Common Use Cases

- Enterprise multi-account access, replace individual IAM users, contractor access
- SaaS app SSO

## Related AWS Services

- [Organizations](../12-governance/README.md#1-aws-organizations)
- [IAM](#1-iam)
- [Control Tower](../12-governance/README.md#2-aws-control-tower)

## Exam Tips

Identity Center = SSO for humans across accounts. Permission Set = template for IAM role in target account. Replaces AWS SSO.

## One Line Definition

**Identity Center = centralized SSO for AWS accounts and apps.**

---

# 3. Amazon Cognito

## What is it?

Authentication service for web and mobile apps. User Pools (directory) and Identity Pools (AWS credential federation).

## Why do we use it?

- Sign-up/sign-in for application users
- Social identity providers (Google, Facebook, Apple)
- Federated access to AWS services from mobile/web apps

## Architecture

```
App User → Cognito User Pool → JWT Tokens → API Gateway / App Backend
                ↓
         Identity Pool → Temporary AWS Credentials → S3/DynamoDB
```

## Components

| Component | Notes |
|-----------|-------|
| **User Pool** | App user directory and authentication |
| **App Client** | App-specific auth configuration |
| **Identity Pool** | Federated AWS credential provider |
| **Hosted UI** | Cognito-managed sign-in page |
| **Social/SAML IdP** | External identity provider integration |
| **Lambda Triggers** | Custom auth workflow hooks |
| **Groups** | Role-like grouping inside user pool |

## Workflow

User Signs Up/In → Cognito Authenticates → JWT Issued → App Validates Token → Optional AWS Credentials via Identity Pool

## Advantages

Managed user directory, social login, MFA, customizable UI, Lambda triggers for custom logic, JWT tokens.

## Limitations

User Pool limits (40M users). Not for AWS admin access (use IAM/Identity Center). Pricing per MAU.

## Common Use Cases

Mobile app authentication, SaaS user management, guest access, federated AWS access from client apps.

## Comparison

| User Pool | Identity Pool |
|-----------|---------------|
| User directory + auth | AWS credential federation |
| JWT tokens | Temporary AWS creds |
| Sign-up/sign-in | Access S3/DynamoDB from app |

## Related AWS Services

- [API Gateway](../14-serverless/README.md#2-api-gateway)
- [IAM](#1-iam)
- [Lambda](../01-compute/README.md#1-aws-lambda)

## Exam Tips

User Pool = authentication (JWT). Identity Pool = authorize AWS service access. Cognito ≠ IAM (app users vs AWS access).

## One Line Definition

**Cognito = user authentication and federation for apps.**

---

# 4. AWS Directory Service

## What is it?

Managed Microsoft Active Directory in AWS. AWS Managed Microsoft AD, Simple AD, and AD Connector for hybrid identity.

## Why do we use it?

- Run Active Directory workloads in AWS
- Join EC2/FSx/RDS to a domain
- Extend on-premises AD to AWS (AD Connector)

## Architecture

```
On-Prem AD ←── Trust ──→ AWS Managed Microsoft AD → EC2/FSx/RDS Domain Join
                              ↓
                        AD Connector (proxy to on-prem)
```

## Components

| Component | Notes |
|-----------|-------|
| **AWS Managed Microsoft AD** | Full-feature managed AD in AWS |
| **Simple AD** | Basic Samba-compatible directory |
| **AD Connector** | Proxy to on-premises Active Directory |
| **Domain Controllers** | Authentication servers in the directory |
| **Trust Relationship** | Cross-domain authentication link |
| **DHCP Options Set** | DNS/domain resolution for VPC clients |

## Workflow

Create Directory → Configure VPC → Join EC2/FSx to Domain → Users Authenticate via AD → Group Policies Apply

## Advantages

Fully managed AD, multi-AZ DCs, trust with on-prem AD, integrates with Office 365, FSx, RDS.

## Limitations

AWS Managed AD cost per DC. Simple AD = limited features. AD Connector requires on-prem connectivity.

## Common Use Cases

Windows workload domain join, FSx for Windows authentication, hybrid AD, enterprise app LDAP integration.

## Comparison

| AWS Managed AD | Simple AD |
|----------------|-----------|
| Full AD features | Basic AD subset |
| Multi-AZ, trusts | Single AZ, no trusts |
| Production use | Small/simple apps |

## Related AWS Services

- [FSx](../02-storage/README.md#4-amazon-fsx)
- [EC2](../01-compute/README.md#1-amazon-ec2)
- [Identity Center](#2-iam-identity-center)

## Exam Tips

Managed Microsoft AD = full AD, multi-AZ. Simple AD = cut-down, cheaper. AD Connector = proxy to on-prem AD.

## One Line Definition

**Directory Service = managed Active Directory in AWS.**
