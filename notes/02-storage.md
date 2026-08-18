# Storage

Notes covering Amazon S3, Amazon EBS, Amazon EFS, and AWS Secrets Manager.

---

## 1. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-s3.svg" width="40" height="40" valign="middle" /> Amazon S3 (Simple Storage Service)
- **Category**: Object / Blob Storage
- **Core Purpose**: General-purpose scalable storage for static web assets, media files, data lakes, and backups.
- **Why Use It**: To store unlimited files, images, backups, and static web assets cheaply with ultra-high durability.

### Key Concepts
- **Versatile Blob Storage**: Stores unstructured data objects across custom buckets and storage classes.
- **Static Website Hosting**: Hosts static HTML, CSS, JS, and media files cheaply and reliably.
- **Event-Driven Integration**: Triggers automated events (e.g., triggering AWS Lambda execution upon file uploads).

---

## 2. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Storage/ElasticBlockStore.png" width="40" height="40" valign="middle" /> Amazon EBS (Elastic Block Store)
- **Category**: Block Storage
- **Core Purpose**: High-performance persistent block storage drive attached to a single compute instance.
- **Why Use It**: When an EC2 instance needs a fast, persistent local disk drive (SSD/HDD) for databases or OS files.

### Key Concepts
- **Single-Instance Attachment**: Operates like an SSD/HDD dedicated to a single EC2 instance or container.
- **High Throughput & Consistency**: Suitable for transactional databases and workloads requiring strict read/write consistency.

---

## 3. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Storage/EFS.png" width="40" height="40" valign="middle" /> Amazon EFS (Elastic File System)
- **Category**: File Storage (NFS)
- **Core Purpose**: Scalable POSIX file system shared concurrently across multiple compute instances.
- **Why Use It**: When multiple EC2 Linux instances need to share and access the exact same file system simultaneously.

### Key Concepts
- **Multi-Instance Sharing**: Mountable simultaneously across dozens or hundreds of EC2 instances.
- **Shared Static Content**: Ideal for content management systems (CMS) and shared cluster storage.

---

## 4. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-secrets-manager.svg" width="40" height="40" valign="middle" /> AWS Secrets Manager
- **Category**: Security & Credential Storage
- **Core Purpose**: Encrypted storage and automated lifecycle management of application credentials.
- **Why Use It**: To securely store, access, and automatically rotate database passwords, API keys, and credentials.

### Key Concepts
- **Encrypted Secret Storage**: Vault for API keys (e.g., Stripe, Gmail) and database passwords.
- **IAM Authorization**: Controls raw secret visibility via fine-grained IAM policies.
- **Automated Rotation**: Automatically rotates database passwords without application downtime.
