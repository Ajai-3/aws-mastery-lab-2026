# API Entry Points, Security, IAM & Key Management

Notes covering Elastic Load Balancing (ELB/ALB), Auto Scaling Groups, Amazon API Gateway, AWS WAF, AWS Shield, Amazon Cognito, AWS IAM, and AWS KMS.

---

## 1. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-elb.svg" width="40" height="40" valign="middle" /> Elastic Load Balancer (ELB / ALB)
- **Category**: Traffic Distribution & Load Balancing
- **Core Purpose**: Horizontally scales applications by distributing incoming traffic across multiple backend compute instances.

### Key Concepts
- **Horizontal Scaling**: Routes requests to backend compute nodes (EC2 instances or containers).
- **Auto Scaling Group Integration**: Dynamically adds or removes instances based on traffic load.
- **Sticky Sessions**: Binds user sessions to specific compute instances (ideal for WebSockets and sessionful connections).

---

## 2. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Compute/EC2AutoScaling.png" width="40" height="40" valign="middle" /> Auto Scaling Groups (ASG)
- **Category**: Compute Capacity Management
- **Core Purpose**: Automatically scales EC2/compute fleets based on demand rules and health checks.

### Key Concepts
- **Dynamic Fleet Sizing**: Adds compute capacity during peak loads and scales down during low-traffic periods.
- **Self-Healing Infrastructure**: Replaces unhealthy compute instances automatically.

---

## 3. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-api-gateway.svg" width="40" height="40" valign="middle" /> Amazon API Gateway
- **Category**: Managed API Gateway ("Front door of AWS")
- **Core Purpose**: Managed entry point for RESTful and WebSocket APIs.

### Key Concepts
- **Direct Service Integration**: Routes HTTP requests directly to AWS services (Lambda, DynamoDB, SQS) without needing an EC2 compute layer.
- **Traffic Control**: Supports rate limiting, request throttling, and custom access key management.
- **WebSocket Support**: Handles stateful, real-time two-way communications.

---

## 4. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-waf.svg" width="40" height="40" valign="middle" /> AWS WAF (Web Application Firewall)
- **Category**: Web Security & Threat Mitigation
- **Core Purpose**: Protects web applications and APIs from common web exploits.

### Key Concepts
- **Exploit Protection**: Guards against SQL injection (SQLi), Cross-Site Scripting (XSS), and bot scraping.
- **IP Rate Limiting & Banning**: Blocks malicious IP addresses and custom HTTP request patterns.

---

## 5. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-shield.svg" width="40" height="40" valign="middle" /> AWS Shield (Standard & Advanced)
- **Category**: DDoS Protection
- **Core Purpose**: Protects infrastructure against Distributed Denial of Service (DDoS) attacks.

### Key Concepts
- **Shield Standard**: Free protection included automatically for all AWS customers.
- **Shield Advanced**: Paid protection tier ($3,000/mo) for mission-critical apps; provides 24/7 AWS DDoS Response Team (DRT) access and financial refund guarantees for attack-induced traffic spikes.

---

## 6. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cognito.svg" width="40" height="40" valign="middle" /> Amazon Cognito
- **Category**: Identity, Authentication & Authorization
- **Core Purpose**: User registration, authentication, and authorization.

### Key Concepts
- **Cognito User Pools (CUP)**: User directory for sign-up, sign-in, multi-factor authentication (MFA), and JWT token issuance.
- **Cognito Identity Pools**: Authorizes authenticated users by granting temporary AWS IAM credentials based on custom user attributes (e.g., Student vs. Admin permissions).

---

## 7. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-iam.svg" width="40" height="40" valign="middle" /> AWS IAM (Identity and Access Management)
- **Category**: Identity & Access Control
- **Core Purpose**: Controls authenticating users/services and authorizing access permissions across all AWS resources.

### Key Concepts
- **Users, Groups & Roles**: Identity primitives for assigning policies to developers, applications, and AWS compute resources.
- **Least Privilege Principle**: Fine-grained JSON policies that enforce explicit allow/deny permissions for specific actions and resources.

---

## 8. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-kms.svg" width="40" height="40" valign="middle" /> AWS KMS (Key Management Service)
- **Category**: Cryptographic Key Management & Encryption
- **Core Purpose**: Managed service for creating and controlling cryptographic keys used to encrypt data at rest.

### Key Concepts
- **Envelope Encryption**: Generates KMS Keys (KMS keys) to encrypt data across S3, EBS, RDS, Secrets Manager, and DynamoDB.
- **FIPS 140-2 Validated**: Hardware Security Module (HSM) backing to ensure compliance and strict key audit logging via CloudTrail.
