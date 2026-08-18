# Storage, Secrets & Configuration Management

Notes covering Amazon S3, Amazon EBS, Amazon EFS, AWS Secrets Manager, and AWS AppConfig.

---

## 1. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-s3.svg" width="40" height="40" valign="middle" /> Amazon S3 (Simple Storage Service)
- **Category**: Object / Blob Storage
- **Core Purpose**: General-purpose scalable storage for static web assets, media files, data lakes, and backups.

### Key Concepts
- **Versatile Blob Storage**: Stores unstructured data objects across custom buckets and storage classes.
- **Static Website Hosting**: Hosts static HTML, CSS, JS, and media files cheaply and reliably.
- **Event-Driven Integration**: Triggers automated events (e.g., triggering AWS Lambda execution upon file uploads).

---

## 2. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Storage/ElasticBlockStore.png" width="40" height="40" valign="middle" /> Amazon EBS (Elastic Block Store)
- **Category**: Block Storage
- **Core Purpose**: High-performance persistent block storage drive attached to a single compute instance.

### Key Concepts
- **Single-Instance Attachment**: Operates like an SSD/HDD dedicated to a single EC2 instance or container.
- **High Throughput & Consistency**: Suitable for transactional databases and workloads requiring strict read/write consistency.

---

## 3. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Storage/EFS.png" width="40" height="40" valign="middle" /> Amazon EFS (Elastic File System)
- **Category**: File Storage (NFS)
- **Core Purpose**: Scalable POSIX file system shared concurrently across multiple compute instances.

### Key Concepts
- **Multi-Instance Sharing**: Mountable simultaneously across dozens or hundreds of EC2 instances.
- **Shared Static Content**: Ideal for content management systems (CMS) and shared cluster storage.

---

## 4. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-secrets-manager.svg" width="40" height="40" valign="middle" /> AWS Secrets Manager
- **Category**: Security & Credential Storage
- **Core Purpose**: Encrypted storage and automated lifecycle management of application credentials.

### Key Concepts
- **Encrypted Secret Storage**: Vault for API keys (e.g., Stripe, Gmail) and database passwords.
- **IAM Authorization**: Controls raw secret visibility via fine-grained IAM policies.
- **Automated Rotation**: Automatically rotates database passwords without application downtime.

---

## 5. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ManagementGovernance/AppConfig.png" width="40" height="40" valign="middle" /> AWS AppConfig
- **Category**: Dynamic Configuration Management
- **Core Purpose**: Real-time feature flags and application configuration management.

### Key Concepts
- **Runtime Branching**: Toggles feature flags and configuration settings without redeploying backend code.
- **Continuous Polling Agent**: Local compute sidecars poll AppConfig for updates to reduce network overhead and latency.
