# Management & Governance

Notes covering Amazon CloudWatch, AWS CloudTrail, AWS CloudFormation, AWS Systems Manager (SSM), AWS Config, AWS AppConfig, and Auto Scaling Groups (ASG).

---

## 1. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cloudwatch.svg" width="40" height="40" valign="middle" /> Amazon CloudWatch
- **Category**: Monitoring & Observability
- **Core Purpose**: Collects metrics, logs, and events from AWS resources and applications in real time.
- **Why Use It**: To monitor infrastructure health, collect operational logs, and trigger alerts on system performance metrics.

### Key Concepts
- **Metrics & Alarms**: Sets threshold alarms for CPU utilization, memory, API latency, and billing costs.
- **CloudWatch Logs**: Centralized log aggregation engine for EC2 syslog, Lambda execution logs, and application logs.
- **Dashboards**: Real-time visualization of system metrics across infrastructure components.

---

## 2. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cloudtrail.svg" width="40" height="40" valign="middle" /> AWS CloudTrail
- **Category**: Governance, Compliance & Auditing
- **Core Purpose**: Records API activity and user actions across your AWS infrastructure.
- **Why Use It**: To record and audit every user and API action across your AWS account for security compliance.

### Key Concepts
- **API Audit Log**: Captures who performed what action, from which IP address, at what timestamp across the AWS Console, CLI, or SDKs.
- **Security Investigations**: Essential for security analysis, change tracking, and regulatory compliance auditing.

---

## 3. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cloudformation.svg" width="40" height="40" valign="middle" /> AWS CloudFormation
- **Category**: Infrastructure as Code (IaC)
- **Core Purpose**: Provisions and updates AWS infrastructure declaratively using code templates (YAML or JSON).
- **Why Use It**: To automate cloud infrastructure deployments using reusable code templates instead of manual setup.

### Key Concepts
- **Automated Provisioning**: Defines stacks of AWS resources (VPCs, EC2, RDS, Lambda) that can be repeatedly deployed, updated, or torn down together.
- **State Management & Rollbacks**: Automatically handles dependency mapping between resources and rolls back updates if failures occur.

---

## 4. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-systems-manager.svg" width="40" height="40" valign="middle" /> AWS Systems Manager (SSM)
- **Category**: Operations & Fleet Management
- **Core Purpose**: Central operational hub for managing EC2 fleets and hybrid cloud infrastructure.
- **Why Use It**: To remotely manage server fleets, run commands securely without SSH ports, and store configuration data.

### Key Concepts
- **Parameter Store**: Secure hierarchical storage for configuration data and secrets management.
- **Session Manager**: Secure shell (SSH-less) browser access to EC2 instances without opening inbound SSH port 22.
- **Patch Manager & Run Command**: Automates OS software patching and script execution across instance fleets.

---

## 5. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-config.svg" width="40" height="40" valign="middle" /> AWS Config
- **Category**: Compliance & Asset Tracking
- **Core Purpose**: Continually monitors, records, and evaluates configurations of AWS resources against compliance rules.
- **Why Use It**: To track resource configuration changes and automatically audit security compliance rules.

### Key Concepts
- **Configuration History**: Maintains a timeline of resource configuration changes to audit security drift.
- **Compliance Rules**: Automatically flags non-compliant resources (e.g., publicly accessible S3 buckets or unencrypted EBS volumes).

---

## 6. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ManagementGovernance/AppConfig.png" width="40" height="40" valign="middle" /> AWS AppConfig
- **Category**: Dynamic Configuration Management
- **Core Purpose**: Real-time feature flags and application configuration management.
- **Why Use It**: To safely deploy feature flags and dynamic configuration changes instantly without redeploying code.

### Key Concepts
- **Runtime Branching**: Toggles feature flags and configuration settings without redeploying backend code.
- **Continuous Polling Agent**: Local compute sidecars poll AppConfig for updates to reduce network overhead and latency.

---

## 7. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Compute/EC2AutoScaling.png" width="40" height="40" valign="middle" /> Auto Scaling Groups (ASG)
- **Category**: Fleet & Capacity Governance
- **Core Purpose**: Automatically scales EC2/compute fleets based on demand rules and health checks.
- **Why Use It**: To automatically adjust EC2 instance capacity based on traffic demand and replace unhealthy servers.

### Key Concepts
- **Dynamic Fleet Sizing**: Adds compute capacity during peak loads and scales down during low-traffic periods.
- **Self-Healing Infrastructure**: Replaces unhealthy compute instances automatically.
