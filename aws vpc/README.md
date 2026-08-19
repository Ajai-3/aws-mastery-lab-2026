# AWS VPC Deep Dive Master Guide — Physical & Logical Networking

Welcome to the **AWS VPC Deep Dive Master Guide**. This document explores the underlying architecture of Virtual Private Clouds, explaining physical-to-logical resource mappings, packet routing mechanics, hardware-level isolation, production multi-AZ subnet designs, and scenario-based interview questions.

---

## Covered Topics

### 1. [01 - Overview & Real-World Utility](#1-01---overview--real-world-utility)
- What a VPC actually is (Nitro card hardware enforcement).
- Real-world utility: debugging network isolation & cost/performance AZ optimization.

### 2. [02 - Physical vs Logical Mapping](#2-02---physical-vs-logical-mapping)
- Scope and physical anchors for VPC, Subnet, Route Table, Security Group, and NACL.
- CIDR limits, VPC creation bounds, and placement group interaction.

### 3. [03 - VPC State, Durability & Data Storage](#3-03---vpc-state-durability--data-storage)
- Configuration state durability in control plane DBs.
- Semi-ephemeral DHCP leases (tied to ENI lifetime).
- External VPC Flow Logs delivery & immediate packet drop behavior (no buffering).

### 4. [04 - Address Space & Subnet Division](#4-04---address-space--subnet-division)
- Carving subnets out of VPC CIDRs (`10.0.0.0/16`).
- AWS reserved 5 IPs per subnet rule (first 4 + last 1).

### 5. [05 - Why Subnets Cannot Span Availability Zones](#5-05---why-subnets-cannot-span-availability-zones)
- Physical facility independence, AZ-aware routing tables, failure domain isolation, and service dependencies.

### 6. [06 - Packet Flow & Traffic Routing](#6-06---packet-flow--traffic-routing)
- Intra-VPC packet path via SDN fabric.
- Internet Gateway (1:1 NAT) vs NAT Gateway (many-to-1 NAT, per-AZ HA requirement).

### 7. [07 - Multi-Tenancy & Hardware Isolation](#7-07---multi-tenancy--hardware-isolation)
- Coexisting identical CIDRs across customers via Nitro VPC tagging.
- MAC and IP spoofing prevention at the hardware layer.
- Security Groups (stateful, instance-level) vs NACLs (stateless, subnet-level).

### 8. [08 - Encryption & Network Security Boundaries](#8-08---encryption--network-security-boundaries)
- Isolation vs encryption distinction.
- In-transit TLS/MACsec requirements.

### 9. [09 - Production 3-AZ Subnet Design Pattern](#9-09---production-3-az-subnet-design-pattern)
- Public (IGW), Private (NAT GW), and Isolated (No IGW/NAT route) subnets layout.

### 10. [10 - CIDR Planning & Account Gotchas](#10-10---cidr-planning--account-gotchas)
- Permanent subnet CIDRs, AZ ID physical mapping (`use1-az2`) vs randomized account AZ names, cross-AZ traffic costs.

### 11. [11 - DevOps Scenario Interview Q&A](#11-11---devops-scenario-interview-qa)
- **Q1:** Overlapping CIDR multi-tenancy on shared physical hardware.
- **Q2:** Single NAT Gateway point of failure across multiple AZs.
- **Q3:** Subnet multi-AZ constraint explanation.
- **Q4:** Security Group allow vs missing NACL return path drop.
- **Q5:** VPC isolation vs actual data encryption.
- **Q6:** Account AZ name randomization vs physical AZ IDs.
- **Q7:** Retroactive Flow Log retrieval limitations.
- **Q8:** DHCP IP changes during stop/start vs reboot.

### 12. [12 - Study & Revision Guide](#12-12---study--revision-guide)
- Hands-on exercises and verification checks.

---

## Quick Navigation

| Fundamentals & State | Routing & Isolation | Design & Gotchas | Scenario Q&A & Guide |
| :---: | :---: | :---: | :---: |
| [01 Overview](#1-01---overview--real-world-utility) \| [02 Mapping](#2-02---physical-vs-logical-mapping) \| [03 State](#3-03---vpc-state-durability--data-storage) | [04 Address Space](#4-04---address-space--subnet-division) \| [05 Subnets & AZs](#5-05---why-subnets-cannot-span-availability-zones) \| [06 Packet Flow](#6-06---packet-flow--traffic-routing) | [07 Multi-Tenancy](#7-07---multi-tenancy--hardware-isolation) \| [08 Encryption](#8-08---encryption--network-security-boundaries) \| [09 Production Design](#9-09---production-3-az-subnet-design-pattern) | [10 CIDR Gotchas](#10-10---cidr-planning--account-gotchas) \| [11 Scenario Q&A](#11-11---devops-scenario-interview-qa) \| [12 Guide](#12-12---study--revision-guide) |

---

## 1. Overview & Real-World Utility

### What this actually is, in plain terms

A VPC is your **private network inside AWS**. Not a physical thing — a logical construct enforced by AWS's hardware (the Nitro card) on every single packet. This document explains what's actually happening underneath the console clicks: how traffic moves, how AWS keeps your network isolated from every other customer's, and why subnets are tied to a single AZ.

This is **deeper than what most devs know** — most people can draw a VPC diagram but can't explain WHY a subnet can't span two AZs, or HOW two customers can both use `10.0.0.0/16` on the same physical server without collision. That's exactly what makes this useful for interviews — it separates "I used AWS" from "I understand AWS."

### Real-world scenario — why this matters practically

You'll hit this knowledge in two situations:

1. **Debugging a networking issue** — "why can't instance A talk to instance B" — you need to know the actual enforcement chain (SG → NACL → route table → IGW/NAT) to debug it in order, not guess randomly
2. **Cost/performance design decisions** — cross-AZ traffic costs $0.01/GB each way. If you don't understand that subnets are AZ-bound, you'll design a "high availability" setup that's secretly expensive and slow because services keep talking cross-AZ unnecessarily

---

## 2. Physical vs Logical Mapping

| Concept | Scope | Physical anchor |
|---|---|---|
| VPC | Region | Spans all AZs in the Region |
| Subnet | Single AZ | Pinned to physical racks in that AZ |
| Route Table | VPC/Subnet | Enforced at the Nitro card |
| Security Group | ENI (per-instance) | Enforced at the hypervisor |
| NACL | Subnet boundary | Enforced at the SDN layer |

**Key facts to remember:**
- A VPC lives entirely within ONE Region — it does not span Regions. The CIDR block (`10.0.0.0/16`, etc.) is a logical address space — no physical router "owns" it at creation time
- VPC CIDR is just an address space on paper until subnets are carved out and instances are launched
- A subnet is created in ONE specific AZ and **cannot be moved or resized** after creation — this is why AZ failure affects specific subnets, but never the VPC itself
- Route tables aren't real routers — they're lookup tables enforced by the Nitro hypervisor on every packet leaving an ENI
- **CIDR block size limits:** a subnet CIDR must be between `/16` and `/28`. You can add secondary CIDRs to a VPC later, but the VPC's original (primary) CIDR can never be changed after creation
- **Placement groups** (cluster, spread, partition) are a separate EC2 construct — they influence physical rack placement of instances within an AZ. They operate *beneath* the VPC layer — VPC doesn't control this, EC2 does

---

## 3. VPC State, Durability & Data Storage

This is a distinction almost nobody explains clearly: what's durable config, what's temporary, and what leaves the VPC entirely.

| Type | Durability | Where it lives |
|---|---|---|
| Configuration state | Durable | AWS's internal control-plane database, replicated across AZs in the Region |
| DHCP leases | Semi-ephemeral | Tied to the ENI's lifetime — not a timer |
| VPC Flow Logs | Exported, not stored in VPC | Delivered to CloudWatch Logs, S3, or Kinesis Data Firehose |
| Dropped packets | Not stored anywhere | VPC does not buffer or queue packets — gone the moment they're dropped |

**Breaking each one down:**

- **Configuration state (durable):** your VPC CIDRs, subnets, route tables, security groups, and NACLs are all stored in AWS's internal control-plane database — replicated across AZs for the whole Region. This is literally what you're looking at in the AWS Console or getting back from the API — it's not "live" infrastructure state, it's config data.

- **DHCP leases (semi-ephemeral):** when an EC2 instance launches, AWS's internal DHCP server assigns it an IP from the subnet's range. Here's the detail people miss — that lease isn't tied to a countdown timer like traditional DHCP. It's tied to the **ENI's lifetime**. As long as the ENI exists, the IP stays assigned to it.

- **VPC Flow Logs (optional, delivered externally):** if you turn this on, AWS captures packet metadata — source/destination IP, port, protocol, and whether the packet was allowed or rejected. This gets buffered at the Nitro layer first, then delivered out to CloudWatch Logs, S3, or Kinesis Data Firehose. Important detail: **flow logs are NOT stored inside the VPC** — they're always exported somewhere else. If you didn't set up a destination, you don't have logs.

- **No packet buffering:** the VPC itself never queues or holds onto packets. If a Security Group or NACL rule drops a packet, that packet is simply gone — no retry, no buffer, no log unless Flow Logs were already turned on.

**Why this matters practically:** if you're troubleshooting "did this connection get blocked or did it fail some other way," and you didn't have Flow Logs enabled BEFORE the incident, that packet-level evidence is permanently gone. You can't retroactively enable logging and see historical drops. This is a real operational gotcha — enable Flow Logs proactively on anything security-sensitive, not after something goes wrong.

---

## 4. Address Space & Subnet Division

```
VPC: 10.0.0.0/16  →  65,536 IPs total
  │
  ├── Subnet A  10.0.1.0/24  →  256 IPs  (us-east-1a)
  ├── Subnet B  10.0.2.0/24  →  256 IPs  (us-east-1b)
  └── Subnet C  10.0.3.0/24  →  256 IPs  (us-east-1c)
```

- Subnets can't overlap each other
- AWS reserves 5 IPs per subnet automatically (first 4 + last 1) — a `/24` gives you 251 usable IPs, not 256. **This trips people up constantly** — don't assume you get the full range.

---

## 5. Why Subnets Cannot Span Availability Zones

Four real reasons:

1. **AZs are physically separate data centers** — independent power, cooling, network. There's no shared physical link that would let one subnet's IP range live in two buildings.
2. **Route tables are AZ-aware** — a private subnet's route table points to the NAT Gateway *in that same AZ*. Spanning two AZs would mean AWS has no clean way to decide which NAT Gateway handles which packet.
3. **Failure isolation** — if an AZ goes down, only subnets IN that AZ should be affected. A subnet spanning two AZs would break that clean failure boundary — ambiguous which instances survive.
4. **AZ-aware AWS services depend on this** — RDS Multi-AZ, EKS node groups, ALB target registration all check which AZ a subnet belongs to, to decide where to place resources.

**Memory hook:** subnet = one AZ, always. If you need multi-AZ coverage, you create multiple subnets — one per AZ — not one big subnet.

---

## 6. Packet Flow & Traffic Routing

**Two instances, same subnet:**
```
Instance A (ENI) → Nitro Card → SDN Fabric → Nitro Card → Instance B (ENI)
```
Never touches the public internet — stays inside AWS's internal network fabric.

**Instance going out to the internet:**
```
Instance → ENI → Route Table (0.0.0.0/0 → igw-xxx) → Internet Gateway → Internet
```

- **Internet Gateway (IGW)** — does 1:1 NAT between your instance's private IP and its public/Elastic IP. No bandwidth cap.
- **NAT Gateway** — for PRIVATE subnets. Does many-to-one NAT (many private instances share one public IP). NAT Gateways are AZ-scoped — for HA, you need one per AZ, not one shared across all AZs.

**Common design mistake:** using a single NAT Gateway for a "multi-AZ" setup. If that one NAT Gateway's AZ goes down, every private subnet in other AZs loses internet access too — because they were all routing through it. Real HA setup = one NAT Gateway per AZ.

---

## 7. Multi-Tenancy & Hardware Isolation

The real question: your VPC and another customer's VPC could both use `10.0.0.0/16` and even run on the **same physical server**. How does AWS make sure they never collide?

- **Every packet is tagged with a VPC identifier at the Nitro card.** Routing decisions use VPC context + destination IP together — not just the IP. This is why two completely different customers can use identical CIDR ranges with zero conflict.
- **MAC spoofing prevention** — Nitro enforces that packets from your ENI can only carry the MAC address assigned to that ENI. Anything else gets dropped in hardware, not software.
- **IP spoofing prevention** — source IP is checked against what's actually assigned to that ENI (the "Source/Destination Check" setting controls this — it's disabled for NAT instances specifically because they need to forward other IPs' traffic).
- **The Nitro card is the enforcement point, not the guest OS.** It runs on separate dedicated silicon from your actual workload's CPU. Even if your instance's OS gets fully compromised, the attacker still can't bypass VPC isolation — because the enforcement isn't happening in software you control.

| Boundary | Enforced by | Stateful? |
|---|---|---|
| Between VPCs | SDN routing context | No traffic without explicit peering/TGW |
| Between subnets | NACLs | Stateless — checks both directions |
| Between instances | Security Groups | Stateful — return traffic auto-allowed |
| API access | IAM | Controls who can change VPC config |

**Interview-critical distinction: NACL vs Security Group**
- Security Group = stateful, applies to an instance/ENI. If you allow inbound, the response is automatically allowed out — you don't need a separate outbound rule for replies.
- NACL = stateless, applies to a subnet. You must explicitly allow BOTH directions — inbound rule and a matching outbound rule, or the response traffic gets blocked.

---

## 8. Encryption & Network Security Boundaries

- Traffic between EC2 instances inside a VPC is **isolated, not encrypted**. Isolation ≠ encryption — two different things people confuse constantly.
- If you need actual encryption on the wire (compliance requirement, sensitive data), you need TLS at the application layer, or MACsec/VPN — the VPC boundary alone does not encrypt anything.
- Traffic to another AWS Region, or to on-premises, goes over the public internet or Direct Connect — same rule applies, use Site-to-Site VPN or MACsec on Direct Connect if you need encryption there.

---

## 9. Production 3-AZ Subnet Design Pattern

```
VPC: 10.0.0.0/16

Public subnets (route to IGW):
  10.0.1.0/24  →  us-east-1a   ALB, NAT GW
  10.0.2.0/24  →  us-east-1b   ALB, NAT GW
  10.0.3.0/24  →  us-east-1c   ALB, NAT GW

Private subnets (route to NAT GW in SAME AZ):
  10.0.11.0/24  →  us-east-1a   App servers, EKS nodes
  10.0.12.0/24  →  us-east-1b   App servers, EKS nodes
  10.0.13.0/24  →  us-east-1c   App servers, EKS nodes

DB subnets (isolated — no internet route at all):
  10.0.21.0/24  →  us-east-1a   RDS, Redis
  10.0.22.0/24  →  us-east-1b   RDS, Redis
  10.0.23.0/24  →  us-east-1c   RDS, Redis
```

**Why "isolated" is a third category, not just "private":** private subnets still route to the internet (outbound only, via NAT). Isolated subnets have NO `0.0.0.0/0` route at all — nothing gets out, period. This is what your data tier should actually be.

| Label | Route table has | Gets a public IP? |
|---|---|---|
| Public | `0.0.0.0/0 → igw-xxx` | Yes (if auto-assign enabled) |
| Private | `0.0.0.0/0 → nat-xxx` | No |
| Isolated | No `0.0.0.0/0` route at all | No |

**Key insight:** "public" and "private" aren't a setting you toggle — they're just labels based on what the route table points to. You could technically turn a private subnet public just by editing its route table. Nothing stops you except good sense.

---

## 10. CIDR Planning & Account Gotchas

- **Subnet CIDR is permanent.** Unlike the VPC (which allows adding secondary CIDRs), you cannot resize a subnet after creation. Undersize it early, and you're stuck creating new subnets later — plan for growth, especially with EKS (pods can consume secondary IPs fast).
- **AZ names aren't physically consistent across accounts.** `us-east-1a` in your account might be a completely different physical data center than `us-east-1a` in another account — AWS randomizes this mapping per account specifically to spread load evenly. If you're coordinating infrastructure across accounts, use **AZ IDs** (like `use1-az2`) — these ARE stable physical identifiers.
- **Cross-AZ traffic costs money** — $0.01/GB each direction. For chatty microservices talking constantly across AZs, this adds up fast. Keep AZ-local traffic on AZ-local subnets where possible.

---

## 11. DevOps Scenario Interview Q&A

### Q1: Two customers both use `10.0.0.0/16` for their VPCs, and their instances happen to land on the same physical AWS server. Why don't their networks collide?

**Answer**
- Every packet is tagged with a VPC identifier at the Nitro card — routing decisions use VPC context + destination IP, not IP alone
- The Nitro card enforces this in hardware, separate from the actual compute your instance uses — a compromised guest OS still can't bypass it
- So identical CIDR ranges can coexist safely because the "context" (which VPC this packet belongs to) is always attached

---

### Q2: Your team set up one NAT Gateway to save cost, shared across 3 AZs. One AZ goes down. What happens, and why?

**Answer**
- If the NAT Gateway itself is in the AZ that went down, ALL private subnets across all 3 AZs lose internet access — not just the one AZ that failed
- This defeats the purpose of a "multi-AZ" setup — you built redundancy at the app layer but left a single point of failure at the networking layer
- Fix: one NAT Gateway per AZ, with each AZ's private subnet route table pointing to its own local NAT Gateway

---

### Q3: A dev asks — "why can't I just create one big subnet and put instances from us-east-1a and us-east-1b in it?" What's your answer?

**Answer**
- Not possible — a subnet is pinned to exactly one AZ at creation and can never span two
- AZs are physically separate data centers with no shared L2 network — there's no physical way for one subnet's IP range to exist in two buildings at once
- If you need multi-AZ coverage, you create separate subnets — one per AZ — that's the only supported pattern

---

### Q4: You allowed inbound traffic on a Security Group, but the NACL on that subnet is blocking responses. What's actually happening?

**Answer**
- Security Groups are **stateful** — allow inbound, and the response traffic is automatically allowed out, no extra rule needed
- NACLs are **stateless** — you must explicitly allow BOTH the inbound rule AND a matching outbound rule, or return traffic gets silently dropped
- This is a very common real bug: someone opens the Security Group correctly but forgets the NACL only has an inbound allow rule, no outbound — traffic goes in, but the response never gets back out

---

### Q5: Your compliance team asks: "is traffic between our EC2 instances inside the VPC encrypted?" What's the honest answer?

**Answer**
- No — VPC isolation is NOT the same as encryption. Traffic between instances inside a VPC is isolated (nobody outside can see it) but not encrypted by default
- If compliance requires actual encryption on the wire, you need TLS at the application layer, or MACsec/VPN
- Don't confuse "isolated" with "encrypted" in front of an auditor — that's an easy way to fail a security review

---

### Q6: You're troubleshooting "instance can't reach instance" across two different accounts using the exact same AZ name (`us-east-1a`). Is that a meaningful comparison?

**Answer**
- No — AZ names are randomized per AWS account. `us-east-1a` in Account A might be a totally different physical location than `us-east-1a` in Account B
- If you're debugging cross-account networking (like VPC peering or Transit Gateway) and need to reference the actual physical location, use **AZ IDs** (e.g. `use1-az2`) — these are consistent across accounts, AZ names are not

---

### Q7: An instance was compromised and made suspicious outbound connections 3 weeks ago. You want to check what traffic it sent. You never enabled VPC Flow Logs. Can you retrieve this now?

**Answer**
- No — Flow Logs are not retroactive. The VPC itself never stores or buffers packet history; if Flow Logs weren't already enabled and pointed at a destination (CloudWatch Logs, S3, or Kinesis Firehose), that data was never captured
- Once a packet passes through (or gets dropped), it's gone — there's no VPC-level packet buffer to go back and inspect
- Fix going forward: enable Flow Logs proactively on anything security-sensitive, before an incident — not after

---

### Q8: An instance's IP address changes unexpectedly after a stop/start cycle, but stayed the same during a reboot. Why the difference?

**Answer**
- DHCP leases in AWS are tied to the **ENI's lifetime**, not a fixed timer
- A reboot doesn't destroy the ENI — same ENI, same lease, same IP
- A stop/start (for most instance types, unless using an Elastic IP) can release and reassign the ENI or its private IP allocation, resulting in a new IP
- This is a common gotcha for anyone assuming AWS IPs behave like a traditional DHCP lease with a countdown timer — it doesn't work that way here

---

## 12. Study & Revision Guide

- Trace the packet flow diagrams out loud (same-subnet, and internet egress) without looking
- Explain the NACL vs Security Group difference to someone with zero AWS background — if you can't simplify it, you don't fully understand it yet
- Answer all 8 scenario questions from memory before your next review session