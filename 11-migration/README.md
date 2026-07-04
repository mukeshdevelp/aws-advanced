# 11. Migration Services ⭐⭐⭐

Move applications and databases to AWS — lift-and-shift, replication, and offline transfer.

**Topics:**
1. [Application Migration Service](#1-aws-application-migration-service-mgn)
2. [Database Migration Service](#2-aws-database-migration-service-dms)
3. [DataSync](#3-aws-datasync)
4. [Snowball](#4-aws-snowball)

---

# 1. AWS Application Migration Service (MGN)

## What is it?

Replaces CloudEndure Migration. Lift-and-shift service that replicates on-premises or cloud servers to AWS with minimal downtime.

## Why do we use it?

- Continuous block-level replication to AWS
- Cutover with minutes of downtime
- Supports physical, virtual, and cloud source servers

## Architecture

```
Source Server → Replication Agent → Staging Subnet (EC2) → Cutover → Production EC2
```

## Components

| Component | Notes |
|-----------|-------|
| **Replication Agent** | Software on source server for block replication |
| **Source Server** | On-prem or cloud server being migrated |
| **Launch Template** | EC2 config for cutover/test instances |
| **Cutover Instance** | Final EC2 launched during migration switch |
| **Staging Area** | Subnet holding replicated instance data |

## Workflow

1. Install Agent
2. Configure Replication
3. Continuous Sync
4. Test Launch
5. Cutover
6. Production on AWS

## Advantages

- Minimal downtime cutover, block-level replication, supports heterogeneous sources, automated conversion.

## Limitations

- Requires agent installation
- Staging area costs during replication
- Not for refactoring (lift-and-shift only)

## Common Use Cases

- Data center migration, disaster recovery setup, server consolidation, cloud-first migration projects.

## Comparison

| MGN | SMS (deprecated) |
|-----|-------------------|
| Block-level replication | Agent-based |
| Minimal downtime | Replaced by MGN |

## Related AWS Services

- [EC2](../01-compute/README.md#1-amazon-ec2)
- [VPC](../04-networking/README.md#1-vpc)
- [DMS](#2-aws-database-migration-service-dms)

## Exam Tips

- MGN = lift-and-shift with continuous replication
- Cutover = final switch with minimal downtime
- Good to know

## One Line Definition

**MGN = lift-and-shift server migration with continuous replication.**

---

# 2. AWS Database Migration Service (DMS)

## What is it?

Migrate databases to AWS with minimal downtime. Supports homogeneous (same engine) and heterogeneous (different engines) migrations.

## Why do we use it?

- Continuous replication (CDC) during migration
- Migrate Oracle → PostgreSQL, SQL Server → Aurora, etc.
- On-going replication for DR

## Architecture

```
Source DB → DMS Replication Instance → Target DB (RDS/Aurora/on-prem)
                ↓
         Schema Conversion Tool (SCT) for heterogeneous
```

## Components

| Component | Notes |
|-----------|-------|
| **Replication Instance** | EC2 instance running migration tasks |
| **Source Endpoint** | Connection details for source DB |
| **Target Endpoint** | Connection details for target DB |
| **Migration Task** | Full load and/or CDC replication job |
| **SCT** | Converts schema for heterogeneous engines |

## Workflow

Create Endpoints → Create Replication Instance → Run Migration Task (full load + CDC) → Validate → Cutover

## Advantages

Minimal downtime with CDC, heterogeneous migration, ongoing replication, supports 20+ database engines.

## Limitations

Replication instance cost. Large LOB data can be slow. Schema conversion may need manual fixes (SCT).

## Common Use Cases

Oracle to Aurora migration, on-prem to RDS, database consolidation, cross-engine migrations.

## Related AWS Services

- [RDS](../03-database/README.md#1-amazon-rds)
- [Aurora](../03-database/README.md#2-amazon-aurora)
- [S3](../02-storage/README.md#1-amazon-s3)

## Exam Tips

CDC = Change Data Capture for ongoing replication. SCT = schema conversion for heterogeneous. DMS instance size matters.

## One Line Definition

**DMS = migrate databases to AWS with minimal downtime.**

---

# 3. AWS DataSync

## What is it?

Online data transfer service that automates moving data between on-premises storage and AWS (S3, EFS, FSx).

## Why do we use it?

- Fast transfer over internet or Direct Connect
- Automated scheduling and incremental transfers
- Data validation with checksums

## Architecture

```
On-Prem NFS/SMB → DataSync Agent → DataSync Service → S3 / EFS / FSx
```

## Components

| Component | Notes |
|-----------|-------|
| **DataSync Agent** | VM connecting on-prem storage to AWS |
| **Task** | Transfer job between two locations |
| **Location** | Source or destination path/ARN |
| **Bandwidth Throttle** | Limits network usage |
| **Verification** | Checksum validation after transfer |

## Workflow

Deploy Agent → Create Locations → Create Task → Start Transfer → Verify Checksums → Schedule Recurring

## Advantages

10x faster than open-source tools, automated scheduling, incremental transfers, in-transit encryption.

## Limitations

Requires agent for on-prem. Bandwidth-dependent speed. Not for continuous replication (use DMS/MGN).

## Common Use Cases

One-time bulk data migration, recurring sync to cloud, archive migration, hybrid storage workflows.

## Comparison

| DataSync | Storage Gateway |
|----------|-----------------|
| One-time/recurring transfer | Continuous hybrid access |
| Move data to cloud | Cloud-backed local access |

## Related AWS Services

- [S3](../02-storage/README.md#1-amazon-s3)
- [EFS](../02-storage/README.md#3-amazon-efs)
- [Direct Connect](../04-networking/README.md#7-site-to-site-vpn)

## Exam Tips

DataSync = online transfer. Snowball = offline (large datasets, slow connection). Storage Gateway = hybrid ongoing.

## One Line Definition

**DataSync = automated online data transfer to AWS.**

---

# 4. AWS Snowball

## What is it?

Physical device for offline data transfer. Snowball Edge for large datasets; Snowmobile for exabyte-scale migration.

## Why do we use it?

- Transfer PB-scale data when network is too slow/expensive
- Secure physical device shipped to/from AWS
- Snowball Edge includes compute for edge processing

## Architecture

```
On-Prem Data → Copy to Snowball Device → Ship to AWS → Import to S3
Snowball Edge → Local Compute + Storage at Edge → Ship back
```

## Components

| Component | Notes |
|-----------|-------|
| **Snowball Edge** | Physical transfer and edge compute device |
| **Storage Optimized** | High-capacity data migration device |
| **Compute Optimized** | Edge compute-heavy device option |
| **Snowmobile** | Exabyte-scale transfer truck |
| **Import/Export Job** | Device order and S3 import workflow |

## Workflow

Request Device → AWS Ships → Copy Data → Return Device → AWS Imports to S3 → Job Complete

## Advantages

Fast for massive datasets, encrypted, tamper-evident, no network dependency, edge compute option.

## Limitations

Physical logistics (shipping time). Device capacity limits (80 TB Snowball Edge). Temporary device availability.

## Common Use Cases

Data center decommission, large media archives, genomics data, disaster recovery seeding, edge computing.

## Comparison

| Snowball | DataSync |
|----------|----------|
| Offline (physical) | Online (network) |
| PB-scale | TB-scale recurring |
| Slow/no network | Good network |

## Related AWS Services

- [S3](../02-storage/README.md#1-amazon-s3)
- [Direct Connect](../04-networking/README.md#7-site-to-site-vpn)
- [Storage Gateway](../02-storage/README.md#5-aws-storage-gateway)

## Exam Tips

Snowball = offline, TB-PB. Snowmobile = exabytes. DataSync = online. Choose based on data size and network speed.

## One Line Definition

**Snowball = physical device for offline large-scale data transfer.**
