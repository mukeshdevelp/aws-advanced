# 3. Database Services ⭐⭐⭐⭐⭐

Managed relational, NoSQL, cache, and analytics databases.

**Topics:** [RDS](#amazon-rds) · [Aurora](#amazon-aurora) · [DynamoDB](#amazon-dynamodb) · [ElastiCache](#amazon-elasticache) · [Redshift](#amazon-redshift)

---

# Amazon RDS

## What is it?

Managed SQL databases (MySQL, PostgreSQL, MariaDB, Oracle, SQL Server) with automated backups and patching.

## Architecture

```
App → Primary (AZ-a) ──sync──→ Standby (AZ-b)  [Multi-AZ]
         ↓ async
    Read Replica ← read traffic
```

## Components

| Component | Notes |
|-----------|-------|
| **DB Instance** | Managed database server in a VPC subnet |
| **Multi-AZ** | Sync standby in another AZ for HA failover |
| **Read Replica** | Async copy for scaling read traffic |
| **Parameter Group** | Engine configuration settings |
| **Subnet Group** | Subnets across AZs where DB can run |
| **Snapshot** | Manual backup; used to create new instances |
| **PITR** | Point-in-time recovery from automated backups |

## Advantages & Limitations

**+** Managed ops, backups, encryption  
**−** Vertical scaling limits · Replica lag (async)

## Comparison

| RDS | DynamoDB |
|-----|----------|
| SQL, schema | NoSQL, flexible |

## Related AWS Services

[VPC](../04-networking/README.md#vpc) · [DMS](../11-migration/README.md#aws-database-migration-service-dms) · [CloudWatch](../08-monitoring/README.md#amazon-cloudwatch)

## Interview Questions

Multi-AZ vs Read Replica?

## Exam Tips

Multi-AZ = sync standby (HA). Read Replica = async (scale reads). Failover ~60-120s.

## One Line Definition

**RDS = managed relational SQL databases.**

---

# Amazon Aurora

## What is it?

AWS-built MySQL/PostgreSQL-compatible DB with auto-scaling storage and up to 15 read replicas.

## Architecture

```
App → Writer + Readers → Shared Storage (6 copies, 3 AZs)
```

## Components

| Component | Notes |
|-----------|-------|
| **Cluster** | Shared storage volume for writer + readers |
| **Writer Instance** | Handles all write operations |
| **Reader Instance** | Read-only; up to 15 replicas |
| **Aurora Serverless** | Auto-scales capacity on demand |
| **Global Database** | Cross-Region read replicas for DR |

## Comparison

| Aurora | RDS |
|--------|-----|
| Auto-scaling storage | Fixed storage |
| <30s failover | ~60-120s failover |

## One Line Definition

**Aurora = high-performance AWS-built relational database.**

---

# Amazon DynamoDB

## What is it?

Serverless NoSQL with single-digit ms latency at any scale.

## Architecture

```
App → Table (Partition Key) → GSI → Streams → Lambda
```

## Components

| Component | Notes |
|-----------|-------|
| **Partition Key** | Required; determines data distribution |
| **Sort Key** | Optional; enables range queries on same partition |
| **GSI** | Alternate partition/sort key; own throughput |
| **LSI** | Same partition key; must be defined at creation |
| **On-Demand/Provisioned** | Pay per request vs set RCU/WCU capacity |
| **Streams** | Change log for Lambda/event processing |
| **DAX** | In-memory cache for microsecond reads |

## Advantages & Limitations

**+** Serverless, ms latency, unlimited scale  
**−** No joins · 400 KB item limit · Hot partition risk

## Related AWS Services

[Lambda](../01-compute/README.md#aws-lambda) · [Serverless](../14-serverless/README.md#amazon-dynamodb)

## Interview Questions

GSI vs LSI? On-Demand vs Provisioned?

## One Line Definition

**DynamoDB = serverless NoSQL with millisecond latency.**

---

# Amazon ElastiCache

## What is it?

In-memory Redis or Memcached caching to reduce DB load.

## Architecture

```
App → ElastiCache → (miss) → RDS/DynamoDB
```

## Components

| Component | Notes |
|-----------|-------|
| **Cluster** | Group of cache nodes |
| **Redis Node** | Primary or replica in replication group |
| **Replication Group** | Redis with Multi-AZ failover |
| **Shard** | Partition in Redis Cluster Mode |
| **Parameter Group** | Engine settings (maxmemory-policy, etc.) |

## Comparison

| Redis | Memcached |
|-------|-----------|
| Persistence, Multi-AZ | Simple, multi-threaded |

## Related AWS Services

[RDS](#amazon-rds) · [DynamoDB](#amazon-dynamodb)

## One Line Definition

**ElastiCache = in-memory Redis/Memcached caching layer.**

---

# Amazon Redshift

## What is it?

Petabyte-scale data warehouse for analytics (OLAP) with columnar storage.

## Architecture

```
S3 → Redshift Cluster → BI Tools
         ↓
    Spectrum (query S3)
```

## Components

| Component | Notes |
|-----------|-------|
| **Leader Node** | Parses queries, aggregates results |
| **Compute Node** | Executes query slices in parallel |
| **RA3 Node** | Managed storage separated from compute |
| **Spectrum** | Query data directly in S3 without loading |
| **WLM** | Workload Management query queues |

## Comparison

| Redshift | RDS |
|----------|-----|
| OLAP/analytics | OLTP/transactions |

## Exam Tips

Redshift = OLAP. Spectrum = query S3 without loading.

## One Line Definition

**Redshift = petabyte-scale analytics data warehouse.**
