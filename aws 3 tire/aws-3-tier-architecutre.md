# AWS 3-Tier Architecture Master Study Guide

Welcome to the **AWS 3-Tier Architecture Master Guide**. This note is organized into modular, bite-sized topics for easy learning and quick reference:

---

## Study Modules

### 1. [01 - Architecture Overview & Concepts](./01-architecture-overview.md)
- Core definition of Presentation, Application, and Data tiers.
- **2-Tier vs 3-Tier** visual comparison & key differences.
- Why 3-Tier wins (Scale, Fail, & Secure separately).
- Deep-dive Q&A (data tier isolation, coupling issues).

### 2. [02 - Services Breakdown by Tier](./02-tier-services.md)
- **Presentation Tier:** Route 53, WAF, CloudFront, S3.
- **Application Tier:** ALB, EKS, Fargate, Auto Scaling (HPA).
- **Data Tier:** Aurora PostgreSQL, ElastiCache Redis, S3, Secrets Manager.
- Service comparison Q&A (S3 Presentation vs Data tier, Redis vs Aurora).

### 3. [03 - EKS & Fargate Scaling Deep Dive](./03-eks-fargate-scaling.md)
- **EKS + EC2** vs **EKS + Fargate** comparison.
- Fargate on-demand infrastructure rules.
- Step-by-step **HPA scaling workflow** (1-7 lifecycle steps).
- Infrastructure abstraction & Golden Rule.

### 4. [04 - AWS Well-Architected Pillars (5 Pillars)](./04-well-architected-pillars.md)
- **Pillar 1: Security** (Defense in depth, IRSA, WAF, Secrets Manager, Encryption).
- **Pillar 2: Reliability** (ALB health checks, Circuit breakers, RDS backups, S3 versioning).
- **Pillar 3: Performance Efficiency** (Caching, Graviton, CDN, DB indexing, Connection pooling).
- **Pillar 4: Cost Optimization** (Spot instances, HPA, Tags).
- **Pillar 5: Operational Excellence** (IaC, CI/CD, Centralized logging, Runbooks).

### 5. [05 - DevOps Scenario Interview Q&A](./aws-3-tier-scenario-questions.md)
- **Q1:** High CPU troubleshooting (EKS pods at 95% CPU).
- **Q2:** High DB load vs low app CPU troubleshooting.
- **Q3:** Aurora Multi-AZ primary DB 3am failover.
- **Q4:** Leaked GitHub AWS credentials emergency response.
- **Q5:** Prove to an auditor DB cannot be reached from public internet.
- **Q6:** IRSA vs static IAM Access Keys.
- **Q7:** End-to-end "Buy Now" request flow trace.

---

## Quick Navigation

| Start Here | Services | Compute & Scaling | Best Practices | Scenario Q&A |
| :---: | :---: | :---: | :---: | :---: |
| [01 Overview](./01-architecture-overview.md) | [02 Services](./02-tier-services.md) | [03 EKS & Fargate](./03-eks-fargate-scaling.md) | [04 5 Pillars](./04-well-architected-pillars.md) | [05 Scenario Q&A](./aws-3-tier-scenario-questions.md) |