# Networking & Content Delivery

Notes covering Amazon Route 53, Amazon CloudFront, Amazon VPC, Internet Gateway, NAT Gateway, Route Tables, Elastic Load Balancers (ELB/ALB), and Amazon API Gateway.

---

## 1. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-route53.svg" width="40" height="40" valign="middle" /> Amazon Route 53
- **Category**: DNS & Traffic Management
- **Core Purpose**: Domain registration, DNS routing, and endpoint health monitoring.
- **Why Use It**: To route user domain requests (DNS) intelligently based on geolocation, latency, or health check status.

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
- **Why Use It**: To deliver web content, videos, and static S3 assets to global users with ultra-low latency via edge caching locations.

### Key Concepts
- **Edge Locations**: Global network of data centers that cache content physically closer to end users.
- **S3 Integration**: Acts as a caching layer in front of S3 buckets containing static content (HTML, CSS, JS, images).
- **Latency Optimization**: Prevents user traffic from traversing global networks back to origin servers (e.g., caching EU-hosted assets for North American users).

---

## 3. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-vpc.svg" width="40" height="40" valign="middle" /> Amazon VPC (Virtual Private Cloud)
- **Category**: Virtual Networking & Isolation
- **Core Purpose**: Logically isolated virtual network for hosting AWS resources securely.
- **Why Use It**: To create an isolated private virtual network to securely host and isolate your cloud resources.

### Key Concepts
- **Subnets**: Division of VPC IP address range into public subnets (internet-facing) and private subnets (isolated data/compute layers).
- **Security Groups & Network ACLs**: Stateful instance-level firewalls and stateless subnet-level boundaries.

---

## 4. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/NetworkingContentDelivery/VPCInternetGateway.png" width="40" height="40" valign="middle" /> Internet Gateway (IGW)
- **Category**: VPC Connectivity Component
- **Core Purpose**: Allows communication between resources in your VPC and the public internet.
- **Why Use It**: To allow public subnet resources (like web servers) to communicate directly to and from the internet.

### Key Concepts
- **Public Subnet Routing**: Enables instances with public IP addresses in public subnets to connect to external endpoints.
- **Bi-Directional Access**: Handles incoming user traffic and outgoing internet communication.

---

## 5. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/NetworkingContentDelivery/VPCNATGateway.png" width="40" height="40" valign="middle" /> NAT Gateway (Network Address Translation)
- **Category**: VPC Managed Gateway
- **Core Purpose**: Enables instances in private subnets to connect to the internet while blocking inbound connections from the internet.
- **Why Use It**: To let private subnet servers (like databases) download internet updates while blocking inbound traffic from the internet.

### Key Concepts
- **Outbound-Only Connectivity**: Allows private compute/DB instances to fetch software updates and external API data securely.
- **Managed High Availability**: Redundant managed gateway deployed inside public subnets.

---

## 6. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/NetworkingContentDelivery/VPCRouter.png" width="40" height="40" valign="middle" /> Route Tables
- **Category**: VPC Network Routing
- **Core Purpose**: Set of rules (routes) that determine where network traffic from subnets or gateways is directed.
- **Why Use It**: To control network traffic paths between subnets, internet gateways, and NAT gateways.

### Key Concepts
- **Subnet Association**: Every subnet must be associated with a route table controlling its traffic destination paths.

---

## 7. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-elb.svg" width="40" height="40" valign="middle" /> Elastic Load Balancer (ELB / ALB)
- **Category**: Traffic Distribution & Load Balancing
- **Core Purpose**: Horizontally scales applications by distributing incoming traffic across multiple backend compute instances.
- **Why Use It**: To automatically distribute incoming web traffic across multiple backend servers for high availability and fault tolerance.

### Key Concepts
- **Horizontal Scaling**: Routes requests to backend compute nodes (EC2 instances or containers).
- **Auto Scaling Group Integration**: Dynamically adds or removes instances based on traffic load.
- **Sticky Sessions**: Binds user sessions to specific compute instances (ideal for WebSockets and sessionful connections).

---

## 8. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-api-gateway.svg" width="40" height="40" valign="middle" /> Amazon API Gateway
- **Category**: Managed API Gateway ("Front door of AWS")
- **Core Purpose**: Managed entry point for RESTful and WebSocket APIs.
- **Why Use It**: To build, publish, rate-limit, and secure REST/WebSocket APIs for frontend client apps.

### Key Concepts
- **Direct Service Integration**: Routes HTTP requests directly to AWS services (Lambda, DynamoDB, SQS) without needing an EC2 compute layer.
- **Traffic Control**: Supports rate limiting, request throttling, and custom access key management.
- **WebSocket Support**: Handles stateful, real-time two-way communications.
