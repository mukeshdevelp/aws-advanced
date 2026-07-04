# 12. Governance ⭐⭐⭐⭐

Multi-account management, compliance, cost control, and resource sharing.

**Topics:** [Organizations](#aws-organizations) · [Control Tower](#aws-control-tower) · [RAM](#aws-resource-access-manager-ram) · [SCPs](#service-control-policies-scp) · [Budgets](#aws-budgets) · [Cost Explorer](#aws-cost-explorer)

---

# AWS Organizations

## What is it?

Centralized multi-account management — consolidated billing, OUs, and **Service Control Policies (SCPs)**.

## Architecture

```
Management Account → Root → OUs (Dev, Prod, Security) → Member Accounts
                              ↓ SCPs filter permissions
```

## Components

| Component | Notes |
|-----------|-------|
| **Management Account** | Root account that creates the organization |
| **Member Account** | Individual AWS account in the org |
| **OU** | Organizational Unit for grouping accounts |
| **SCP** | Service Control Policy guardrails |
| **Consolidated Billing** | Single bill for all member accounts |

## Related AWS Services

[Control Tower](#aws-control-tower) · [SCPs](#service-control-policies-scp) · [CloudTrail](../08-monitoring/README.md#aws-cloudtrail)

## Interview Questions

Management vs member account? SCP purpose?

## Exam Tips

SCP = guardrail (what accounts CAN'T do). Management account not affected by SCPs.

## One Line Definition

**Organizations = centralized multi-account AWS management.**

---

# AWS Control Tower

## What is it?

Automated multi-account **landing zone** with guardrails, Account Factory, and dashboard.

## Architecture

```
Control Tower → Landing Zone → OUs + Guardrails → Account Factory → New Accounts
```

## Components

| Component | Notes |
|-----------|-------|
| **Landing Zone** | Pre-configured multi-account setup |
| **Guardrail** | Policy/control for governance |
| **Account Factory** | Automated account provisioning |
| **Log Archive Account** | Centralized audit log storage |
| **Audit Account** | Security review and compliance account |

## Related AWS Services

[Organizations](#aws-organizations) · [Config](../08-monitoring/README.md#aws-config)

## Exam Tips

Control Tower builds on Organizations. Guardrails = SCPs + Config rules.

## One Line Definition

**Control Tower = automated multi-account landing zone setup.**

---

# AWS Resource Access Manager (RAM)

## What is it?

Share AWS resources (subnets, Transit Gateway, licenses) across accounts without duplication.

## Architecture

```
Owner Account → RAM Share → Consumer Accounts
     ↓
Shared Subnet / TGW / License
```

## Components

| Component | Notes |
|-----------|-------|
| **Resource Share** | Shared resource container |
| **Principal** | Account/OU allowed to consume share |
| **Shared Resource** | Subnet, TGW, license, etc. |
| **Permission** | Access level on shared resource |

## Related AWS Services

[Organizations](#aws-organizations) · [VPC](../04-networking/README.md#vpc) · [Transit Gateway](../04-networking/README.md#transit-gateway)

## Exam Tips

Shared VPC = central account owns VPC, others use subnets. Common enterprise pattern.

## One Line Definition

**RAM = share AWS resources across accounts.**

---

# Service Control Policies (SCP)

## What is it?

Organization-level guardrails filtering max permissions. Never grants — only restricts.

## Architecture

```
SCP on OU → Filters IAM for all accounts in OU
Effective Permission = IAM ∩ SCP
```

## Components

| Component | Notes |
|-----------|-------|
| **SCP Document** | JSON policy with Allow/Deny statements |
| **Attachment** | Applied to root, OU, or account |
| **Allow List Strategy** | Only explicitly allowed actions pass |
| **Deny List Strategy** | Block specific actions/services |

## Examples

Deny leaving org · Restrict regions · Prevent disabling CloudTrail · Block expensive services in sandbox

## Related AWS Services

[Organizations](#aws-organizations) · [IAM](../15-identity/README.md#iam)

## Exam Tips

SCP never grants permissions. Explicit Deny in SCP overrides IAM Allow. Max 5 SCPs per target.

## One Line Definition

**SCP = organization-level permission guardrails.**

---

# AWS Budgets

## What is it?

Custom cost/usage budgets with alerts at thresholds (50%, 80%, 100%, forecasted). Optional budget actions.

## Architecture

```
Costs → Budget Evaluation → Threshold Breached → SNS Alert → Optional Action
```

## Components

| Component | Notes |
|-----------|-------|
| **Budget** | Cost or usage spending limit |
| **Alert Threshold** | Notify at 50/80/100% or forecast |
| **SNS Notification** | Email/SMS alert destination |
| **Budget Action** | Auto-remediation when threshold hit |

## Related AWS Services

[Cost Explorer](#aws-cost-explorer) · [SNS](../10-messaging/README.md#amazon-sns)

## Exam Tips

First 2 budgets free. Budget actions can apply IAM policy or run SSM automation.

## One Line Definition

**Budgets = cost and usage alerts with optional actions.**

---

# AWS Cost Explorer

## What is it?

Visualize and analyze AWS costs — filter by service, account, tag, region. Forecast and RI/SP recommendations.

## Architecture

```
Billing Data → Cost Explorer → Filters/Reports → Optimization Recommendations
```

## Components

| Component | Notes |
|-----------|-------|
| **Reports** | Cost and usage visualizations |
| **Filters** | Slice by service, account, tag, region |
| **Forecast** | Predicted future spend |
| **RI/SP Coverage** | Reserved/Savings Plans utilization view |
| **Tags** | Cost allocation labels |

## Comparison

| Cost Explorer | CUR |
|---------------|-----|
| Visual analysis | Raw billing data in S3 |
| Daily updates | Detailed line items |

## Related AWS Services

[Budgets](#aws-budgets) · [Trusted Advisor](../08-monitoring/README.md#aws-trusted-advisor)

## Exam Tips

Tag resources for cost allocation. CUR = Cost and Usage Report in S3 for detailed analysis.

## One Line Definition

**Cost Explorer = visualize and analyze AWS spending.**
