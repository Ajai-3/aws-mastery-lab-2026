# AWS 3-Tier Services Breakdown

[<- Previous: Architecture Overview](./01-architecture-overview.md) | [Main Index](./aws-3-tier-architecutre.md) | [Next: EKS & Fargate Scaling ->](./03-eks-fargate-scaling.md)

---

## Presentation Tier Services

### Route 53
- **What:** AWS's DNS service
- **Job:** Converts `yourapp.com` into a server IP address
- **Use case:** When user types your domain, Route 53 tells the browser where to go
- **Extra:** Does health checks — if one region is down, routes traffic to a healthy one

### WAF (Web Application Firewall)
- **What:** A filter that sits in front of your app
- **Job:** Blocks malicious requests before they reach your servers
- **Use case:** Stops SQL injection, bot attacks, DDoS floods
- **Without it:** Raw internet traffic — good and bad — hits your app directly

### CloudFront
- **What:** AWS's CDN (Content Delivery Network)
- **Job:** Caches your website content at edge locations close to users worldwide
- **Use case:** User in Kochi and user in Tokyo both load your site fast, no long-distance server calls every time
- **Extra:** Also terminates SSL/HTTPS at the edge

### S3 (static hosting)
- **What:** Object storage, used here to host frontend files
- **Job:** Stores your React build files (HTML, CSS, JS) and serves them
- **Use case:** No server needed to serve a static frontend — cheap, scales automatically
- **Note:** S3 alone can't do server-side logic — that's the app tier's job

---

## Application Tier Services

### ALB (Application Load Balancer)
- **What:** Distributes incoming traffic across multiple backend servers
- **Job:** Prevents one server from getting overloaded while others sit idle
- **Use case:** 10,000 users hit your app at once — ALB spreads them across your EKS pods
- **Extra:** Does SSL termination and path-based routing (e.g., `/api/users` vs `/api/orders`)

### EKS (Elastic Kubernetes Service)
- **What:** Managed Kubernetes — runs and orchestrates your containers
- **Job:** Hosts your actual backend code (Node.js/FastAPI) that processes requests
- **Use case:** Your business logic — user authentication, order processing, validation — runs here
- **Extra:** Kubernetes restarts crashed containers automatically

### Fargate
- **What:** Serverless compute engine for EKS
- **Job:** Runs your containers without you managing the underlying EC2 servers
- **Use case:** You deploy a container, AWS handles the server provisioning behind it
- **Trade-off:** Less control, but less operational overhead

### Auto Scaling (HPA - Horizontal Pod Autoscaler)
- **What:** Kubernetes Horizontal Pod Autoscaler (HPA) — **not** an EC2 Auto Scaling Group
- **Job:** Automatically adjusts the number of running Pods in EKS based on real-time metrics (CPU/RAM utilization)
- **Use case:** Traffic spikes at 6pm — HPA scales up Pod replicas automatically; at 3am, it scales them down to save cost
- **Key Clarification:** In an EKS architecture (especially with Fargate), "Auto Scaling" refers to **HPA scaling Pods**, while Fargate supplies the infrastructure needed to run those Pods.

---

## Data Tier Services

### Aurora PostgreSQL
- **What:** AWS's managed relational database (PostgreSQL-compatible)
- **Job:** Stores your structured data — users, orders, transactions
- **Use case:** Any query needing consistency and relationships (e.g., *"get all orders for user X"*)
- **Multi-AZ:** Keeps a live replica in another data center — if primary fails, replica takes over with near-zero downtime

### ElastiCache Redis
- **What:** In-memory data store (cache)
- **Job:** Stores frequently accessed data so you don't hit the database every time
- **Use case:** User session data, or a product list that doesn't change often — read from cache in milliseconds instead of querying the DB
- **Why it matters:** Reduces DB load and speeds up response times

### S3 (documents/images)
- **What:** Same S3 service, different use here — object storage for files
- **Job:** Stores unstructured files — user uploads, PDFs, images, backups
- **Use case:** User uploads a profile picture — goes to S3, not the database
- **Why not DB:** Databases are bad at storing large binary files; S3 is built for it

### Secrets Manager
- **What:** Secure storage for credentials
- **Job:** Stores DB passwords, API keys — encrypted, with automatic rotation
- **Use case:** Your app fetches the DB password from Secrets Manager at runtime instead of having it hardcoded in code
- **Why it matters:** If code leaks (e.g., pushed to GitHub by mistake), no credentials are exposed

---

## Common Questions & Differences

### Q1: S3 appears in both presentation tier and data tier. What's the difference?
- **Presentation tier S3:** Stores the app's own code — HTML, CSS, JS build files. Same files served to every user. This is your frontend, not user data.
- **Data tier S3:** Stores user-generated files — uploaded photos, PDFs, documents, backups. Different per user, grows over time. Not code — just files/blobs.

> **Quick test to decide:** Does it fit in a database row (name, price, email)? -> **Aurora**. Is it a file (image, PDF)? -> **S3**.

---

### Q2: Redis and Aurora both store data. What decides which one you use?
- **Speed:** Redis = in-memory -> millisecond reads. Aurora = disk-based -> slower, but still fast.
- **Durability:** Redis = temporary, can lose data on restart. Aurora = permanent, durable, source of truth.
- **Change frequency:** Read often but rarely changes -> Redis. Must always be accurate right now -> Aurora.

> **One-line rule to memorize:** Redis = fast + temporary + "good enough." Aurora = slower + permanent + source of truth.

---

[<- Previous: Architecture Overview](./01-architecture-overview.md) | [Main Index](./aws-3-tier-architecutre.md) | [Next: EKS & Fargate Scaling ->](./03-eks-fargate-scaling.md)
