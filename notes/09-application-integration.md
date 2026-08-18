# Application Integration

Notes covering Amazon SNS, Amazon SQS, Amazon EventBridge, AWS Step Functions, and AWS MWAA.

---

## 1. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-sns.svg" width="40" height="40" valign="middle" /> Amazon SNS (Simple Notification Service)
- **Category**: Pub/Sub Messaging
- **Core Purpose**: Asynchronous push notification pub/sub service.
- **Why Use It**: To send real-time notifications (SMS, email) and fan out messages to multiple subscriber services at once.

### Key Concepts
- **Publish-Subscribe Model**: Publishers publish a single message to an SNS topic, which immediately fans out duplicate copies to multiple subscribers.
- **Fan-Out Pattern**: Enables parallel asynchronous processing across analytics, fraud detection, and order fulfillment backend services.

---

## 2. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-sqs.svg" width="40" height="40" valign="middle" /> Amazon SQS (Simple Queue Service)
- **Category**: Message Queuing & Decoupling
- **Core Purpose**: Decouples application components using pull-based message queues.
- **Why Use It**: To decouple microservices using message queues so traffic spikes don't overwhelm downstream servers.

### Key Concepts
- **Queue Types**:
  - **Standard Queues**: Maximum throughput, best-effort message ordering, at-least-once delivery.
  - **FIFO Queues**: Strictly ordered message delivery with exactly-once processing guarantees.
- **Backpressure Prevention**: Consumers poll queues at their own processing rate, preventing spikes in API traffic from crashing downstream systems.

---

## 3. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-eventbridge.svg" width="40" height="40" valign="middle" /> Amazon EventBridge
- **Category**: Event Bus & Scheduling Engine
- **Core Purpose**: Serverless event bus connecting AWS services, SaaS apps, and custom microservices.
- **Why Use It**: To route events between AWS services, SaaS apps, and microservices using event rules and cron schedules.

### Key Concepts
- **Event Bus**: Routes native AWS system events (e.g., EC2 state changes) and application events based on message schemas and pattern rules.
- **EventBridge Rules**: Cron-style interval timers (e.g., trigger a Lambda function every 5 minutes).
- **EventBridge Scheduler**: Schedules dynamic, one-time target events at a precise timestamp in the future (e.g., sending an order status email exactly 3 hours after purchase).

---

## 4. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-step-functions.svg" width="40" height="40" valign="middle" /> AWS Step Functions
- **Category**: Serverless Visual Workflow Orchestration
- **Core Purpose**: Orchestrates complex microservice workflows using visual state machines.
- **Why Use It**: To orchestrate complex multi-step workflows and microservices using visual state machines with built-in retries.

### Key Concepts
- **Distributed Graph Workflows**: Coordinates multi-step tasks, parallel branch execution, conditional if/else logic, and custom error handling/retries.
- **Deep Service Integration**: Directly invokes Lambda, SQS, SNS, ECS, and DynamoDB operations out of the box.

---

## 5. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ApplicationIntegration/ManagedWorkflowsforApacheAirflow.png" width="40" height="40" valign="middle" /> AWS MWAA (Managed Workflows for Apache Airflow)
- **Category**: Open-Source Workflow Orchestration
- **Core Purpose**: Managed Apache Airflow service for building data engineering pipelines.
- **Why Use It**: To run managed Apache Airflow data engineering pipelines without managing underlying infrastructure.

### Key Concepts
- **Open-Source Alternative**: Provides an open-source Python-based DAG workflow orchestration engine to avoid cloud vendor lock-in.
