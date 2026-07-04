# 8. Monitoring & Logging ⭐⭐⭐⭐

Metrics, logs, audit trails, compliance, and tracing.

**Topics:**
1. [CloudWatch](#1-amazon-cloudwatch)
2. [CloudTrail](#2-aws-cloudtrail)
3. [Config](#3-aws-config)
4. [X-Ray](#4-aws-x-ray)
5. [VPC Flow Logs](#5-vpc-flow-logs)
6. [Trusted Advisor](#6-aws-trusted-advisor)
7. [Health Dashboard](#7-aws-health-dashboard)

---

# 1. Amazon CloudWatch

## What is it?

Monitoring service — **Metrics**, **Logs**, **Alarms**, **Dashboards**, and **Events/EventBridge** integration.

## Architecture

```
Resources → Metrics/Logs → CloudWatch → Alarms → SNS/Auto Scaling/Lambda
```

## Components

| Component | Notes |
|-----------|-------|
| **Metrics** | Time-series data points (CPU, custom, etc.) |
| **Alarms** | Trigger actions when threshold breached |
| **Dashboards** | Visual widgets for metrics and logs |
| **Log Groups/Streams** | Container and stream for log events |
| **Insights** | SQL-like query language for logs |
| **Synthetics** | Canaries for endpoint monitoring |

## Advantages

- Centralized monitoring, custom metrics, log analytics

## Limitations

- Log storage costs
- EC2 memory needs agent
- 1-min default granularity
## Comparison

| CloudWatch | CloudTrail |
|------------|------------|
| Performance metrics/logs | API audit trail |
| Real-time alarms | Who did what |

## Related AWS Services

- [Auto Scaling](../01-compute/README.md#7-auto-scaling)
- [SNS](../10-messaging/README.md#5-amazon-sns)
- [X-Ray](#4-aws-x-ray)

## Exam Tips

- EC2 default: CPU, network, disk — NOT memory (needs agent)
- Alarm states: OK, ALARM, INSUFFICIENT_DATA

## One Line Definition

**CloudWatch = metrics, logs, and alarms for AWS monitoring.**

---

# 2. AWS CloudTrail

## What is it?

Records all AWS API calls — who, what, when, where. Essential for audit and compliance.

## Architecture

```
API Call → CloudTrail → S3 Log File → Athena/CloudWatch analysis
              ↓
        Event History (90 days free)
```

## Components

| Component | Notes |
|-----------|-------|
| **Trail** | Configuration logging API events to S3 |
| **Event History** | 90-day searchable API activity (free) |
| **Organization Trail** | Single trail for all accounts in org |
| **Insights** | ML anomaly detection on API activity |
| **Data Events** | S3 object-level and Lambda invoke logs |

## Related AWS Services

- [S3](../02-storage/README.md#1-amazon-s3)
- [GuardDuty](../07-security/README.md#5-amazon-guardduty)

## Exam Tips

CloudTrail = WHO (API audit). CloudWatch = WHAT performance. Enable in all regions.

## One Line Definition

**CloudTrail = audit log of all AWS API activity.**

---

# 3. AWS Config

## What is it?

Track resource configuration changes over time. Evaluate **compliance rules** (e.g., no public S3 buckets).

## Architecture

```
Resources → Config Recorder → Snapshots → S3
                ↓
         Config Rules → Compliance → SNS Alert
```

## Components

| Component | Notes |
|-----------|-------|
| **Configuration Recorder** | Tracks resource config changes over time |
| **Config Rule** | Evaluates resources against compliance check |
| **Compliance Timeline** | History of rule pass/fail per resource |
| **Aggregator** | Collects config data from multiple accounts |

## Related AWS Services

- [Organizations](../12-governance/README.md#1-aws-organizations)
- [CloudTrail](#2-aws-cloudtrail)

## Exam Tips

Config = resource state over time. CloudTrail = API calls. Both needed for governance.

## One Line Definition

**Config = track and audit AWS resource configurations.**

---

# 4. AWS X-Ray

## What is it?

Distributed tracing for microservices — visualize request flow and latency across services.

## Architecture

```
API GW → Lambda → DynamoDB (trace segments) → X-Ray Service Map
```

## Components

| Component | Notes |
|-----------|-------|
| **Trace** | End-to-end record of a single request |
| **Segment** | Work done by a single service in a trace |
| **Subsegment** | Finer detail within a segment (DB call) |
| **Service Map** | Visual graph of service dependencies |
| **Sampling Rule** | Controls which requests get traced |

## Related AWS Services

- [Lambda](../01-compute/README.md#1-aws-lambda)
- [API Gateway](../14-serverless/README.md#2-api-gateway)

## Exam Tips

X-Ray = tracing. CloudWatch = metrics/logs. Enable via SDK or Lambda active tracing.

## One Line Definition

**X-Ray = distributed tracing for debugging microservices.**

---

# 5. VPC Flow Logs

## What is it?

Capture IP traffic metadata for VPC/subnet/ENI — accepted, rejected, or all traffic.

## Architecture

```
ENI Traffic → Flow Log → CloudWatch/S3 → GuardDuty/Athena
```

## Components

| Component | Notes |
|-----------|-------|
| **Flow Log** | Captured IP traffic metadata |
| **Traffic Type** | ACCEPT, REJECT, or ALL traffic |
| **Log Destination** | CloudWatch Logs or S3 bucket |
| **Aggregation Interval** | 1 min (default) or 10 min capture window |

## Related AWS Services

- [VPC](../04-networking/README.md#1-vpc)
- [GuardDuty](../07-security/README.md#5-amazon-guardduty)

## Exam Tips

Flow logs = network metadata. Not real-time packet capture. GuardDuty uses flow logs.

## One Line Definition

**VPC Flow Logs = log IP traffic in your VPC.**

---

# 6. AWS Trusted Advisor

## What is it?

Best-practice checks across **Cost**, **Performance**, **Security**, **Fault Tolerance**, and **Service Limits**.

## Components

| Component | Notes |
|-----------|-------|
| **Cost Checks** | Unused resources, RI recommendations |
| **Performance Checks** | Over/under-provisioned resources |
| **Security Checks** | Open ports, public S3 buckets |
| **Fault Tolerance** | Multi-AZ, backup configuration |
| **Service Limits** | Usage approaching account quotas |

## Related AWS Services

- [Cost Explorer](../12-governance/README.md#6-aws-cost-explorer)
- [Security Hub](../07-security/README.md#7-aws-security-hub)

## Exam Tips

Full checks need Business/Enterprise Support. 5 categories of recommendations.

## One Line Definition

**Trusted Advisor = AWS best practice recommendations.**

---

# 7. AWS Health Dashboard

## What is it?

**Service Health** — public AWS outage info. **Personal Health** — account-specific impact and affected resources.

## Architecture

```
AWS Event → Health Dashboard → EventBridge/SNS → Your Response
```

## Components

| Component | Notes |
|-----------|-------|
| **Service Health Dashboard** | Public AWS service outage status |
| **Personal Health Dashboard** | Account-specific events and impact |
| **Affected Entities** | Resources impacted by an AWS event |
| **EventBridge Integration** | Automate response to health events |

## Related AWS Services

- [CloudWatch](#1-amazon-cloudwatch)
- [EventBridge](../10-messaging/README.md#4-amazon-eventbridge)

## One Line Definition

**Health Dashboard = AWS service health and maintenance notifications.**
