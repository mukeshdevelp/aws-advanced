# 14. Serverless ⭐⭐⭐⭐

Build applications without managing servers.

**Topics:** [Lambda](#aws-lambda) · [API Gateway](#api-gateway) · [DynamoDB](#amazon-dynamodb) · [EventBridge](#amazon-eventbridge) · [SNS & SQS](#amazon-sns) · [Step Functions](#aws-step-functions)

---

# AWS Lambda

## What is it?

Run code on events without servers. Auto-scales, pay per 100ms of execution.

## Architecture

```
Trigger → Lambda → Target Service → CloudWatch Logs
```

## Components

| Component | Notes |
|-----------|-------|
| **Function** | Code plus runtime configuration |
| **Trigger** | Event source invoking Lambda |
| **Execution Role** | IAM permissions for the function |
| **Environment Variables** | Config values for the function |
| **DLQ** | Stores failed async invocations |

## Advantages & Limitations

**+** No servers, ms scaling, 200+ triggers  
**−** 15-min timeout · Cold starts · 10 GB memory max

## Related AWS Services

[Compute Lambda](../01-compute/README.md#aws-lambda) · [API Gateway](#api-gateway) · [CloudWatch](../08-monitoring/README.md#amazon-cloudwatch)

## Exam Tips

Provisioned concurrency eliminates cold starts. Default concurrency 1000/account/region.

## One Line Definition

**Lambda = run code without managing servers.**

---

# API Gateway

## What is it?

Managed API front-end — REST, HTTP, and WebSocket APIs with auth, throttling, and caching.

## Architecture

```
Client → API Gateway → Lambda / HTTP Backend
              ↓
        Stage + Authorizer + Usage Plan
```

## Components

| Component | Notes |
|-----------|-------|
| **REST/HTTP/WebSocket API** | API type and protocol support |
| **Route** | Path/method mapping to backend |
| **Integration** | Backend connection (Lambda/HTTP) |
| **Stage** | Deployed environment (dev/prod) |
| **Authorizer** | Auth via Cognito/IAM/Lambda |
| **Usage Plan** | Throttling and API key controls

## Comparison

| HTTP API | REST API |
|----------|----------|
| Cheaper, simpler | WAF, caching, more features |

## Related AWS Services

[Lambda](#aws-lambda) · [Cognito](../15-identity/README.md#amazon-cognito) · [WAF](../07-security/README.md#aws-waf)

## Exam Tips

29-second integration timeout. HTTP API for simple use cases. REST API for advanced features.

## One Line Definition

**API Gateway = managed API front-end for backends.**

---

# Amazon DynamoDB

## What is it?

Serverless NoSQL with ms latency — ideal serverless backend paired with Lambda.

## Architecture

```
Lambda → DynamoDB Table → Streams → Lambda (CDC)
```

## Components

| Component | Notes |
|-----------|-------|
| **Table** | Primary NoSQL data container |
| **Partition Key** | Required key for item distribution |
| **Sort Key** | Optional range key within partition |
| **GSI** | Alternate query index |
| **Streams** | Change feed for event processing |

## Related AWS Services

[Database DynamoDB](../03-database/README.md#amazon-dynamodb) · [Lambda](#aws-lambda) · [DAX](../03-database/README.md#amazon-dynamodb)

## Exam Tips

Design partition keys to avoid hot partitions. On-Demand for unpredictable traffic.

## One Line Definition

**DynamoDB = serverless NoSQL with millisecond latency.**

---

# Amazon EventBridge

## What is it?

Event bus routing events between AWS services, SaaS apps, and custom applications.

## Components

| Component | Notes |
|-----------|-------|
| **Event Bus** | Central router for events |
| **Rule** | Matches events to targets |
| **Event Pattern** | JSON filter on event fields |
| **Target** | Destination service/resource |
| **Archive** | Store and replay past events |

## Related AWS Services

[Messaging EventBridge](../10-messaging/README.md#amazon-eventbridge) · [Lambda](#aws-lambda) · [Step Functions](#aws-step-functions)

## One Line Definition

**EventBridge = serverless event bus for routing events.**

---

# Amazon SNS

## What is it?

Pub/sub fan-out combined with **SQS** queues — the standard serverless async pattern.

## Architecture

```
API → SNS Topic → SQS Queue → Lambda (process)
               → Lambda (direct notify)
```

## Components

| Component | Notes |
|-----------|-------|
| **SNS Topic** | Fan-out pub/sub channel |
| **Subscription** | Endpoint receiving topic messages |
| **SQS Queue** | Buffer/decouple async processing |
| **Message Filter** | Route only matching events |
| **DLQ** | Capture failed queue messages |

## Related AWS Services

[Messaging SQS](../10-messaging/README.md#amazon-sqs) · [Messaging SNS](../10-messaging/README.md#amazon-sns)

## One Line Definition

**SNS = pub/sub fan-out; SQS = decoupled message queue.**

---

# AWS Step Functions

## What is it?

Visual workflow orchestration — coordinate Lambda, ECS, SNS, SQS with retry/catch/parallel states.

## Architecture

```
Start → Task → Choice → Parallel → Catch/Retry → End
              ↓ (error)
         Fallback State
```

## Components

| Component | Notes |
|-----------|-------|
| **State Machine** | Workflow definition container |
| **Task State** | Calls a service (Lambda, ECS) |
| **Choice State** | Branch based on conditions |
| **Parallel State** | Run branches concurrently |
| **Map State** | Iterate over input items |
| **ASL** | Amazon States Language (JSON) |

## Comparison

| Standard | Express |
|----------|---------|
| Up to 1 year, exactly-once | High volume, at-least-once |
| Long workflows | Short, high-throughput |

## Related AWS Services

[Lambda](#aws-lambda) · [EventBridge](#amazon-eventbridge)

## Exam Tips

ASL = Amazon States Language. Standard for long workflows. Express for high-volume event processing.

## One Line Definition

**Step Functions = orchestrate serverless workflows visually.**
