# <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws.svg" width="48" height="48" align="center" /> AWS Service Documentation & Master Notes

AWS services documented by category — compute, storage, networking, security/IAM, database, containers, messaging, AI/ML, analytics, and management/governance.

Focused on the most critical AWS services relevant to Cloud/DevOps practitioners and system design.

---

## Core Service Notes Index

All core AWS services explained in detail with official 40px AWS icons:

1. **[Networking, VPC, DNS & Content Delivery](./01-networking-and-cdn.md)**
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-route53.svg" width="40" height="40" /> **Route 53**: [Amazon Route 53](./01-networking-and-cdn.md#1-amazon-route-53)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cloudfront.svg" width="40" height="40" /> **CloudFront**: [Amazon CloudFront](./01-networking-and-cdn.md#2-amazon-cloudfront)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-certificate-manager.svg" width="40" height="40" /> **ACM**: [AWS Certificate Manager](./01-networking-and-cdn.md#3-aws-certificate-manager-acm)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-vpc.svg" width="40" height="40" /> **VPC**: [Amazon VPC](./01-networking-and-cdn.md#4-amazon-vpc-virtual-private-cloud)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/NetworkingContentDelivery/VPCInternetGateway.png" width="40" height="40" /> **Internet Gateway**: [Internet Gateway (IGW)](./01-networking-and-cdn.md#5-internet-gateway-igw)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/NetworkingContentDelivery/VPCNATGateway.png" width="40" height="40" /> **NAT Gateway**: [NAT Gateway](./01-networking-and-cdn.md#6-nat-gateway-network-address-translation)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/NetworkingContentDelivery/VPCRouter.png" width="40" height="40" /> **Route Tables**: [Route Tables](./01-networking-and-cdn.md#7-route-tables)

2. **[Storage, Secrets & Configuration](./02-storage-and-config.md)**
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-s3.svg" width="40" height="40" /> **S3**: [Amazon S3](./02-storage-and-config.md#1-amazon-s3-simple-storage-service)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Storage/ElasticBlockStore.png" width="40" height="40" /> **EBS**: [Amazon EBS](./02-storage-and-config.md#2-amazon-ebs-elastic-block-store)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Storage/EFS.png" width="40" height="40" /> **EFS**: [Amazon EFS](./02-storage-and-config.md#3-amazon-efs-elastic-file-system)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-secrets-manager.svg" width="40" height="40" /> **Secrets Manager**: [AWS Secrets Manager](./02-storage-and-config.md#4-aws-secrets-manager)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ManagementGovernance/AppConfig.png" width="40" height="40" /> **AppConfig**: [AWS AppConfig](./02-storage-and-config.md#5-aws-appconfig)

3. **[API Entry Points, Security, IAM & Key Management](./03-api-and-security.md)**
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-elb.svg" width="40" height="40" /> **ELB / ALB**: [Elastic Load Balancing](./03-api-and-security.md#1-elastic-load-balancer-elb--alb)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Compute/EC2AutoScaling.png" width="40" height="40" /> **Auto Scaling**: [Auto Scaling Groups](./03-api-and-security.md#2-auto-scaling-groups-asg)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-api-gateway.svg" width="40" height="40" /> **API Gateway**: [Amazon API Gateway](./03-api-and-security.md#3-amazon-api-gateway)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-waf.svg" width="40" height="40" /> **WAF**: [AWS WAF](./03-api-and-security.md#4-aws-waf-web-application-firewall)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-shield.svg" width="40" height="40" /> **Shield**: [AWS Shield](./03-api-and-security.md#5-aws-shield-standard--advanced)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cognito.svg" width="40" height="40" /> **Cognito**: [Amazon Cognito](./03-api-and-security.md#6-amazon-cognito)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-iam.svg" width="40" height="40" /> **IAM**: [AWS IAM](./03-api-and-security.md#7-aws-iam-identity-and-access-management)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-kms.svg" width="40" height="40" /> **KMS**: [AWS KMS](./03-api-and-security.md#8-aws-kms-key-management-service)

4. **[Compute, Containers & PaaS](./04-compute-and-containers.md)**
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-ec2.svg" width="40" height="40" /> **EC2**: [Amazon EC2](./04-compute-and-containers.md#1-amazon-ec2-elastic-compute-cloud)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-lightsail.svg" width="40" height="40" /> **Lightsail**: [AWS Lightsail](./04-compute-and-containers.md#2-aws-lightsail)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-ecs.svg" width="40" height="40" /> **ECS**: [Amazon ECS](./04-compute-and-containers.md#3-amazon-ecs-elastic-container-service)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-fargate.svg" width="40" height="40" /> **Fargate**: [AWS Fargate](./04-compute-and-containers.md#4-aws-fargate)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-eks.svg" width="40" height="40" /> **EKS**: [Amazon EKS](./04-compute-and-containers.md#5-amazon-eks-elastic-kubernetes-service)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-lambda.svg" width="40" height="40" /> **Lambda**: [AWS Lambda](./04-compute-and-containers.md#6-aws-lambda)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-elastic-beanstalk.svg" width="40" height="40" /> **Elastic Beanstalk**: [AWS Elastic Beanstalk](./04-compute-and-containers.md#7-aws-elastic-beanstalk)

5. **[Databases & In-Memory Caching](./05-databases-and-caching.md)**
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-rds.svg" width="40" height="40" /> **RDS**: [Amazon RDS](./05-databases-and-caching.md#1-amazon-rds-relational-database-service)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-aurora.svg" width="40" height="40" /> **Aurora**: [Amazon Aurora](./05-databases-and-caching.md#2-amazon-aurora--aurora-serverless)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-dynamodb.svg" width="40" height="40" /> **DynamoDB**: [Amazon DynamoDB](./05-databases-and-caching.md#3-amazon-dynamodb)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-documentdb.svg" width="40" height="40" /> **DocumentDB**: [Amazon DocumentDB](./05-databases-and-caching.md#4-amazon-documentdb)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-keyspaces.svg" width="40" height="40" /> **Keyspaces**: [Amazon Keyspaces](./05-databases-and-caching.md#5-amazon-keyspaces)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-neptune.svg" width="40" height="40" /> **Neptune**: [Amazon Neptune](./05-databases-and-caching.md#6-amazon-neptune)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-open-search.svg" width="40" height="40" /> **OpenSearch**: [Amazon OpenSearch](./05-databases-and-caching.md#7-amazon-opensearch)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Database/DatabaseMigrationService.png" width="40" height="40" /> **DMS**: [AWS DMS](./05-databases-and-caching.md#8-aws-dms-database-migration-service)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-elasticache.svg" width="40" height="40" /> **ElastiCache**: [Amazon ElastiCache](./05-databases-and-caching.md#9-amazon-elasticache)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Database/MemoryDB.png" width="40" height="40" /> **MemoryDB**: [Amazon MemoryDB](./05-databases-and-caching.md#10-amazon-memorydb)

6. **[Application Integration & Workflows](./06-integration-and-workflows.md)**
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-sns.svg" width="40" height="40" /> **SNS**: [Amazon SNS](./06-integration-and-workflows.md#1-amazon-sns-simple-notification-service)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-sqs.svg" width="40" height="40" /> **SQS**: [Amazon SQS](./06-integration-and-workflows.md#2-amazon-sqs-simple-queue-service)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-eventbridge.svg" width="40" height="40" /> **EventBridge**: [Amazon EventBridge](./06-integration-and-workflows.md#3-amazon-eventbridge)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-step-functions.svg" width="40" height="40" /> **Step Functions**: [AWS Step Functions](./06-integration-and-workflows.md#4-aws-step-functions)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ApplicationIntegration/ManagedWorkflowsforApacheAirflow.png" width="40" height="40" /> **MWAA**: [AWS MWAA](./06-integration-and-workflows.md#5-aws-mwaa-managed-workflows-for-apache-airflow)

7. **[AI / Machine Learning & Analytics](./07-ai-ml-and-analytics.md)**
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ArtificialIntelligence/Bedrock.png" width="40" height="40" /> **Bedrock**: [Amazon Bedrock](./07-ai-ml-and-analytics.md#1-amazon-bedrock)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ArtificialIntelligence/SageMakerAI.png" width="40" height="40" /> **SageMaker**: [AWS SageMaker](./07-ai-ml-and-analytics.md#2-aws-sagemaker)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ArtificialIntelligence/Rekognition.png" width="40" height="40" /> **Rekognition**: [Amazon Rekognition](./07-ai-ml-and-analytics.md#3-amazon-rekognition)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ArtificialIntelligence/Polly.png" width="40" height="40" /> **Polly**: [Amazon Polly](./07-ai-ml-and-analytics.md#4-amazon-polly)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/ArtificialIntelligence/Transcribe.png" width="40" height="40" /> **Transcribe**: [Amazon Transcribe](./07-ai-ml-and-analytics.md#5-amazon-transcribe)
   - <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Analytics/EMR.png" width="40" height="40" /> **EMR**: [Amazon EMR](./07-ai-ml-and-analytics.md#6-amazon-emr-elastic-mapreduce)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-athena.svg" width="40" height="40" /> **Athena**: [Amazon Athena](./07-ai-ml-and-analytics.md#7-amazon-athena)

8. **[Management, Governance & IaC](./08-management-and-governance.md)**
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cloudwatch.svg" width="40" height="40" /> **CloudWatch**: [Amazon CloudWatch](./08-management-and-governance.md#1-amazon-cloudwatch)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cloudtrail.svg" width="40" height="40" /> **CloudTrail**: [AWS CloudTrail](./08-management-and-governance.md#2-aws-cloudtrail)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-cloudformation.svg" width="40" height="40" /> **CloudFormation**: [AWS CloudFormation](./08-management-and-governance.md#3-aws-cloudformation)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-systems-manager.svg" width="40" height="40" /> **Systems Manager**: [AWS Systems Manager (SSM)](./08-management-and-governance.md#4-aws-systems-manager-ssm)
   - <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-config.svg" width="40" height="40" /> **AWS Config**: [AWS Config](./08-management-and-governance.md#5-aws-config)