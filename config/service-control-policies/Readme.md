# HIPAA based Service Control Policies (SCPs)

baseline-guardrails-hipaa.json file contains the Service Control Policies (SCPs) that can be deployed while building HIPAA eligible environments.

HIPAA Eligible Services are last updated on June 17, 2025.

Here are some statistics about the SCPs:

- HIPAA Lookup Data Service Count: 166
- HIPAAA Eligible Services API Count: 180
- Additional Services Count: 7
- Additional Services API Count: 18
- Total APIs in SCP: 198

The reference guide can be accessed via the following link: https://aws.amazon.com/compliance/hipaa-eligible-services-reference/

Please also check the notes below for specific services:
Alexa for Business [for healthcare skills only requires Alexa Skills BAA. See [HIPAA whitepaper](https://docs.aws.amazon.com/whitepapers/latest/architecting-hipaa-security-and-compliance-on-aws/architecting-hipaa-security-and-compliance-on-aws.html) for details] \
AWS Amplify Console \
Amazon API Gateway \
AWS App Mesh \
AWS AppFabric \
Amazon AppFlow \
AWS Application Migration Service \
Amazon AppStream 2.0 \
AWS AppSync \
Amazon Athena \
AWS Audit Manager \
Amazon Augmented AI [excludes Public Workforce and Vendor Workforce for all features] \
Amazon Aurora \
AWS B2B Data Interchange \
AWS Backup \
AWS Batch \
Amazon Bedrock \
AWS Certificate Manager \
Amazon Chime \
Amazon Chime SDK \
AWS Clean Rooms \
AWS Cloud 9 \
Amazon Cloud Directory \
AWS Cloud Map \
AWS CloudEndure \
AWS CloudFormation \
Amazon CloudFront [excludes content delivery through Amazon CloudFront Embedded Point of Presences] \
AWS CloudHSM \
AWS CloudShell \
AWS CloudTrail \
Amazon CloudWatch \
Amazon CloudWatch Logs \
Amazon CloudWatch SDK Metrics \
AWS CodeBuild \
AWS CodeCommit \
AWS CodeDeploy \
AWS CodePipeline \
Amazon Cognito \
Amazon Comprehend \
Amazon Comprehend Medical \
AWS Config \
Amazon Connect \
AWS Control Tower \
AWS Data Exchange \
AWS Database Migration Service (DMS) \
AWS DataSync \
Amazon DataZone \
Amazon Detective \
Amazon DevOps Guru \
AWS Direct Connect \
AWS Directory Service [excludes Simple AD] \
Amazon DocumentDB [with MongoDB compatibility] \
Amazon DynamoDB \
Amazon EC2 Auto Scaling \
Amazon ElastiCache \
AWS Elastic Beanstalk \
Amazon Elastic Block Store (Amazon EBS) \
Amazon Elastic Compute Cloud (Amazon EC2) \
Amazon Elastic Container Registry (ECR) \
Amazon Elastic Container Service (ECS) \
AWS Elastic Disaster Recovery \
Amazon Elastic File System (EFS) \
Amazon Elastic Kubernetes Service (EKS) \
Elastic Load Balancing \
Amazon Elastic MapReduce (EMR) \
AWS Elemental MediaConnect \
AWS Elemental MediaConvert \
AWS Elemental MediaLive \
AWS Entity Resolution \
Amazon EventBridge [formerly Amazon Cloudwatch Events] \
AWS Fargate [ECS and EKS engines only] \
AWS Fault Injection Simulator \
AWS Firewall Manager \
Amazon Forecast \
Amazon FreeRTOS \
Amazon FSx \
AWS Global Accelerator \
AWS Glue \
AWS Glue DataBrew \
Amazon GuardDuty \
AWS HealthLake \
AWS HealthOmics \
AWS HealthImaging \
AWS IAM Identity Center \
Amazon Inspector \
AWS IoT Core \
AWS IoT Device Management \
AWS IoT Events \
AWS IoT Greengrass \
AWS IoT SiteWise \
Amazon Kendra \
AWS Key Management Service (KMS) \
Amazon Managed Service for Apache Flink \
Amazon Keyspaces [For Apache Cassandra] \
Amazon Kinesis Data Streams \
Amazon Kinesis Data Firehose \
Amazon Kinesis Video Streams \
AWS Lake Formation \
AWS Lambda \
Amazon Lex \
Amazon Location Service \
Amazon Macie \
AWS Mainframe Modernization \
AWS Managed Services [excluding Operations on Demand Services, except for the RFC Expedite feature] \
Amazon Managed Service for Prometheus \
Amazon Managed Workflow for Apache Airflow \
Amazon Managed Streaming for Apache Kafka \
Amazon MemoryDB \
Amazon MQ \
Amazon Neptune \
AWS Network Firewall \
Amazon OpenSearch Service \
AWS OpsWorks for Chef Automate \
AWS OpsWorks for Puppet Enterprise \
AWS OpsWorks Stacks \
AWS Organizations \
AWS Outposts \
Amazon Personalize \
Amazon Pinpoint and End User Messaging (formerly Amazon Pinpoint) [excluding Voice Message capabilities and WhatsApp Channel] \
Amazon Polly \
AWS Private Certificate Authority \
Amazon Q Business \
Amazon Quantum Ledger Database (QLDB) \
Amazon QuickSight \
Amazon Rekognition \
Amazon Redshift \
Amazon Relational Database Service (Amazon RDS) [SQL Server, MySQL, Oracle, PostgreSQL, Db2 and MariaDB engines only] \
AWS Resilience Hub \
AWS Resource Access Manager (RAM) \
AWS Resource Explorer \
AWS RoboMaker \
Amazon Route 53 \
Amazon S3 Glacier \
Amazon SageMaker AI [formerly Amazon Sagemaker, excludes Studio Lab, Ground Truth Plus, Public Workforce and Vendor Workforce for all features] \
AWS Secrets Manager \
AWS Security Hub CSPM (formerly AWS Security Hub) \
AWS Service Catalog \
AWS Serverless Application Repository \
AWS Shield [Standard and Advanced] \
Amazon Simple Email Service (Amazon SES) \
Amazon Simple Notification Service (SNS) \
Amazon Simple Queue Service (SQS) \
Amazon Simple Storage Service (S3) \
Amazon Simple Workflow Service (SWF) \
AWS Snowball \
AWS Snowball Edge \
AWS Step Functions \
AWS Storage Gateway \
AWS Systems Manager \
Amazon Textract \
Amazon Timestream \
AWS Transcribe \
AWS Transfer Family \
Amazon Translate \
AWS Verified Access \
Amazon Verified Permissions \
Amazon Virtual Private Cloud (VPC) \
AWS Web Application Firewall (WAF) \
AWS Wickr \
Amazon WorkDocs [Excluding Adding Controls for Deleting Previous File Version Feature] \
Amazon WorkLink \
Amazon WorkSpaces \
Amazon WorkSpaces Thin Client \
Amazon WorkSpaces Secure Browser \
AWS X-Ray \
VM Import/Export \

**NOTE:** If you are a Covered Entity or Business Associate as defined by the Health Insurance Portability and Accountability Act of 1996 (as amended, “HIPAA”), you agree not to use these HIPAA Eligible Services for any purpose or in any manner involving Protected Health Information (as defined by HIPAA) without first entering into an AWS business associate agreement.

Unless specifically excluded, generally available features of each of the HIPAA eligible services listed are also considered HIPAA eligible.

## "Tools" related APIs included

The featured scp-hcl-hipaa-service.json includes APIs for other tools / services within AWS that do not typically store, transform, or process PHI data.

### Service Catagory - Cost Management

- "aws-portal:\*"
- "budgets:\*"
- "ce:\*"
- "cur:\*"
- "bcm-data-exports:\*"
- "pricing:\*"

### Service Catagory - Identity and Access Management

- "apiname": "iam:\*"
- "apiname": "access-analyzer:\*"
- "apiname": "sts:\*"

### Service Catagory - Compliance

- "apiname": "artifact:\*"

### Service Catagory - License Manager

- "apiname": "license-manager:\*"

### Service Catagory - Resource Management

- "apiname": "resource-groups:\*"
- "apiname": "resource-explorer:\*",

### Service Catagory - Support

- "apiname": "support:\*"
- "apiname": "supportplans:\*"
- "apiname": "trustedadvisor:\*"

### Service Catagory - Monitoring / Telemetry

- "apiname": "pi:\*"
- "apiname": "applicationinsights:\*"

### Service Catagory - Notable Tools not intended to store, process, or transmit PHI

- "apiname": "qdeveloper:\*"