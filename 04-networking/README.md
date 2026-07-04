# 4. Networking ⭐⭐⭐⭐⭐

Isolated networks, routing, security, and hybrid connectivity.

**Topics:** [VPC](#vpc) · [NAT Gateway](#nat-gateway) · [Security Groups](#security-groups) · [VPC Peering](#vpc-peering) · [Transit Gateway](#transit-gateway) · [VPC Endpoints](#vpc-endpoints) · [VPN & Direct Connect](#site-to-site-vpn) · [Global Accelerator](#aws-global-accelerator)

---

# VPC

## What is it?

Virtual Private Cloud — isolated network in a Region with **Subnets** (1 per AZ), **Route Tables**, **Internet Gateway**, and **CIDR Blocks**.

## Why do we use it?

Network isolation · Public/private tiers · Foundation for all AWS deployments

## Architecture

```
VPC (10.0.0.0/16)
  ├── Public Subnet  → IGW → Internet
  ├── Private Subnet → NAT GW → Internet (outbound)
  └── Route Tables · Elastic IP (static public IP)
```

## Components

| Component | Notes |
|-----------|-------|
| **VPC** | Isolated network with CIDR block in a Region |
| **Subnet** | Segment of VPC in one Availability Zone |
| **Route Table** | Rules directing traffic from subnet |
| **IGW** | Allows public subnet internet access |
| **CIDR** | IP address range (e.g., 10.0.0.0/16) |
| **Elastic IP** | Static public IPv4 for NAT or bastion |

## Workflow

Create VPC → Subnets (multi-AZ) → Attach IGW → Route Tables → Launch Resources

## Advantages & Limitations

**+** Full control, multi-tier design, hybrid connectivity  
**−** CIDR fixed at creation · 5 VPC/Region default

## Related AWS Services

[EC2](../01-compute/README.md#amazon-ec2) · [NAT Gateway](#nat-gateway) · [ALB](../05-load-balancing/README.md)

## Exam Tips

Public = route to IGW. Private = NAT for outbound. 1 subnet = 1 AZ.

## One Line Definition

**VPC = isolated virtual network in an AWS Region.**

---

# NAT Gateway

## What is it?

Managed NAT for private subnets — outbound internet without public IPs. Placed in public subnet with Elastic IP.

## Architecture

```
Private EC2 → NAT GW (public subnet) → IGW → Internet
```

## Components

| Component | Notes |
|-----------|-------|
| **NAT Gateway** | Managed NAT service in public subnet |
| **Elastic IP** | Static IP attached to NAT Gateway |
| **Route Table Entry** | 0.0.0.0/0 → NAT GW in private subnet |
| **Public Subnet** | Required placement for NAT Gateway |

## Comparison

| NAT Gateway | NAT Instance |
|-------------|--------------|
| Managed, HA | Self-managed EC2 |

## One Line Definition

**NAT Gateway = managed outbound internet for private subnets.**

---

# Security Groups

## What is it?

**Security Groups** — stateful instance firewall. **NACLs** — stateless subnet firewall with allow/deny.

## Architecture

```
Internet → NACL (subnet) → SG (instance) → EC2
```

## Components

| Component | Notes |
|-----------|-------|
| **Security Group** | Stateful firewall; allow rules only |
| **NACL** | Stateless subnet firewall; allow + deny |
| **Inbound/Outbound Rules** | Port, protocol, source/destination |
| **Default SG** | Auto-created; allows traffic from same SG |

## Comparison

| Security Group | NACL |
|----------------|------|
| Stateful, allow only | Stateless, allow/deny |
| Instance level | Subnet level |

## Related AWS Services

[VPC](#vpc) · [RDS](../03-database/README.md#amazon-rds) · [ALB](../05-load-balancing/README.md)

## Exam Tips

SG = stateful. NACL = numbered rules, processed in order.

## One Line Definition

**SG = stateful instance firewall; NACL = stateless subnet firewall.**

---

# VPC Peering

## What is it?

Private connection between two VPCs via AWS backbone (same/different accounts/regions).

## Architecture

```
VPC-A (10.0.0.0/16) ←── Peering ──→ VPC-B (10.1.0.0/16)
  Route Table (+peer CIDR)    Route Table (+peer CIDR)
```

## Components

| Component | Notes |
|-----------|-------|
| **Peering Connection** | Link between two VPCs |
| **Accepter/Requester** | Accounts initiating vs accepting peering |
| **Route Table Entry** | Add peer VPC CIDR on both sides |
| **DNS Resolution** | Optional cross-VPC hostname resolution |

## Advantages & Limitations

**+** Low latency, private, cross-account  
**−** NOT transitive · CIDR must not overlap

## Comparison

| VPC Peering | Transit Gateway |
|-------------|-----------------|
| Point-to-point | Hub-and-spoke |
| Non-transitive | Transitive |

## One Line Definition

**VPC Peering = private connection between two VPCs.**

---

# Transit Gateway

## What is it?

Central hub connecting VPCs, VPN, and Direct Connect with transitive routing.

## Architecture

```
VPC-A/B/C ──→ Transit Gateway ←── VPN / Direct Connect
```

## Components

| Component | Notes |
|-----------|-------|
| **Transit Gateway** | Central routing hub |
| **Attachment** | VPC, VPN, or DX connection to TGW |
| **Route Table** | Controls traffic between attachments |
| **Route Domain** | Isolated routing context within TGW |

## One Line Definition

**Transit Gateway = central hub connecting VPCs and on-premises.**

---

# VPC Endpoints

## What is it?

**Gateway Endpoint** — private S3/DynamoDB (free). **Interface Endpoint** — PrivateLink for other services. **PrivateLink** — expose your service privately.

## Architecture

```
Private EC2 → Gateway Endpoint → S3
Private EC2 → Interface Endpoint → KMS/SNS/etc.
Consumer VPC → PrivateLink → Provider Service
```

## Components

| Component | Notes |
|-----------|-------|
| **Gateway Endpoint** | Route table entry for S3/DynamoDB (free) |
| **Interface Endpoint** | ENI in subnet for AWS services (PrivateLink) |
| **Endpoint Policy** | Restrict which resources/principals can access |
| **PrivateLink Service** | Expose your app/service to other VPCs privately |

## Comparison

| Gateway Endpoint | Interface Endpoint |
|------------------|-------------------|
| S3, DynamoDB only | Most other services |
| Free | Per-hour + data |

## Related AWS Services

[S3](../02-storage/README.md#amazon-s3) · [KMS](../07-security/README.md#aws-kms)

## One Line Definition

**VPC Endpoints = private AWS access; PrivateLink = private service sharing.**

---

# Site-to-Site VPN

## What is it?

**VPN** — IPsec tunnel over internet. **Direct Connect** — dedicated private line via DX Location.

## Architecture

```
On-Prem ──VPN──→ Virtual Private Gateway → VPC
On-Prem ──DX───→ DX Location → DX Gateway → VPC
```

## Components

| Component | Notes |
|-----------|-------|
| **Virtual Private Gateway** | AWS-side VPN termination on VPC |
| **Customer Gateway** | On-premises VPN device representation |
| **VPN Connection** | IPsec tunnel (2 redundant tunnels) |
| **Direct Connect** | Dedicated private fiber to DX Location |
| **DX Gateway** | Connect DX to multiple VPCs via TGW/VGW |

## Comparison

| VPN | Direct Connect |
|-----|----------------|
| Internet, quick | Dedicated, weeks to provision |
| Encrypted default | Private line |

## Related AWS Services

[Transit Gateway](#transit-gateway) · [Storage Gateway](../02-storage/README.md#aws-storage-gateway)

## Exam Tips

VPN = 2 redundant tunnels. DX + VPN = backup pattern.

## One Line Definition

**VPN = encrypted tunnel; Direct Connect = dedicated private link.**

---

# AWS Global Accelerator

## What is it?

**Global Accelerator** — static anycast IPs, Layer 4 routing to optimal endpoint. **Cloud WAN** — managed global enterprise network.

## Architecture

```
Global Users → GA Anycast IP → Nearest Edge → ALB/EC2/EIP
Enterprise → Cloud WAN Core → Sites/VPCs/VPN/DX
```

## Components

| Component | Notes |
|-----------|-------|
| **Accelerator** | Global anycast IP and listener config |
| **Listener** | TCP/UDP port routing rules |
| **Endpoint Group** | Regional group of ALB/EC2/EIP targets |
| **Cloud WAN Core Network** | Global network with segments and attachments |
| **Segment** | Isolated routing domain in Cloud WAN |

## Comparison

| Global Accelerator | CloudFront |
|--------------------|------------|
| Layer 4 routing | Layer 7 CDN/cache |

## Related AWS Services

[Route 53](../06-dns-cdn/README.md#amazon-route-53) · [Shield](../07-security/README.md#aws-waf)

## Exam Tips

GA ≠ CDN. GA = TCP/UDP. CloudFront = HTTP caching.

## One Line Definition

**GA = global traffic optimization; Cloud WAN = managed enterprise WAN.**
