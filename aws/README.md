# AWS Fundamentals — IAM, VPC, EC2, ALB & CloudFront Master Guide

Welcome to the **AWS Fundamentals Master Study Guide**. This note covers the primary building blocks of AWS cloud infrastructure, how each component functions independently, how they interconnect into a resilient production architecture, and real-world DevOps scenario interview Q&As.

---

## Covered Topics

### 1. [01 - IAM (Identity and Access Management)](#1-iam--identity-and-access-management)
- Core structure (User, Group, Role, Policy) & User vs Role distinction.
- Policy evaluation logic (Explicit Deny, Allow, Default Deny).
- Key concepts: Least privilege, Instance Profiles, Trust Policies, Managed vs Inline policies.

### 2. [02 - VPC (Virtual Private Cloud)](#2-vpc--virtual-private-cloud)
- Core network architecture & subnet layout.
- What actually makes a subnet "public" (Internet Gateway routing).
- Security Group vs Network ACL (Stateful vs Stateless comparison table).

### 3. [03 - EC2 (Elastic Compute Cloud)](#3-ec2--elastic-compute-cloud)
- Instance components (AMI, Instance Type, EBS, ENI, IAM Profile, Key Pair).
- Instance lifecycle (Reboot vs Stop/Start vs Terminate data persistence).
- Instance family types (t3/t4g, m6i, c6i, r6i) & Placement Groups.

### 4. [04 - ALB (Application Load Balancer)](#4-alb--application-load-balancer)
- Listener & Target Group architecture.
- Health checks, Sticky sessions, SSL termination.
- Layer 7 routing capabilities (Path, Host, Method, Query string, Source IP).

### 5. [05 - CloudFront (CDN & Edge Network)](#5-cloudfront--cdn--edge-network)
- Distribution structure (Origins, Behaviors, Cache Policies, Settings).
- Edge locations, Cache Hits/Misses, TTL, Invalidation, OAC.
- Cache key mechanics & performance trade-offs.

### 6. [06 - How They All Work Together (Production Architecture)](#6-how-they-all-work-together-production-architecture)
- Standard multi-tier production architecture diagram.
- Step-by-step end-to-end request flow.
- IAM integration across services & Security Group chaining.
- CloudFront + ALB layered security & automatic scaling dynamics.

### 7. [07 - Key Exam & Interview Takeaways](#7-key-exam--interview-takeaways)
- Quick-reference Q&A table covering core certification and technical interview questions.

### 8. [08 - DevOps Scenario Interview Q&A](#8-devops-scenario-interview-qa)
- **Q1:** CloudFront bypass vulnerability & ALB security group fix.
- **Q2:** Orphaned EBS volumes cost leak upon EC2 termination.
- **Q3:** Cookie caching bug in CloudFront path routing.
- **Q4:** Auto Scaling failure due to hardcoded IP security group rules.
- **Q5:** Explicit Deny vs explicit Allow policy precedence in IAM.

### 9. [09 - Study & Revision Guide](#9-study--revision-guide)
- Actionable steps for self-testing, architectural diagramming, and scenario practice.

---

## Quick Navigation

| IAM & VPC | EC2 & ALB | CDN & Architecture | Takeaways & Scenario Q&A |
| :---: | :---: | :---: | :---: |
| [01 IAM](#1-iam--identity-and-access-management) \| [02 VPC](#2-vpc--virtual-private-cloud) | [03 EC2](#3-ec2--elastic-compute-cloud) \| [04 ALB](#4-alb--application-load-balancer) | [05 CloudFront](#5-cloudfront--cdn--edge-network) \| [06 Architecture](#6-how-they-all-work-together-production-architecture) | [07 Takeaways](#7-key-exam--interview-takeaways) \| [08 Scenario Q&A](#8-devops-scenario-interview-qa) |

---

## 1. IAM — Identity and Access Management

**What it actually is:** the permission system for your entire AWS account. Nothing happens in AWS without IAM checking "is this identity allowed to do this?" first — every single API call, no exceptions.

### Core structure

| Concept | What it is |
|---|---|
| User | A person or app with long-term credentials |
| Group | A bundle of users who share the same permissions |
| Role | A **temporary** identity — assumed by a service, EC2 instance, or external account |
| Policy | A JSON document saying what's allowed/denied |

**User vs Role — the distinction that matters most:** a User has permanent credentials you must manage and rotate yourself. A Role has no permanent credentials at all — something "assumes" it temporarily and gets short-lived access. This is why EC2 instances should use Roles, never Users with hardcoded keys.

### How policy evaluation actually works

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject"],
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

- **Deny by default.** Nothing is allowed unless a policy explicitly allows it.
- If ANY attached policy has an explicit `Deny`, that wins — no matter how many `Allow` statements exist elsewhere. Explicit deny always beats explicit allow.
- Read this JSON like: "Allow this identity to do GetObject and PutObject, but only on objects inside `my-bucket`."

### Key IAM concepts, explained simply

- **Least privilege** — give only the exact permissions needed, nothing extra. Not "S3 full access" when you only need to read one bucket.
- **Instance Profile** — the actual mechanism that lets an EC2 instance "wear" an IAM Role. Without this, an instance has zero AWS permissions by default.
- **Trust Policy** — a separate policy that controls WHO is allowed to assume a role in the first place (e.g. "only the EC2 service can assume this role"). This is different from the permissions policy — trust policy = who can wear the badge, permissions policy = what the badge lets you do.
- **Managed vs Inline policies** — Managed = reusable, attach to multiple identities, easier to audit. Inline = glued to one specific identity, harder to reuse or track — generally avoid unless there's a specific one-off reason.

**Mental model:** IAM is the security guard standing in front of every single AWS resource, checking ID before letting any request through.

---

## 2. VPC — Virtual Private Cloud

**What it actually is:** your own private, isolated network inside AWS — you control the IP ranges, how subnets are split, and how traffic is allowed to move.

### Structure

```
VPC (CIDR: 10.0.0.0/16)
├── Availability Zone A
│   ├── Public Subnet  (10.0.1.0/24)  ← has route to Internet Gateway
│   └── Private Subnet (10.0.2.0/24)  ← no direct internet access
└── Availability Zone B
    ├── Public Subnet  (10.0.3.0/24)
    └── Private Subnet (10.0.4.0/24)
```

| Component | Role |
|---|---|
| Internet Gateway (IGW) | Lets public subnets send/receive internet traffic |
| Route Table | Decides where traffic from a subnet goes |
| NAT Gateway | Lets private subnets reach the internet — outbound only |
| Security Group | Stateful firewall, instance-level, allow rules only |
| Network ACL | Stateless firewall, subnet-level, allow AND deny rules |

### What actually makes a subnet "public"

A subnet is public **only** because its route table has a route to the Internet Gateway (`0.0.0.0/0 → igw-xxxxx`). There's no toggle or checkbox that says "make this public" — it's purely determined by routing. You could turn a private subnet public just by editing the route table.

### Security Group vs NACL — memorize this table, it's asked constantly

| | Security Group | Network ACL |
|---|---|---|
| Level | Instance (ENI) | Subnet |
| Stateful? | Yes — return traffic auto-allowed | No — must explicitly allow both directions |
| Rules | Allow only | Allow AND deny |
| Evaluation | All rules checked | Rules checked in number order, first match wins |

**Why "stateful vs stateless" actually matters in practice:** if a Security Group allows inbound traffic on port 443, the response automatically goes back out — no extra rule needed. But a NACL doesn't remember anything — if you allow inbound 443, you must ALSO add an outbound rule allowing the response, or the client never gets a reply.

**Mental model:** VPC is your data center floor plan. Subnets are rooms. Route tables are hallways connecting rooms. Security Groups are door locks on each room. The Internet Gateway is the building's front entrance.

---

## 3. EC2 — Elastic Compute Cloud

**What it actually is:** virtual machines that run your actual application. Every EC2 instance lives inside one VPC subnet.

### Structure

```
EC2 Instance
├── AMI (OS image — what's installed)
├── Instance Type (hardware — CPU, RAM, network)
├── EBS Volume (persistent storage)
├── Network Interface (ENI — IP address, SG attachment)
├── IAM Instance Profile (role the instance assumes)
└── Key Pair (SSH access)
```

### Instance lifecycle — know exactly what survives each transition

```
Pending → Running → Stopping → Stopped → Terminated
```

| Action | What happens | What's lost |
|---|---|---|
| **Reboot** | Same physical host | Nothing — everything persists |
| **Stop/Start** | Moves to a **new** physical host | Any data on **instance store** (ephemeral) volumes — EBS survives because it's separate from the host |
| **Terminate** | Instance permanently deleted | Root EBS volume deleted by default; **additional attached volumes are NOT deleted by default** unless you explicitly set `DeleteOnTermination` for them |

**That last row is a common misunderstanding** — people assume terminating an instance wipes everything. Root volume, yes, by default. Any extra EBS volumes you attached separately, no — they survive unless you configured them to auto-delete. Worth double-checking in real deployments so you don't leave orphaned (and billed) volumes lying around, or worse, assume they're gone when they're not.

### Key concepts

| Concept | What it means |
|---|---|
| AMI | Amazon Machine Image — OS + software snapshot, the blueprint |
| Instance Type | Hardware profile — e.g. `t3.medium` = burstable, 2 vCPU, 4GB RAM |
| EBS | Block storage — persists independently of the instance |
| User Data | A shell script that runs once, on first boot — used for bootstrapping (installing packages, pulling code, etc.) |
| Placement Group | Controls physical rack placement — `cluster` for low latency, `spread` for high availability |

### Instance type families (simplified)

| Family | Use case | Example |
|---|---|---|
| t3 / t4g | Burstable general purpose | Web servers, dev environments |
| m6i | Balanced compute/memory | Application servers |
| c6i | Compute-optimized | CPU-heavy workloads |
| r6i | Memory-optimized | Databases, caches |

**Mental model:** EC2 instances are the workers. IAM tells them what they're allowed to touch. VPC decides who they can talk to. EBS gives them a hard drive.

---

## 4. ALB — Application Load Balancer

**What it actually is:** distributes incoming HTTP/HTTPS traffic across multiple backend targets, based on Layer 7 (application-level) rules — meaning it can read the actual HTTP request, not just the IP/port.

### Structure

```
ALB
├── Listeners (port 80 HTTP, port 443 HTTPS)
│   └── Rules (evaluated top-to-bottom)
│       ├── Condition: path /api/*   → forward to TargetGroup-API
│       ├── Condition: path /admin/* → forward to TargetGroup-Admin
│       └── Default → forward to TargetGroup-Web
└── Target Groups
    ├── Targets: EC2 instances, IPs, Lambda
    ├── Health Check: GET /health every 30s
    └── Algorithm: Round Robin / Least Outstanding Requests
```

### Key concepts

| Concept | What it does |
|---|---|
| Listener | The port + protocol the ALB is listening on |
| Rule | Condition-action pair — routes based on path, host header, query string, IP |
| Target Group | Pool of backends receiving traffic — manages health checks |
| Health Check | ALB probes each target repeatedly; pulls unhealthy ones out of rotation automatically |
| Sticky Sessions | Same client always routed to same backend, using a cookie |
| SSL Termination | ALB decrypts HTTPS — backend EC2 instances often only receive plain HTTP internally |

### What you can route on

- **Path** — `/api/*` vs `/static/*`
- **Host header** — `api.example.com` vs `app.example.com`
- **HTTP method** — GET, POST
- **Query string** — `?version=2`
- **Source IP** — specific CIDR ranges

**Mental model:** ALB is the traffic cop. It reads every request, checks the details, and sends it to the correct group of servers — so no single server has to handle everything.

---

## 5. CloudFront — CDN & Edge Network

**What it actually is:** AWS's Content Delivery Network. It caches content at 400+ edge locations globally, so users get responses from somewhere physically close to them instead of your actual origin server every time.

### Structure

```
CloudFront Distribution
├── Origins (where CloudFront fetches content from)
│   ├── S3 Bucket (static assets)
│   ├── ALB (dynamic API)
│   └── Custom HTTP endpoint
├── Behaviors (rules that map URL patterns to origins)
│   ├── /static/* → S3 origin, cache 1 year
│   ├── /api/*    → ALB origin, no cache (or short TTL)
│   └── /*        → ALB origin, cache 5 minutes
├── Cache Policy (what to cache, TTL, key components)
└── Distribution Settings
    ├── Custom domain + ACM SSL certificate
    ├── WAF attachment (optional)
    └── Geo-restriction (optional)
```

### Key concepts

| Concept | What it means |
|---|---|
| Edge Location | A data center where CloudFront caches content, close to users |
| Origin | Where CloudFront fetches from on a cache miss |
| Behavior | Maps a URL pattern to a specific origin + cache rule |
| TTL | How long a cached object is kept before re-checking the origin |
| Cache Hit/Miss | Hit = served instantly from edge. Miss = fetched from origin, then cached for next time |
| Invalidation | Manually force-expire a cached object before its TTL runs out |
| OAC (Origin Access Control) | Locks down S3 so ONLY CloudFront can read from it — not the public internet directly |

### Cache key — the detail people get wrong

By default, CloudFront's cache key is **just the URL path**. Adding headers, query strings, or cookies to the cache key makes caching more precise — but every addition you add **reduces cache efficiency**, because now more variations count as "different" objects, meaning more cache misses. Don't add cache-key components you don't actually need.

**Mental model:** CloudFront is a global proxy cache sitting in front of everything — serving cached responses from the nearest edge at high speed, and only bothering your real origin server when it truly has to.

---

## 6. How They All Work Together (Production Architecture)

### The standard production architecture

```
User (Browser)
    │
    ▼
CloudFront Distribution
    │  (edge cache — serves static assets instantly)
    │  (forwards dynamic requests to origin)
    │
    ▼
ALB (Application Load Balancer)
    │  (in public subnet — has public IP)
    │  (routes /api/* → API Target Group)
    │  (routes /*    → Web Target Group)
    │
    ├─────────────────┐
    ▼                 ▼
EC2 (Web)        EC2 (API)
(private subnet) (private subnet)
    │                 │
    └────────┬────────┘
             ▼
        RDS / ElastiCache
        (private subnet)
```

**Rule to memorize:** every EC2 instance lives in a **private subnet** — no public IPs, no direct internet exposure. The ALB is the ONLY thing with a public-facing entry point.

### Request flow, step by step

**Scenario: user visits `https://app.example.com/api/users`**

1. DNS resolves `app.example.com` → nearest CloudFront edge location
2. CloudFront receives the HTTPS request, checks cache for `/api/users` — MISS (APIs generally aren't cached), forwards to the ALB as origin
3. ALB receives it on port 443 — **SSL terminates here** (decrypts HTTPS into plain HTTP internally). Rule evaluation: path `/api/*` → routes to API Target Group. Health checks ensure only healthy EC2s get traffic
4. EC2 (API) receives plain HTTP on port 8080 — runs under its IAM Instance Profile, can call S3/RDS/SSM directly via that role, lives in a private subnet, reaches the internet (if needed) only via NAT Gateway
5. EC2 queries RDS — RDS's security group only allows inbound port 5432 **from the EC2 security group specifically**, nothing else
6. Response travels back the same path in reverse: EC2 → ALB → CloudFront → User

**Worth flagging — SSL termination happens in TWO places, not one:** CloudFront terminates the client's HTTPS connection at the edge. Then CloudFront makes its OWN separate connection to the ALB as origin — which can also be HTTPS (re-encrypted) depending on your "origin protocol policy" setting. So "SSL terminates at ALB" isn't the full picture — it terminates once at CloudFront (client-facing) and can terminate again at ALB (origin-facing). Don't say "SSL only terminates at ALB" in an interview — that's incomplete.

### IAM — the thread connecting everything

IAM isn't just "human logins" — it's how every service authenticates to every other service, with zero hardcoded credentials anywhere:

| Resource | What its IAM role/policy allows |
|---|---|
| EC2 instance | Instance Profile grants: S3 read, SSM GetParameter, CloudWatch logs |
| ALB | Reads the ACM certificate — auto-managed by AWS |
| CloudFront | OAC role allows `s3:GetObject` on your specific S3 bucket only |
| Developers | IAM users/roles scoped tightly to exactly what their job needs |

### VPC layout, security-group chaining

```
VPC 10.0.0.0/16
│
├── Public Subnets (10.0.1.0/24, 10.0.3.0/24)
│   ├── ALB  ← only resource that's public-facing
│   └── NAT Gateway  ← lets private instances reach internet
│
└── Private Subnets (10.0.2.0/24, 10.0.4.0/24)
    ├── EC2 instances
    └── RDS / ElastiCache
```

```
Security Group Rules:
  ALB-SG:    inbound 443 from 0.0.0.0/0
  EC2-SG:    inbound 8080 from ALB-SG only
  RDS-SG:    inbound 5432 from EC2-SG only
```

**Why chaining security groups (SG referencing SG) matters:** instead of hardcoding IP addresses in each rule, each layer only trusts the SECURITY GROUP of the layer in front of it. This means when Auto Scaling launches a brand new EC2 instance, it automatically inherits the correct access — no manual IP updates needed anywhere in the chain.

### CloudFront + ALB — layered security and performance

| Concern | Handled by |
|---|---|
| Global latency | CloudFront edge caching |
| DDoS protection | CloudFront + AWS Shield Standard (automatic, free, always on) |
| WAF (SQL injection, rate limiting) | CloudFront with WAF attached |
| SSL certificate | ACM cert on CloudFront (client-facing); ALB cert for origin |
| Load distribution | ALB spreading traffic across EC2s in multiple AZs |
| ALB not directly exposed | ALB's security group only allows traffic from CloudFront's managed IP prefix list — blocking anyone who tries to bypass CloudFront and hit the ALB directly |

### Scaling — how each piece participates automatically

1. **CloudFront** absorbs spikes for anything cached — zero load reaches your origin at all
2. **ALB** automatically spreads uncached requests across every healthy target
3. **EC2 Auto Scaling** (attached to the ALB target group) launches new instances once CPU or request count crosses a threshold — new instances get auto-registered with the ALB, no manual step
4. **IAM roles** apply instantly to new instances via the Instance Profile — no manual credential setup, ever
5. **VPC networking** scales invisibly — new instances just pull IPs from the subnet's CIDR pool

---

## 7. Key Exam & Interview Takeaways

| Question | Answer |
|---|---|
| Where does SSL terminate? | At CloudFront (client-facing) AND typically again at ALB (origin-facing) — not on EC2 |
| How does EC2 access S3 without hardcoded credentials? | IAM Instance Profile — the role attached to the instance |
| Why do EC2 instances live in private subnets? | Security — no direct internet exposure; ALB is the single controlled entry point |
| How does CloudFront reach a "private" ALB? | ALB has a public DNS name — CloudFront connects to it as a standard HTTPS origin |
| How do you restrict an S3 bucket to CloudFront only? | Origin Access Control (OAC) + a matching S3 bucket policy |
| What makes a subnet "public"? | A route table entry: `0.0.0.0/0 → Internet Gateway` — nothing else |
| How does ALB know an EC2 target is healthy? | Health checks on the Target Group — HTTP path, interval, and failure thresholds |
| Security Group vs NACL? | SG = stateful, instance-level. NACL = stateless, subnet-level |

---

## 8. DevOps Scenario Interview Q&A

### Q1: A developer bypasses CloudFront and hits the ALB's public DNS name directly — and it still works. Is this a problem?

**Answer**
- Yes — if the ALB's security group allows traffic from `0.0.0.0/0` instead of only CloudFront's managed prefix list, anyone can skip your CDN/WAF layer entirely and hit the origin directly
- This defeats DDoS protection, WAF filtering, and caching — all the protection you built at the CloudFront layer becomes optional, not enforced
- Fix: restrict the ALB security group's inbound rule to only CloudFront's managed IP prefix list, so direct access to the ALB is blocked

---

### Q2: You terminated an EC2 instance that had 2 extra EBS volumes attached (beyond the root volume). A week later, you notice you're still being billed for storage. Why?

**Answer**
- Only the ROOT EBS volume is deleted by default on termination
- Additional attached volumes are NOT automatically deleted unless `DeleteOnTermination` was explicitly set to true for them
- This is a common real cost leak — orphaned volumes sitting around, still billed, easy to miss unless you're checking the EBS console separately from EC2

---

### Q3: A request to `/api/users` comes into CloudFront. The API response depends on the user's auth token (a cookie), but CloudFront's default cache key is just the URL path. What happens, and how do you fix it?

**Answer**
- By default, CloudFront would cache the FIRST user's response and serve that same cached response to every other user hitting the same path — a serious bug, potentially a security issue (user A's data served to user B)
- Fix: add the relevant cookie/header to the cache key, so CloudFront treats requests with different auth tokens as different cacheable objects
- Trade-off to mention: this reduces cache efficiency since more variations now count as unique — for highly personalized API responses, it's often better to just not cache that route at all (short/zero TTL)

---

### Q4: Your RDS security group allows inbound from a specific EC2 instance's IP address, not a security group reference. Auto Scaling adds 3 more EC2 instances. What breaks?

**Answer**
- The 3 new instances can't reach RDS — the security group rule only trusts the OLD instance's specific IP, not the new ones
- This is exactly why security groups should reference OTHER security groups (like `EC2-SG`), not hardcoded IPs — any instance carrying that SG automatically inherits access, including future ones from Auto Scaling
- Fix: change the RDS security group rule to allow inbound from the EC2 security group itself, not individual IPs

---

### Q5: An IAM policy explicitly denies `s3:DeleteObject` for a user. A different policy attached to the same user explicitly allows `s3:DeleteObject`. Can the user delete the object?

**Answer**
- No — explicit Deny always wins over explicit Allow, regardless of how many Allow statements exist elsewhere
- IAM evaluates ALL attached policies together; if even one has an explicit deny for that action, it's blocked, full stop
- This is a common IAM trap — attaching a broad "Allow" managed policy doesn't override a more specific "Deny" sitting somewhere else, and people forget to check for that when debugging "why can't this user do X"

---

## 9. Study & Revision Guide

- Redraw the request flow (CloudFront → ALB → EC2 → RDS) from memory, labeling which subnet each piece sits in
- Say the SG vs NACL differences out loud without checking the table
- Answer all 5 scenario questions before your next study session — these are the type of "connect the dots" questions that separate someone who memorized definitions from someone who understands the system