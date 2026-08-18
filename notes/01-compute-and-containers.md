# Compute & Containers

Notes covering Amazon EC2, AWS Lightsail, Amazon ECS, AWS Fargate, Amazon EKS, AWS Lambda, and AWS Elastic Beanstalk.

---

## 1. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-ec2.svg" width="40" height="40" valign="middle" /> Amazon EC2 (Elastic Compute Cloud)
- **Category**: Virtual Server Compute (IaaS)
- **Core Purpose**: Flexible, raw virtual machine instances.
- **Why Use It**: When you need full control over virtual servers with custom OS, CPU, memory, and software.

### Key Concepts
- **Full Flexibility**: Rent virtual instances running Linux, Windows, or specialized OS configurations.
- **Administrative Overhead**: Requires manual management of OS patching, security updates, scaling rules, and lifecycle maintenance.

---

## 2. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-lightsail.svg" width="40" height="40" valign="middle" /> AWS Lightsail
- **Category**: Simplified VPS ("EC2 for Beginners")
- **Core Purpose**: Simplified application hosting with predictable monthly costs.
- **Why Use It**: To quickly launch simple websites or apps with fixed monthly pricing and minimal cloud setup.

### Key Concepts
- **Turnkey Templates**: Pre-configured blueprints for popular frameworks (WordPress, LAMP, Node.js, Docker).
- **Simplified Management**: Guided console experience with bundled compute, storage, and networking.

---

## 3. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-ecs.svg" width="40" height="40" valign="middle" /> Amazon ECS (Elastic Container Service)
- **Category**: Docker Container Orchestration
- **Core Purpose**: Scalable container management platform for deploying and running Docker containers.
- **Why Use It**: To easily deploy, manage, and scale Docker containerized applications.

### Key Concepts
- **Container Lifecycle Management**: Handles task definitions, health checking, and container scheduling across compute clusters.
- **Deployment Models**: Can run containers on EC2 instance clusters or on AWS Fargate.

---

## 4. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-fargate.svg" width="40" height="40" valign="middle" /> AWS Fargate
- **Category**: Serverless Container Compute
- **Core Purpose**: Runs ECS/EKS containers without managing underlying server instances.
- **Why Use It**: To run containers without provisioning, scaling, or maintaining EC2 servers.

### Key Concepts
- **Serverless Containers**: Specify memory and CPU requirements per task, and AWS provisions the execution environment automatically.
- **Low Maintenance**: Eliminates EC2 cluster management, capacity planning, and server patching.

---

## 5. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-eks.svg" width="40" height="40" valign="middle" /> Amazon EKS (Elastic Kubernetes Service)
- **Category**: Managed Kubernetes Orchestration
- **Core Purpose**: Managed Kubernetes control plane.
- **Why Use It**: To run standard Kubernetes workloads using existing K8s tools without managing control plane nodes.

### Key Concepts
- **Kubernetes Native**: Fully compatible with standard Kubernetes tooling and Helm charts.
- **Fargate Integration**: Supports serverless Kubernetes pod execution using Fargate.

---

## 6. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-lambda.svg" width="40" height="40" valign="middle" /> AWS Lambda
- **Category**: Serverless Event-Driven Compute (FaaS)
- **Core Purpose**: Runs application logic automatically in response to events without provisioning servers.
- **Why Use It**: To run code automatically on-demand and pay only for exact execution time, with zero idle server costs.

### Key Concepts
- **Zero Server Management**: Deploy code functions directly; AWS handles scaling, patching, and fault tolerance.
- **Scales to Zero**: Only pay for the exact execution duration (ms) and memory used; zero cost when idle.
- **Rich AWS Integration**: Native event triggers from API Gateway, S3 uploads, SQS queues, Cognito actions, and DynamoDB streams.

---

## 7. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-elastic-beanstalk.svg" width="40" height="40" valign="middle" /> AWS Elastic Beanstalk
- **Category**: Platform as a Service (PaaS)
- **Core Purpose**: Easy-to-use service for deploying web applications written in Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker.
- **Why Use It**: To quickly deploy web apps by simply uploading code while AWS handles servers, scaling, and load balancing.

### Key Concepts
- **Automated Provisioning**: Upload application code and Elastic Beanstalk automatically handles deployment, capacity provisioning, load balancing, and auto-scaling.
- **Full Infrastructure Control**: Maintains full control over the underlying AWS resources (EC2, ELB, ASG) powering the application.
