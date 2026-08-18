# <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws.svg" width="48" height="48" align="center" /> AWS Service Documentation & Master Notes

AWS services documented by standard AWS functional categories and primary icon color schemes — Compute & Containers, Storage, Databases, Networking & Content Delivery, Security, Identity & Compliance, Management & Governance, Analytics, Machine Learning & AI, and Application Integration.

Focused on the most critical AWS services relevant to Cloud/DevOps practitioners and system design.

---

## Core Service Notes Index

1. **[Compute & Containers](./01-compute-and-containers.md)** *(Orange)*
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-ec2.svg" width="40" height="40" /> **EC2**: [Amazon EC2](./01-compute-and-containers.md#1-amazon-ec2-elastic-compute-cloud)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-lightsail.svg" width="40" height="40" /> **Lightsail**: [AWS Lightsail](./01-compute-and-containers.md#2-aws-lightsail)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-ecs.svg" width="40" height="40" /> **ECS**: [Amazon ECS](./01-compute-and-containers.md#3-amazon-ecs-elastic-container-service)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-fargate.svg" width="40" height="40" /> **Fargate**: [AWS Fargate](./01-compute-and-containers.md#4-aws-fargate)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-eks.svg" width="40" height="40" /> **EKS**: [Amazon EKS](./01-compute-and-containers.md#5-amazon-eks-elastic-kubernetes-service)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-lambda.svg" width="40" height="40" /> **Lambda**: [AWS Lambda](./01-compute-and-containers.md#6-aws-lambda)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-elastic-beanstalk.svg" width="40" height="40" /> **Elastic Beanstalk**: [AWS Elastic Beanstalk](./01-compute-and-containers.md#7-aws-elastic-beanstalk)

2. **[Storage](./02-storage.md)** *(Green)*
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-s3.svg" width="40" height="40" /> **S3**: [Amazon S3](./02-storage.md#1-amazon-s3-simple-storage-service)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Storage/ElasticBlockStore.png" width="40" height="40" /> **EBS**: [Amazon EBS](./02-storage.md#2-amazon-ebs-elastic-block-store)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Storage/EFS.png" width="40" height="40" /> **EFS**: [Amazon EFS](./02-storage.md#3-amazon-efs-elastic-file-system)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-secrets-manager.svg" width="40" height="40" /> **Secrets Manager**: [AWS Secrets Manager](./02-storage.md#4-aws-secrets-manager)

3. **[Databases](./03-databases.md)** *(Blue)*
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-rds.svg" width="40" height="40" /> **RDS**: [Amazon RDS](./03-databases.md#1-amazon-rds-relational-database-service)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-aurora.svg" width="40" height="40" /> **Aurora**: [Amazon Aurora](./03-databases.md#2-amazon-aurora--aurora-serverless)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-dynamodb.svg" width="40" height="40" /> **DynamoDB**: [Amazon DynamoDB](./03-databases.md#3-amazon-dynamodb)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-documentdb.svg" width="40" height="40" /> **DocumentDB**: [Amazon DocumentDB](./03-databases.md#4-amazon-documentdb)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-keyspaces.svg" width="40" height="40" /> **Keyspaces**: [Amazon Keyspaces](./03-databases.md#5-amazon-keyspaces)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-neptune.svg" width="40" height="40" /> **Neptune**: [Amazon Neptune](./03-databases.md#6-amazon-neptune)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Database/DatabaseMigrationService.png" width="40" height="40" /> **DMS**: [AWS DMS](./03-databases.md#7-aws-dms-database-migration-service)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-elasticache.svg" width="40" height="40" /> **ElastiCache**: [Amazon ElastiCache](./03-databases.md#8-amazon-elasticache)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Database/MemoryDB.png" width="40" height="40" /> **MemoryDB**: [Amazon MemoryDB](./03-databases.md#9-amazon-memorydb)

4. **[Networking & Content Delivery](./04-networking-and-content-delivery.md)** *(Purple)*
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-route53.svg" width="40" height="40" /> **Route 53**: [Amazon Route 53](./04-networking-and-content-delivery.md#1-amazon-route-53)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cloudfront.svg" width="40" height="40" /> **CloudFront**: [Amazon CloudFront](./04-networking-and-content-delivery.md#2-amazon-cloudfront)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-vpc.svg" width="40" height="40" /> **VPC**: [Amazon VPC](./04-networking-and-content-delivery.md#3-amazon-vpc-virtual-private-cloud)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/NetworkingContentDelivery/VPCInternetGateway.png" width="40" height="40" /> **Internet Gateway**: [Internet Gateway (IGW)](./04-networking-and-content-delivery.md#4-internet-gateway-igw)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/NetworkingContentDelivery/VPCNATGateway.png" width="40" height="40" /> **NAT Gateway**: [NAT Gateway](./04-networking-and-content-delivery.md#5-nat-gateway-network-address-translation)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/NetworkingContentDelivery/VPCRouter.png" width="40" height="40" /> **Route Tables**: [Route Tables](./04-networking-and-content-delivery.md#6-route-tables)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-elb.svg" width="40" height="40" /> **ELB / ALB**: [Elastic Load Balancing](./04-networking-and-content-delivery.md#7-elastic-load-balancer-elb--alb)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-api-gateway.svg" width="40" height="40" /> **API Gateway**: [Amazon API Gateway](./04-networking-and-content-delivery.md#8-amazon-api-gateway)

5. **[Security, Identity, & Compliance](./05-security-identity-and-compliance.md)** *(Red)*
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-certificate-manager.svg" width="40" height="40" /> **ACM**: [AWS Certificate Manager](./05-security-identity-and-compliance.md#1-aws-certificate-manager-acm)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-waf.svg" width="40" height="40" /> **WAF**: [AWS WAF](./05-security-identity-and-compliance.md#2-aws-waf-web-application-firewall)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-shield.svg" width="40" height="40" /> **Shield**: [AWS Shield](./05-security-identity-and-compliance.md#3-aws-shield-standard--advanced)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cognito.svg" width="40" height="40" /> **Cognito**: [Amazon Cognito](./05-security-identity-and-compliance.md#4-amazon-cognito)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-iam.svg" width="40" height="40" /> **IAM**: [AWS IAM](./05-security-identity-and-compliance.md#5-aws-iam-identity-and-access-management)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-kms.svg" width="40" height="40" /> **KMS**: [AWS KMS](./05-security-identity-and-compliance.md#6-aws-kms-key-management-service)

6. **[Management & Governance](./06-management-and-governance.md)** *(Pink / Magenta)*
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cloudwatch.svg" width="40" height="40" /> **CloudWatch**: [Amazon CloudWatch](./06-management-and-governance.md#1-amazon-cloudwatch)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cloudtrail.svg" width="40" height="40" /> **CloudTrail**: [AWS CloudTrail](./06-management-and-governance.md#2-aws-cloudtrail)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cloudformation.svg" width="40" height="40" /> **CloudFormation**: [AWS CloudFormation](./06-management-and-governance.md#3-aws-cloudformation)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-systems-manager.svg" width="40" height="40" /> **Systems Manager**: [AWS Systems Manager (SSM)](./06-management-and-governance.md#4-aws-systems-manager-ssm)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-config.svg" width="40" height="40" /> **AWS Config**: [AWS Config](./06-management-and-governance.md#5-aws-config)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ManagementGovernance/AppConfig.png" width="40" height="40" /> **AppConfig**: [AWS AppConfig](./06-management-and-governance.md#6-aws-appconfig)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Compute/EC2AutoScaling.png" width="40" height="40" /> **Auto Scaling**: [Auto Scaling Groups](./06-management-and-governance.md#7-auto-scaling-groups-asg)

7. **[Analytics](./07-analytics.md)** *(Dark Blue / Indigo)*
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-athena.svg" width="40" height="40" /> **Athena**: [Amazon Athena](./07-analytics.md#1-amazon-athena)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Analytics/EMR.png" width="40" height="40" /> **EMR**: [Amazon EMR](./07-analytics.md#2-amazon-emr-elastic-mapreduce)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-open-search.svg" width="40" height="40" /> **OpenSearch**: [Amazon OpenSearch](./07-analytics.md#3-amazon-opensearch)

8. **[Machine Learning & AI](./08-machine-learning-and-ai.md)** *(Teal / Seafoam Green)*
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ArtificialIntelligence/Bedrock.png" width="40" height="40" /> **Bedrock**: [Amazon Bedrock](./08-machine-learning-and-ai.md#1-amazon-bedrock)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ArtificialIntelligence/SageMakerAI.png" width="40" height="40" /> **SageMaker**: [AWS SageMaker](./08-machine-learning-and-ai.md#2-aws-sagemaker)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ArtificialIntelligence/Rekognition.png" width="40" height="40" /> **Rekognition**: [Amazon Rekognition](./08-machine-learning-and-ai.md#3-amazon-rekognition)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ArtificialIntelligence/Polly.png" width="40" height="40" /> **Polly**: [Amazon Polly](./08-machine-learning-and-ai.md#4-amazon-polly)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ArtificialIntelligence/Transcribe.png" width="40" height="40" /> **Transcribe**: [Amazon Transcribe](./08-machine-learning-and-ai.md#5-amazon-transcribe)

9. **[Application Integration](./09-application-integration.md)** *(Coral / Light Red)*
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-sns.svg" width="40" height="40" /> **SNS**: [Amazon SNS](./09-application-integration.md#1-amazon-sns-simple-notification-service)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-sqs.svg" width="40" height="40" /> **SQS**: [Amazon SQS](./09-application-integration.md#2-amazon-sqs-simple-queue-service)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-eventbridge.svg" width="40" height="40" /> **EventBridge**: [Amazon EventBridge](./09-application-integration.md#3-amazon-eventbridge)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-step-functions.svg" width="40" height="40" /> **Step Functions**: [AWS Step Functions](./09-application-integration.md#4-aws-step-functions)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ApplicationIntegration/ManagedWorkflowsforApacheAirflow.png" width="40" height="40" /> **MWAA**: [AWS MWAA](./09-application-integration.md#5-aws-mwaa-managed-workflows-for-apache-airflow)