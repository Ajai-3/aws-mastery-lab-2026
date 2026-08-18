# Security, Identity, & Compliance

Notes covering AWS Certificate Manager (ACM), AWS WAF, AWS Shield, Amazon Cognito, AWS IAM, and AWS KMS.

---

## 1. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-certificate-manager.svg" width="40" height="40" valign="middle" /> AWS Certificate Manager (ACM)
- **Category**: Security & Encryption
- **Core Purpose**: Provisioning, managing, and renewing SSL/TLS certificates.
- **Why Use It**: To easily provision and automatically renew free SSL/TLS certificates for HTTPS encryption.

### Key Concepts
- **End-to-End Encryption**: Encrypts traffic in transit between users, Route 53 endpoints, CloudFront distributions, and Load Balancers.
- **Certificate Authority Integration**: Issues free public/private SSL certificates directly from AWS or imports third-party certificates.

---

## 2. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-waf.svg" width="40" height="40" valign="middle" /> AWS WAF (Web Application Firewall)
- **Category**: Web Security & Threat Mitigation
- **Core Purpose**: Protects web applications and APIs from common web exploits.
- **Why Use It**: To block web attacks (SQL injection, XSS, bots) before they hit your web apps or APIs.

### Key Concepts
- **Exploit Protection**: Guards against SQL injection (SQLi), Cross-Site Scripting (XSS), and bot scraping.
- **IP Rate Limiting & Banning**: Blocks malicious IP addresses and custom HTTP request patterns.

---

## 3. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-shield.svg" width="40" height="40" valign="middle" /> AWS Shield (Standard & Advanced)
- **Category**: DDoS Protection
- **Core Purpose**: Protects infrastructure against Distributed Denial of Service (DDoS) attacks.
- **Why Use It**: To protect applications from malicious distributed denial-of-service (DDoS) traffic spikes.

### Key Concepts
- **Shield Standard**: Free protection included automatically for all AWS customers.
- **Shield Advanced**: Paid protection tier ($3,000/mo) for mission-critical apps; provides 24/7 AWS DDoS Response Team (DRT) access and financial refund guarantees for attack-induced traffic spikes.

---

## 4. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cognito.svg" width="40" height="40" valign="middle" /> Amazon Cognito
- **Category**: Identity, Authentication & Authorization
- **Core Purpose**: User registration, authentication, and authorization.
- **Why Use It**: To add user sign-up, sign-in, MFA, and social identity logins to web and mobile apps.

### Key Concepts
- **Cognito User Pools (CUP)**: User directory for sign-up, sign-in, multi-factor authentication (MFA), and JWT token issuance.
- **Cognito Identity Pools**: Authorizes authenticated users by granting temporary AWS IAM credentials based on custom user attributes (e.g., Student vs. Admin permissions).

---

## 5. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-iam.svg" width="40" height="40" valign="middle" /> AWS IAM (Identity and Access Management)
- **Category**: Identity & Access Control
- **Core Purpose**: Controls authenticating users/services and authorizing access permissions across all AWS resources.
- **Why Use It**: To control who can access which AWS resources using least-privilege permission policies.

### Key Concepts
- **Users, Groups & Roles**: Identity primitives for assigning policies to developers, applications, and AWS compute resources.
- **Least Privilege Principle**: Fine-grained JSON policies that enforce explicit allow/deny permissions for specific actions and resources.

---

## 6. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-kms.svg" width="40" height="40" valign="middle" /> AWS KMS (Key Management Service)
- **Category**: Cryptographic Key Management & Encryption
- **Core Purpose**: Managed service for creating and controlling cryptographic keys used to encrypt data at rest.
- **Why Use It**: To generate, manage, and audit encryption keys used to encrypt data stored across AWS services.

### Key Concepts
- **Envelope Encryption**: Generates KMS Keys (KMS keys) to encrypt data across S3, EBS, RDS, Secrets Manager, and DynamoDB.
- **FIPS 140-2 Validated**: Hardware Security Module (HSM) backing to ensure compliance and strict key audit logging via CloudTrail.
