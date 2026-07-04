# 5. Load Balancing ⭐⭐⭐⭐⭐

Distribute traffic across servers for high availability and scalability.

**Topics:**
1. [Application Load Balancer](#1-application-load-balancer-alb)
2. [Network Load Balancer](#2-network-load-balancer-nlb)
3. [Gateway Load Balancer](#3-gateway-load-balancer-gwlb)
4. [Classic Load Balancer](#4-classic-load-balancer)

---

# 1. Application Load Balancer (ALB)

## What is it?

Layer 7 (HTTP/HTTPS) load balancer. Routes traffic based on URL path, host header, and query string. Best for web applications and microservices.

## Why do we use it?

- Path/host-based routing (/api → API, /web → Web)
- WebSocket and HTTP/2 support
- Integrates with ECS, EKS, Lambda targets, Cognito auth

## Architecture

```
Users → ALB (Public Subnet) → Target Group → EC2/ECS/Lambda (multi-AZ)
              ↓
        Listener Rules (path/host)
```

## Components

| Component | Notes |
|-----------|-------|
| **Listener** | Port/protocol accepting incoming traffic |
| **Target Group** | Group of EC2/ECS/Lambda/IP targets |
| **Health Check** | Probe to detect unhealthy targets |
| **Rules** | Path/host-based routing with priority |
| **Stickiness** | Session affinity via cookie |
| **ACM** | SSL/TLS certificate for HTTPS |

## Workflow

1. Create ALB
2. Create Target Group
3. Register Targets
4. Configure Listener Rules
5. Health Checks Pass
6. Traffic Flows

## Advantages

- Layer 7 routing, container-native
- Lambda targets, redirect/fixed-response actions, slow start

## Limitations

- Higher latency than NLB
- Not for non-HTTP protocols (use NLB)
- Cross-zone load balancing optional (cost)

## Common Use Cases

- Web apps, microservices
- API gateways, container services, blue/green deployments

## Comparison

| ALB | NLB |
|-----|-----|
| Layer 7 (HTTP) | Layer 4 (TCP/UDP) |
| Path/host routing | Ultra-low latency |
| Web apps | Gaming, IoT, TLS passthrough |

## Related AWS Services

- [EC2](../01-compute/README.md#1-amazon-ec2)
- [Auto Scaling](../01-compute/README.md#7-auto-scaling)
- [ACM](../07-security/README.md#7-aws-security-hub)
- [WAF](../07-security/README.md#4-aws-waf)

## Exam Tips

- ALB = Layer 7, path routing
- Target types: EC2, IP, Lambda, ALB
- Health check grace period for ASG

## One Line Definition

**ALB = Layer 7 load balancer for HTTP/HTTPS traffic.**

---

# 2. Network Load Balancer (NLB)

## What is it?

Layer 4 (TCP/UDP/TLS) load balancer. Handles millions of requests/sec with ultra-low latency (~100μs). Static IP per AZ.

## Why do we use it?

- Extreme performance for TCP/UDP traffic
- Static IP addresses (Elastic IP per AZ)
- Preserves source IP, TLS passthrough

## Architecture

```
Clients → NLB (Static IP per AZ) → Target Group → EC2/ECS/IP targets
              ↓
         TCP/UDP/TLS (passthrough)
```

## Components

| Component | Notes |
|-----------|-------|
| **Listener** | TCP/UDP/TLS port listener |
| **Target Group** | Backend EC2/ECS/IP targets |
| **Static IP (EIP)** | Fixed IP per AZ for clients |
| **Cross-Zone LB** | Distribute traffic across all AZs |
| **Preserve Client IP** | Pass original source IP to targets |

## Workflow

Create NLB → Assign Static IPs → Create Target Group → Register Targets → Clients Connect via Static IP

## Advantages

Millions RPS, static IPs, ultra-low latency, handles sudden spikes, ideal for non-HTTP.

## Limitations

No path-based routing. No HTTP-specific features. More expensive than ALB for simple web apps.

## Common Use Cases

Gaming servers, IoT, real-time streaming, VoIP, TLS passthrough, extreme traffic workloads.

## Comparison

| NLB | ALB |
|-----|-----|
| Layer 4 | Layer 7 |
| Static IP | Dynamic DNS |
| Ultra-low latency | Content routing |

## Related AWS Services

- [Global Accelerator](../04-networking/README.md#8-aws-global-accelerator)
- [EC2](../01-compute/README.md#1-amazon-ec2)
- [Route 53](../06-dns-cdn/README.md#1-amazon-route-53)

## Exam Tips

NLB = Layer 4, static IP, millions RPS. Use for non-HTTP or extreme performance needs.

## One Line Definition

**NLB = ultra-high performance Layer 4 load balancer.**

---

# 3. Gateway Load Balancer (GWLB)

## What is it?

Layer 3 Gateway load balancer for deploying third-party virtual appliances (firewalls, IDS/IPS, deep packet inspection).

## Why do we use it?

- Transparently insert security appliances in traffic flow
- Scale virtual appliances automatically
- GENEVE protocol on port 6081

## Architecture

```
Traffic → GWLB → Appliance Fleet (firewall/IDS) → Inspected Traffic → Destination
              ↓
        GWLB Endpoint (VPC)
```

## Components

| Component | Notes |
|-----------|-------|
| **GWLB** | Layer 3 load balancer for appliances |
| **GWLB Endpoint** | VPC endpoint to send traffic to GWLB |
| **Target Group** | Fleet of firewall/IDS appliance instances |
| **GENEVE** | Encapsulation protocol on port 6081 |
| **Route Table** | Routes traffic through GWLB endpoint |

## Workflow

Deploy Appliances → Create GWLB → Create Endpoints in Consumer VPCs → Route Traffic Through Appliances

## Advantages

Transparent inline inspection, auto-scales appliances, multi-VPC via endpoints, vendor ecosystem.

## Limitations

Niche use case. Requires compatible virtual appliances. Additional complexity and cost.

## Common Use Cases

Centralized firewall inspection, IDS/IPS, network traffic analysis, compliance requirements.

## Related AWS Services

- [VPC](../04-networking/README.md#1-vpc)
- [Transit Gateway](../04-networking/README.md#5-transit-gateway)
- [WAF](../07-security/README.md#4-aws-waf)

## Exam Tips

GWLB = Layer 3, GENEVE protocol, port 6081. For third-party network appliances. Good to know.

## One Line Definition

**GWLB = load balancer for inline network security appliances.**

---

# 4. Classic Load Balancer

## What is it?

Legacy Layer 4/7 load balancer (previous generation). AWS recommends ALB or NLB for new deployments.

## Why do we use it?

- Legacy applications already using Classic LB
- Basic Layer 4 and Layer 7 load balancing
- EC2-Classic support (deprecated)

## Architecture

```
Users → Classic LB → EC2 Instances (single AZ or multi-AZ)
```

## Components

| Component | Notes |
|-----------|-------|
| **Load Balancer** | Distributes traffic to registered instances |
| **Listener** | Front-end port and protocol |
| **Health Check** | Ping/HTTP check on instances |
| **Sticky Sessions** | Bind user to same instance via cookie |
| **SSL Termination** | Decrypt HTTPS at load balancer |

## Workflow

Create CLB → Register EC2 Instances → Configure Listener → Health Checks → Traffic Distributed

## Advantages

Simple setup, supports both Layer 4 and 7, familiar for legacy apps.

## Limitations

No path-based routing, no Lambda/container targets, no WAF integration, limited features vs ALB/NLB.

## Common Use Cases

Legacy app maintenance only. New deployments should use ALB or NLB.

## Comparison

| Classic LB | ALB |
|------------|-----|
| Legacy | Modern |
| Basic L4/L7 | Advanced L7 routing |
| EC2 only | EC2/IP/Lambda |

## Related AWS Services

- [ALB](#1-application-load-balancer-alb)
- [EC2](../01-compute/README.md#1-amazon-ec2)
- [Auto Scaling](../01-compute/README.md#7-auto-scaling)

## Exam Tips

Classic LB = legacy, avoid for new designs. Know it exists for exam context. Migrate to ALB/NLB.

## One Line Definition

**Classic LB = legacy load balancer; use ALB/NLB instead.**
