# 2. Storage Services ⭐⭐⭐⭐⭐

Object, block, and file storage on AWS.

**Topics:**
1. [S3](#1-amazon-s3)
2. [EBS](#2-amazon-ebs)
3. [EFS](#3-amazon-efs)
4. [FSx](#4-amazon-fsx)
5. [Storage Gateway](#5-aws-storage-gateway)

---

# 1. Amazon S3

## What is it?

Infinitely scalable object storage with 11 9s durability. Store files as objects in buckets.

## Why do we use it?

- Static sites
- Backups
- Data lakes
- Lifecycle policies
- Versioning
- Cross-Region Replication

## Architecture

```
App → S3 Bucket → Objects → Storage Classes → Glacier
```

## Components

| Component | Notes |
|-----------|-------|
| **Bucket** | Global unique container for objects |
| **Object/Key** | File + its unique name within bucket |
| **Storage Class** | Standard, IA, Glacier, Intelligent-Tiering |
| **Versioning** | Keep multiple versions of an object |
| **Lifecycle** | Auto-transition or expire objects |
| **CRR** | Replicate objects to another Region |
| **Presigned URL** | Temporary authenticated download/upload URL |

## Advantages

- Unlimited, durable, event triggers

## Limitations

- Not a file system
- Object store only
## Comparison

| S3 | EBS |
|----|-----|
| Object, REST API | Block, attached to EC2 |

## Related AWS Services

- [CloudFront](../06-dns-cdn/README.md#2-amazon-cloudfront)
- [Lambda](../01-compute/README.md#1-aws-lambda)
- [IAM](../15-identity/README.md#1-iam)

## One Line Definition

**S3 = scalable object storage for any amount of data.**

---

# 2. Amazon EBS

## What is it?

Persistent block storage volumes attached to EC2 — like a virtual hard drive.

## Architecture

```
EC2 ←→ EBS Volume (same AZ) → Snapshot → S3 (incremental)
```

## Components

| Component | Notes |
|-----------|-------|
| **Volume** | Block storage attached to EC2 (same AZ) |
| **Snapshot** | Point-in-time backup stored incrementally in S3 |
| **gp3/io2** | SSD general purpose / high IOPS |
| **st1/sc1** | HDD throughput / cold storage |
| **Encryption** | At-rest encryption via KMS |

## Comparison

| EBS | EFS |
|-----|-----|
| Block, single EC2 | NFS, multi-EC2 |

## Related AWS Services

- [EC2](../01-compute/README.md#1-amazon-ec2)
- [KMS](../07-security/README.md#2-aws-kms)

## One Line Definition

**EBS = block storage volumes for EC2.**

---

# 3. Amazon EFS

## What is it?

Managed NFS file storage — multiple EC2/containers share files concurrently.

## Architecture

```
EC2-a ──┐
EC2-b ──┼──→ Mount Target (per AZ) → EFS File System
```

## Components

| Component | Notes |
|-----------|-------|
| **File System** | Regional NFS storage shared across AZs |
| **Mount Target** | ENI in subnet for NFS access per AZ |
| **Access Point** | Application-specific entry with own permissions |
| **Performance Mode** | General Purpose vs Max I/O |
| **EFS IA** | Infrequent access tier for cost savings |

## Related AWS Services

- [EC2](../01-compute/README.md#1-amazon-ec2)
- [ECS](../13-containers/README.md#3-amazon-ecs)
- [VPC](../04-networking/README.md#1-vpc)

## One Line Definition

**EFS = managed NFS file storage shared across compute.**

---

# 4. Amazon FSx

## What is it?

Managed file systems: Windows (SMB), Lustre (HPC), NetApp ONTAP, OpenZFS.

## Components

| Component | Notes |
|-----------|-------|
| **FSx for Windows** | SMB file shares with AD integration |
| **FSx for Lustre** | High-performance storage for HPC/ML |
| **FSx for ONTAP** | NetApp features (snapshots, replication) |
| **FSx for OpenZFS** | ZFS-based managed file system |
| **Backup** | Automatic daily backups of file systems |

## Comparison

| FSx Windows | EFS |
|-------------|-----|
| SMB | NFS |

## Related AWS Services

- [VPC](../04-networking/README.md#1-vpc)
- [Directory Service](../15-identity/README.md#4-aws-directory-service)

## One Line Definition

**FSx = managed Windows/Lustre/ONTAP/OpenZFS file systems.**

---

# 5. AWS Storage Gateway

## What is it?

Hybrid storage bridge connecting on-premises to AWS (S3, EBS snapshots, Glacier).

## Components

| Component | Notes |
|-----------|-------|
| **File Gateway** | NFS/SMB on-prem access backed by S3 |
| **Volume Gateway** | iSCSI block storage → EBS snapshots |
| **Tape Gateway** | Virtual tape library → S3 Glacier |
| **Cache Storage** | Local disk cache for low-latency access |
| **Bandwidth Throttling** | Control upload/download speed to AWS |

## Related AWS Services

- [S3](#1-amazon-s3)
- [Snowball](../11-migration/README.md#4-aws-snowball)

## One Line Definition

**Storage Gateway = hybrid bridge between on-premises and AWS storage.**
