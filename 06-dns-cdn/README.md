# 6. DNS & CDN ⭐⭐⭐⭐

Deliver applications globally with DNS routing and edge caching.

**Topics:** [Route 53](#amazon-route-53) · [CloudFront](#amazon-cloudfront)

---

# Amazon Route 53

## What is it?

Highly available and scalable Domain Name System (DNS) web service. Register domains, route traffic, and perform health checks.

## Why do we use it?

- DNS resolution with low latency globally
- Advanced routing policies (weighted, latency, failover, geolocation)
- Domain registration and DNSSEC support

## Architecture

```
User Query → Route 53 Resolver → Routing Policy → Target (ALB/CloudFront/S3/EC2)
                    ↓
              Health Checks → Failover
```

## Components

| Component | Notes |
|-----------|-------|
| **Hosted Zone** | Container for DNS records (public/private) |
| **Record Set** | A, AAAA, CNAME, MX, Alias records |
| **Routing Policy** | Simple, Weighted, Latency, Failover, Geo |
| **Health Check** | Monitor endpoint for failover routing |
| **Resolver** | DNS resolution for hybrid/on-prem |
| **DNSSEC** | Cryptographic signing of DNS responses |

## Workflow

Register/Transfer Domain → Create Hosted Zone → Add Records → Configure Routing Policy → DNS Resolves Globally

## Advantages

99.99% SLA, alias records (free queries to AWS resources), health-check failover, private DNS for VPC.

## Limitations

DNS propagation delay (TTL). Alias records only for AWS resources. Private hosted zones need VPC association.

## Common Use Cases

Domain management, multi-region failover, blue/green DNS cutover, hybrid DNS (on-prem + AWS).

## Comparison

| Route 53 | CloudFront |
|----------|------------|
| DNS routing | Content caching |
| Name resolution | Edge delivery |
| Routing policies | CDN performance |

## Related AWS Services

- [ALB](../05-load-balancing/README.md#application-load-balancer-alb) · [CloudFront](#amazon-cloudfront) · [S3](../02-storage/README.md#amazon-s3) · [Global Accelerator](../04-networking/README.md#aws-global-accelerator)

## Interview Questions

1. Routing policy types and use cases? 2. Alias vs CNAME? 3. Public vs private hosted zone?

## Exam Tips

Know all 6 routing policies: Simple, Weighted, Latency, Failover, Geolocation, Geoproximity. Alias = free, CNAME = charged.

## One Line Definition

**Route 53 = AWS DNS service for domain routing and health checks.**

---

# Amazon CloudFront

## What is it?

Content Delivery Network (CDN) with 450+ edge locations worldwide. Caches content close to users for low latency and high transfer speeds.

## Why do we use it?

- Reduce latency for global users
- Offload origin server traffic
- DDoS protection (AWS Shield Standard included)
- Signed URLs/cookies for private content

## Architecture

```
User → Edge Location (cache hit → serve) → Origin (S3/ALB/EC2/Custom)
              ↓ (cache miss)
         Fetch from Origin → Cache at Edge
```

## Components

| Component | Notes |
|-----------|-------|
| **Distribution** | CDN config linking origins to edge |
| **Origin** | S3, ALB, EC2, or custom HTTP source |
| **Edge Location** | PoP where content is cached globally |
| **Cache Behavior** | Path pattern, TTL, allowed methods |
| **OAC/OAI** | Restrict S3 origin to CloudFront only |
| **Signed URL/Cookie** | Private content access control |
| **Lambda@Edge** | Run code at edge on viewer/origin events |

## Workflow

Create Distribution → Configure Origin → Set Cache Behaviors → Update DNS (Route 53/CNAME) → Users Hit Edge

## Advantages

Global low latency, reduces origin load, HTTPS support, integrates with WAF, Lambda@Edge customization.

## Limitations

Cache invalidation costs and limits (1000 free/month). Not for dynamic content without tuning. Propagation takes minutes.

## Common Use Cases

Static website delivery, video streaming, software downloads, API acceleration, global web apps.

## Comparison

| CloudFront | Global Accelerator |
|------------|-------------------|
| Layer 7 CDN (cache) | Layer 4 routing |
| Static/dynamic content | TCP/UDP traffic |
| Edge caching | Anycast IP routing |

## Related AWS Services

- [S3](../02-storage/README.md#amazon-s3) · [Route 53](#amazon-route-53) · [WAF](../07-security/README.md#aws-waf) · [ACM](../07-security/README.md#aws-certificate-manager-acm) · [Lambda](../01-compute/README.md#aws-lambda)

## Interview Questions

1. CloudFront vs Global Accelerator? 2. OAI vs OAC? 3. How does cache invalidation work?

## Exam Tips

OAC replaces OAI (recommended). Origin types: S3, ALB, EC2, custom. TTL controls cache duration. Lambda@Edge = edge compute.

## One Line Definition

**CloudFront = global CDN that caches content at edge locations.**
