# Management, Governance, Monitoring & Infrastructure as Code

Notes covering Amazon CloudWatch, AWS CloudTrail, AWS CloudFormation, AWS Systems Manager (SSM), and AWS Config.

---

## 1. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cloudwatch.svg" width="40" height="40" valign="middle" /> Amazon CloudWatch
- **Category**: Monitoring & Observability
- **Core Purpose**: Collects metrics, logs, and events from AWS resources and applications in real time.

### Key Concepts
- **Metrics & Alarms**: Sets threshold alarms for CPU utilization, memory, API latency, and billing costs.
- **CloudWatch Logs**: Centralized log aggregation engine for EC2 syslog, Lambda execution logs, and application logs.
- **Dashboards**: Real-time visualization of system metrics across infrastructure components.

---

## 2. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cloudtrail.svg" width="40" height="40" valign="middle" /> AWS CloudTrail
- **Category**: Governance, Compliance & Auditing
- **Core Purpose**: Records API activity and user actions across your AWS infrastructure.

### Key Concepts
- **API Audit Log**: Captures who performed what action, from which IP address, at what timestamp across the AWS Console, CLI, or SDKs.
- **Security Investigations**: Essential for security analysis, change tracking, and regulatory compliance auditing.

---

## 3. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cloudformation.svg" width="40" height="40" valign="middle" /> AWS CloudFormation
- **Category**: Infrastructure as Code (IaC)
- **Core Purpose**: Provisions and updates AWS infrastructure declaratively using code templates (YAML or JSON).

### Key Concepts
- **Automated Provisioning**: Defines stacks of AWS resources (VPCs, EC2, RDS, Lambda) that can be repeatedly deployed, updated, or torn down together.
- **State Management & Rollbacks**: Automatically handles dependency mapping between resources and rolls back updates if failures occur.

---

## 4. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-systems-manager.svg" width="40" height="40" valign="middle" /> AWS Systems Manager (SSM)
- **Category**: Operations & Fleet Management
- **Core Purpose**: Central operational hub for managing EC2 fleets and hybrid cloud infrastructure.

### Key Concepts
- **Parameter Store**: Secure hierarchical storage for configuration data and secrets management.
- **Session Manager**: Secure shell (SSH-less) browser access to EC2 instances without opening inbound SSH port 22.
- **Patch Manager & Run Command**: Automates OS software patching and script execution across instance fleets.

---

## 5. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-config.svg" width="40" height="40" valign="middle" /> AWS Config
- **Category**: Compliance & Asset Tracking
- **Core Purpose**: Continually monitors, records, and evaluates configurations of AWS resources against compliance rules.

### Key Concepts
- **Configuration History**: Maintains a timeline of resource configuration changes to audit security drift.
- **Compliance Rules**: Automatically flags non-compliant resources (e.g., publicly accessible S3 buckets or unencrypted EBS volumes).
