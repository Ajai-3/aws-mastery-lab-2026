# Networking, VPC, DNS & Content Delivery

Notes covering Amazon Route 53, Amazon CloudFront, AWS Certificate Manager (ACM), Amazon VPC, Internet Gateway, NAT Gateway, and Route Tables.

---

## 1. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-route53.svg" width="40" height="40" valign="middle" /> Amazon Route 53
- **Category**: DNS & Traffic Management
- **Core Purpose**: Domain registration, DNS routing, and endpoint health monitoring.

### Key Concepts & Routing Types
1. **Domain Registration & DNS**: Maps domain names (e.g., `amazon.com`) to IP addresses or AWS alias targets.
2. **Health Checks**: Pings back-end infrastructure (e.g., EC2 instances, load balancers) to route traffic only to available and stable nodes.
3. **Routing Policies**:
   - **Geolocation Routing**: Directs requests based on user location to the closest geographical region.
   - **Latency-Based Routing**: Directs requests to the region offering the lowest latency for the specific user.
   - **Failover Routing**: Redirects traffic to secondary backup infrastructure if the primary health checks fail.
   - **Weighted Routing**: Distributes traffic across multiple endpoints according to assigned weights (useful for canary deployments and A/B testing).

---

## 2. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cloudfront.svg" width="40" height="40" valign="middle" /> Amazon CloudFront
- **Category**: Content Delivery Network (CDN)
- **Core Purpose**: Low-latency global content delivery using regional caching and edge locations.

### Key Concepts
- **Edge Locations**: Global network of data centers that cache content physically closer to end users.
- **S3 Integration**: Acts as a caching layer in front of S3 buckets containing static content (HTML, CSS, JS, images).
- **Latency Optimization**: Prevents user traffic from traversing global networks back to origin servers (e.g., caching EU-hosted assets for North American users).

---

## 3. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-certificate-manager.svg" width="40" height="40" valign="middle" /> AWS Certificate Manager (ACM)
- **Category**: Security & Encryption
- **Core Purpose**: Provisioning, managing, and renewing SSL/TLS certificates.

### Key Concepts
- **End-to-End Encryption**: Encrypts traffic in transit between users, Route 53 endpoints, CloudFront distributions, and Load Balancers.
- **Certificate Authority Integration**: Issues free public/private SSL certificates directly from AWS or imports third-party certificates.

---

## 4. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-vpc.svg" width="40" height="40" valign="middle" /> Amazon VPC (Virtual Private Cloud)
- **Category**: Virtual Networking & Isolation
- **Core Purpose**: Logically isolated virtual network for hosting AWS resources securely.

### Key Concepts
- **Subnets**: Division of VPC IP address range into public subnets (internet-facing) and private subnets (isolated data/compute layers).
- **Security Groups & Network ACLs**: Stateful instance-level firewalls and stateless subnet-level boundaries.

---

## 5. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/NetworkingContentDelivery/VPCInternetGateway.png" width="40" height="40" valign="middle" /> Internet Gateway (IGW)
- **Category**: VPC Connectivity Component
- **Core Purpose**: Allows communication between resources in your VPC and the public internet.

### Key Concepts
- **Public Subnet Routing**: Enables instances with public IP addresses in public subnets to connect to external endpoints.
- **Bi-Directional Access**: Handles incoming user traffic and outgoing internet communication.

---

## 6. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/NetworkingContentDelivery/VPCNATGateway.png" width="40" height="40" valign="middle" /> NAT Gateway (Network Address Translation)
- **Category**: VPC Managed Gateway
- **Core Purpose**: Enables instances in private subnets to connect to the internet while blocking inbound connections from the internet.

### Key Concepts
- **Outbound-Only Connectivity**: Allows private compute/DB instances to fetch software updates and external API data securely.
- **Managed High Availability**: Redundant managed gateway deployed inside public subnets.

---

## 7. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/NetworkingContentDelivery/VPCRouter.png" width="40" height="40" valign="middle" /> Route Tables
- **Category**: VPC Network Routing
- **Core Purpose**: Set of rules (routes) that determine where network traffic from subnets or gateways is directed.

### Key Concepts
- **Target Destinations**: Directs subnet traffic to local VPC addresses, Internet Gateways (`0.0.0.0/0`), NAT Gateways, or VPC Peering endpoints.
- **Explicit Subnet Association**: Assigns custom route tables to public vs. private subnets.
