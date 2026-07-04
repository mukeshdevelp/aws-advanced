# 10. Messaging Services ⭐⭐⭐⭐

Event-driven architecture with queues, topics, and event buses.

**Topics:** [SQS](#amazon-sqs) · [SNS](#amazon-sns) · [EventBridge](#amazon-eventbridge) · [MSK](#amazon-msk)

---

# Amazon SQS

## What is it?

Managed message **queue** — decouple producers and consumers. Standard (high throughput) and FIFO (ordering) queues.

## Architecture

```
Producer → SQS Queue → Consumer (EC2/Lambda)
              ↓ (failures)
         Dead Letter Queue (DLQ)
```

## Components

| Component | Notes |
|-----------|-------|
| **Queue** | Standard (high throughput) or FIFO (ordered) |
| **Message** | Payload up to 256 KB |
| **Visibility Timeout** | Hide message after read for processing |
| **DLQ** | Queue for messages that fail repeatedly |
| **Long Polling** | Wait for messages to reduce empty polls |

## Workflow

Send Message → Consumer Polls → Process → Delete (or DLQ after max receives)

## Advantages & Limitations

**+** Fully managed, auto-scale, decoupling, DLQ  
**−** Pull model · 256 KB limit · Standard = best-effort ordering

## Comparison

| SQS | SNS |
|-----|-----|
| Queue (1 consumer) | Topic (fan-out) |
| Pull | Push |

## Related AWS Services

[Lambda](../01-compute/README.md#aws-lambda) · [SNS](#amazon-sns) · [EventBridge](#amazon-eventbridge)

## Interview Questions

Standard vs FIFO? Visibility timeout? DLQ purpose?

## Exam Tips

FIFO = .fifo suffix, exactly-once, ordering. Long polling reduces empty receives cost.

## One Line Definition

**SQS = managed message queue for decoupling applications.**

---

# Amazon SNS

## What is it?

Pub/sub **topic** — publish once, multiple subscribers receive (SQS, Lambda, email, SMS, HTTP).

## Architecture

```
Publisher → SNS Topic → SQS / Lambda / Email / HTTP
```

## Components

| Component | Notes |
|-----------|-------|
| **Topic** | Channel for pub/sub message broadcast |
| **Subscription** | Endpoint receiving copies (SQS, Lambda, email) |
| **Message Filter** | JSON policy to filter who receives message |
| **Delivery Policy** | Retry and backoff for failed deliveries |
| **FIFO Topic** | Ordered, exactly-once delivery topic |

## Workflow

Create Topic → Add Subscriptions → Publish → All Subscribers Receive Copy

## Advantages & Limitations

**+** Fan-out, multi-protocol, message filtering  
**−** No persistence · 256 KB limit

## Common Pattern

SNS → multiple SQS queues → Lambda (fan-out decoupling)

## Related AWS Services

[SQS](#amazon-sqs) · [CloudWatch](../08-monitoring/README.md#amazon-cloudwatch)

## Exam Tips

SNS = 1→many push. SQS = 1→1 pull. Classic pattern: SNS fans out to SQS queues.

## One Line Definition

**SNS = pub/sub notification service for fan-out messaging.**

---

# Amazon EventBridge

## What is it?

Serverless **event bus** with content-based filtering, cron scheduling, and SaaS integration.

## Architecture

```
Event Source → EventBridge Bus → Rules (filter) → Lambda/SQS/SNS/Step Functions
```

## Components

| Component | Notes |
|-----------|-------|
| **Event Bus** | Router for default/custom/SaaS events |
| **Rule** | Matches events and sends to targets |
| **Event Pattern** | JSON filter on event content |
| **Target** | Lambda, SQS, SNS, Step Functions, etc. |
| **Archive** | Store/replay historical events |
| **Scheduler** | Cron/rate-based event trigger |

## Comparison

| EventBridge | SNS |
|-------------|-----|
| Content filtering | Simple broadcast |
| Event routing | Pub/sub |

## Related AWS Services

[Lambda](../01-compute/README.md#aws-lambda) · [Step Functions](../14-serverless/README.md#aws-step-functions)

## Interview Questions

EventBridge vs CloudWatch Events? vs SNS?

## Exam Tips

EventBridge = successor to CloudWatch Events. Archive + replay. Content-based event patterns.

## One Line Definition

**EventBridge = serverless event bus for routing events.**

---

# Amazon MSK

## What is it?

Managed Apache **Kafka** for real-time streaming data pipelines at scale.

## Architecture

```
Producers → MSK Brokers → Topics/Partitions → Consumers
                ↓
         MSK Serverless (no cluster mgmt)
```

## Components

| Component | Notes |
|-----------|-------|
| **Cluster** | Managed Kafka broker group |
| **Broker** | Server handling producer/consumer traffic |
| **Topic** | Named stream of records |
| **Partition** | Ordered shard inside a topic |
| **Consumer Group** | Consumers sharing topic partitions |
| **MSK Connect** | Managed Kafka connectors |

## Comparison

| MSK (Kafka) | SQS |
|-------------|-----|
| High-throughput streaming | Simple queuing |
| Complex, powerful | Easy, managed |

## Related AWS Services

[Lambda](../01-compute/README.md#aws-lambda) · [S3](../02-storage/README.md#amazon-s3)

## Exam Tips

MSK = managed Kafka. Use for streaming, not simple queuing. MSK Serverless available.

## One Line Definition

**MSK = managed Apache Kafka for real-time streaming.**
