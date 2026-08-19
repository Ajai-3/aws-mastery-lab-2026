# AWS Well-Architected Pillars (3-Tier Best Practices)

[<- Previous: EKS & Fargate Scaling](./03-eks-fargate-scaling.md) | [Main Index](./aws-3-tier-architecutre.md) | [Next: Scenario Interview Q&A ->](./aws-3-tier-scenario-questions.md)

---

### Pillar 1: Security

- **Defense in Depth (Private Subnets)**
  - **What it does:** Keeps App and Database layers hidden in private subnets with no direct internet access.
  - **Why we need it:** Prevents hackers from connecting directly to your database or backend servers from the web.

- **IAM Least Privilege (IRSA - IAM Roles for Service Accounts)**
  - **What it does:** Gives each EKS pod *only* the specific AWS permissions it needs, without hardcoded credentials.
  - **Why we need it:** If one pod is compromised, the hacker cannot access other AWS services or steal master keys.

- **WAF Protection (Web Application Firewall)**
  - **What it does:** Acts as a digital security guard in front of CloudFront and ALB, filtering incoming web traffic.
  - **Why we need it:** Blocks malicious attacks (SQL injection, DDoS floods, bad bots) before they reach your app.

- **Secrets Manager**
  - **What it does:** Securely stores DB passwords and API keys, automatically rotating (changing) them regularly.
  - **Why we need it:** Keeps secrets out of code. If source code leaks on GitHub, no real passwords are exposed.

- **Encryption Everywhere (TLS & KMS)**
  - **What it does:** Encrypts data in transit (**TLS**) and data at rest (**KMS** keys for RDS, S3, EBS).
  - **Why we need it:** Protects sensitive data so nobody can read it, even if network traffic is intercepted or a disk is stolen.

---

### Pillar 2: Reliability

- **ALB Health Checks**
  - **What it does:** Continuously checks if app pods are alive and responding properly.
  - **Why we need it:** If a pod crashes or freezes, ALB instantly stops sending user traffic to it, preventing error pages.

- **Circuit Breakers (Retry with Exponential Backoff)**
  - **What it does:** Pauses and waits progressively longer before retrying failed requests instead of spamming.
  - **Why we need it:** Stops a minor temporary outage from cascading into a full system crash.

- **RDS Automated Backups**
  - **What it does:** Takes automatic daily snapshots and transaction logs of your database (retained for 7–35 days).
  - **Why we need it:** Allows point-in-time recovery to restore data to any exact second if data is corrupted or lost.

- **S3 Versioning**
  - **What it does:** Saves multiple historical versions of every file uploaded to S3.
  - **Why we need it:** If a file is accidentally deleted or overwritten, you can restore previous versions with one click.

---

### Pillar 3: Performance Efficiency

- **Caching at Every Layer (CloudFront & Redis)**
  - **What it does:** Stores copies of static files on CDN edges and frequent DB queries in super-fast Redis memory.
  - **Why we need it:** Delivers instant response times to users and reduces heavy read traffic on your main database.

- **Right-Sizing Compute (Graviton ARM Processors)**
  - **What it does:** Uses AWS's custom ARM-based Graviton CPUs for EKS compute nodes.
  - **Why we need it:** Delivers up to 40% better price-to-performance compared to traditional x86 processors.

- **Global CDN (CloudFront)**
  - **What it does:** Serves frontend files (React build, HTML, CSS) from edge locations close to users worldwide.
  - **Why we need it:** Ensures sub-50ms fast load times for global users, regardless of where your backend server lives.

- **Database Indexing (`EXPLAIN ANALYZE`)**
  - **What it does:** Creates fast search indexes on database tables and identifies slow-running queries.
  - **Why we need it:** Speeds up database queries from scanning millions of rows down to millisecond lookups.

- **Connection Pooling (PgBouncer / RDS Proxy)**
  - **What it does:** Reuses a shared pool of open database connections for incoming app requests.
  - **Why we need it:** Prevents thousands of concurrent users from overwhelming and crashing database connection limits.

---

### Pillar 4: Cost Optimization

- **Spot Instances**
  - **What it does:** Rents spare AWS compute capacity at up to 90% discount for EKS node groups.
  - **Why we need it:** Drastically cuts infrastructure costs for non-critical batch jobs, ETL, and background tasks.

- **Auto Scaling (HPA & Cluster Autoscaler)**
  - **What it does:** Automatically adds pods/nodes when traffic spikes and removes them when traffic slows down.
  - **Why we need it:** Ensures you only pay for extra servers during peak hours, saving money during quiet hours (e.g., 3 AM).

- **Cost Allocation Tags**
  - **What it does:** Labels every AWS resource with tags like `project`, `environment`, and `team`.
  - **Why we need it:** Gives clear monthly billing reports showing exactly which team or project spent money.

---

### Pillar 5: Operational Excellence

- **Infrastructure as Code (IaC - Terraform / AWS CDK)**
  - **What it does:** Defines your entire AWS infrastructure in code files instead of manual console clicks.
  - **Why we need it:** Makes deployments 100% automated, repeatable, error-free, and easy to recreate.

- **CI/CD Pipelines (GitHub Actions & Helm)**
  - **What it does:** Automatically builds Docker images, runs tests, and deploys updates to EKS on code push.
  - **Why we need it:** Ships new features quickly and safely without manual deployment mistakes.

- **Centralized Logging (Fluent Bit to CloudWatch / OpenSearch)**
  - **What it does:** Collects and aggregates logs from all EKS pods into one central dashboard.
  - **Why we need it:** Enables engineers to quickly search and troubleshoot errors across hundreds of containers in seconds.

- **Runbooks**
  - **What it does:** Written emergency response guides for common system failures (DB failovers, pod crashes).
  - **Why we need it:** Gives engineers step-by-step instructions to fix outages fast without panic or guessing.

---

[<- Previous: EKS & Fargate Scaling](./03-eks-fargate-scaling.md) | [Main Index](./aws-3-tier-architecutre.md) | [Next: Scenario Interview Q&A ->](./aws-3-tier-scenario-questions.md)
