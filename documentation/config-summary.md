# Landing Zone Accelerator on AWS for Healthcare - Configuration Summary

This file serves to provide a general overall summary of the LZA for Healthcare configuration files.

> **_Note_:** LZA administrators are required to review all configuration files and adjust to fit organizational security and compliance needs. This configuration does not inheritly provide full compliance for any framework.\_\_\_

Visit the [configuration reference](https://awslabs.github.io/landing-zone-accelerator-on-aws/) to explore available customizations for LZA.

## **_Table of Contents_**

| Section                                                                                       | Relevant Config File     |
| --------------------------------------------------------------------------------------------- | ------------------------ |
| [Global Configuration](#global-configuration)                                                 | global-config.yaml       |
| [Account Configuration](#account-configuration)                                               | accounts-config.yaml     |
| [Identity and Access Management Configuration](#identity-and-access-management-configuration) | iam-config.yaml          |
| [Network Configuration](#network-configuration)                                               | network-config.yaml      |
| [AWS Organization Configuration](#aws-organizations-configuration)                            | organization-config.yaml |
| [Security Configuration](#security-configuration)                                             | security-config.yaml     |
| [Replacements Configuration](#replacements-configuration)                                     | replacements-config.yaml |
| [AWS Config Customer Rule Details](#aws-config-customer-rule-details)                         | security-config.yaml     |

### **Global Configuration**

| Configuration Item       | Status    | Detail                                                                                                                                                                     |
| ------------------------ | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Home Region              | Defined   | us-east-1                                                                                                                                                                  |
| Enabled Regions          | us-east-1 | Uses Home Region Variable                                                                                                                                                  |
| CloudWatch Log Retention | Defined   | 3653 Days                                                                                                                                                                  |
| Termination Protection   | Enabled   |
| CDK Options              | Defined   | centralizeBuckets: true <br> useManagementAccessRole: true                                                                                                                 |
| SNS Topics               | Defined   | SecurityHigh<br>SecurityMedium<br>SecurityLow                                                                                                                              |
| Control Tower            | Enabled   | version: 3.3<br>loggingBucketRetentionDays: 365<br>accessLoggingBucketRetentionDays: 3650                                                                                  |
| Logging                  | Defined   | account: LogArchive<br><u>cloudtrail</u><br><u>sessionManager</u><br><u>cloudwatchLogs</u><br><u>accessLogBucket</u><br><u>centralLogBucket</u><br><u>elbLogBucket</u><br> |
| Reports                  | Defined   | <u>costAndUsageReport</u><br>-format: Parquet<br>-timeUnit: DAILY<br><u>budgets</u><br>timeUnit: MONTHLY<br>amount: 2000                                                   |
| AWS Backup Vault         | Enabled   |

### **Account Configuration**

| Configuration Item     | Status  | Detail                                  |
| ---------------------- | ------- | --------------------------------------- |
| Mandatory AWS Accounts | Defined | Update Email Address Placeholders       |
| Workload AWS Accounts  | Defined | Network-Prod<br>HIS-Dev<br>EIS-Prod<br> |

### **Identity and Access Management Configuration**

| Configuration Item | Status            | Detail                                                                                                                                                                                              |
| ------------------ | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IAM Policy Sets    | Defined           | Default Boundary Policy: restricts permissions an identity-based policy can grant an IAM entity<br>IAM User Boundary Policy: restricts permissions an identity-based policy can grant an IAM entity |
| IAM Role Sets      | IAM Roles Defined | SSM <br> Backup <br> Budgets                                                                                                                                                                        |
| IAM Group Sets     | Groups Defined    | BreakGlassAdministrators                                                                                                                                                                            |
| IAM User Sets      | Users Defined     | BreakGlass Users                                                                                                                                                                                    |

### **Network Configuration**

An example network architecture diagram is provided below to demonstrate the intent of using AWS Transit Gateway to connect VPCs in AWS Accounts managed by LZA.
| Configuration Item | Status | Detail |
| - | - | - |
| Delete Default VPC | Enabled | |
| Transit Gateway | Enabled | Shared with Infra-Prod, HIS-Non-Prod, and EIS-Prod OUs |
| VPC Endpoint Policies | Defined | Default and EC2 VPC endpoints |
| Provisioned VPCs | Defined | -Network-Endpoints <br> -Network-Inspection <br> -EIS-Prod <br> -HIS-Dev |
| Global VPC Flow Logs | Enabled | Delivery to CloudWatch Logs | |

### **AWS Organizations Configuration**

| Configuration Item       | Status  | Detail                                                                                                                                                                 |
| ------------------------ | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AWS Organizations        | Enabled |                                                                                                                                                                        |
| Organizational Units     | Defined | Security <br> Infrastructure <br> Infrastructure/Infra-Dev <br> Infrastructure/Infra-Prod <br> HIS <br> HIS/HIS-Prod <br> EIS <br> EIS/EIS-Prod                        |
| Quarantine New Accounts  | Enabled |
| Service Control Policies | Enabled | <u>quarantine.json</u> <br> <u>scp-accelerator1.json</u> <br><u>scp-accelerator1.json</u> <br><u>scp-hcl-base-root.json</u> <br><u>scp-hcl-hipaa-service.json</u> <br> |
| Tagging Policies         | Enabled | Organization Tagging Policy <br> Healthcare Organization Tagging Policy                                                                                                |
| Backup Policy            | Enabled | Organization Backup Policy                                                                                                                                             |

### **Security Configuration**

| Configuration Item      | Status            | Detail                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ----------------------- | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| centralSecurityServices | Defined           | account: Audit                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Amazon Macie            | Disabled          |
| Amazon GuardDuty        | Enabled           |
| AWS Audit Manager       | Enabled           |
| Amazon Detective        | Disabled          |
| AWS SecurityHub         | Standards Enabled | AWS Foundational Security Best Practices <br> CIS AWS Foundations Benchmark v1.4.0 <br> NIST Special Publication 800-53 Revision 5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| AWS Access Analyzer     | Enabled           |
| IAM Password Policy     | Defined           |
| AWS Config              | Enabled           | <u>Custom Rules: Review [Details Table](#aws-config-custom-rule-details)</u> <br> -accelerator-attach-ec2-instance-profile <br> -accelerator-ec2-instance-profile-permission <br> -accelerator-s3-bucket-server-side-encryption-enabled <br> -accelerator-s3-bucket-enforce-https<br> -accelerator-elb-logging-enabled<br> <u>Managed Rules</u> <br> -accelerator-iam-user-group-membership-check <br> -accelerator-securityhub-enabled <br> -accelerator-cloudtrail-enabled <br> -accelerator-cloudwatch-alarm-action-check <br> -accelerator-cloudtrail-s3-dataevents-enabled <br> -accelerator-emr-kerberos-enabled <br> -accelerator-iam-group-has-users-check <br> -accelerator-s3-bucket-policy-grantee-check <br> -accelerator-internet-gateway-authorized-vpc-only <br> -accelerator-iam-no-inline-policy-check <br> -accelerator-cloudwatch-log-group-encrypted <br> -accelerator-ec2-instance-detailed-monitoring-enabled <br> -accelerator-ec2-volume-inuse-check <br> -accelerator-cloudtrail-security-trail-enabled <br> -accelerator-guardduty-non-archived-findings <br> -accelerator-sagemaker-endpoint-configuration-kms-key-configured <br> -accelerator-sagemaker-notebook-instance-kms-key-configured <br> -accelerator-dynamodb-table-encrypted-kms <br> --accelerator-codebuild-project-artifact-encryption <br> -accelerator-dynamodb-throughput-limit-check <br>-accelerator-ebs-optimized-instance <br> -accelerator-lambda-dlq-check <br> -accelerator-no-unrestricted-route-to-igw <br> -accelerator-secretsmanager-using-cmk <br> -accelerator-backup-plan-min-frequency-and-min-retention-check<br>-accelerator-backup-recovery-point-minimum-retention-check<br>-accelerator-backup-recovery-point-manual-deletion-disabled<br>-accelerator-athena-workgroup-encrypted-at-rest<br>-accelerator-aurora-last-backup-recovery-point-created<br>-accelerator-aurora-meets-restore-time-target<br>-accelerator-aurora-resources-protected-by-backup-plan<br>-accelerator-cloudfront-origin-access-identity-enabled<br>-accelerator-cloudfront-security-policy-check<br>-accelerator-cloudwatch-alarm-resource-check<br>-accelerator-custom-schema-registry-policy-attached<br>-accelerator-dynamodb-last-backup-recovery-point-created<br>-accelerator-dynamodb-table-encryption-enabled<br>-accelerator-ebs-in-backup-plan<br>-accelerator-ebs-last-backup-recovery-point-created<br>-accelerator-ec2-last-backup-recovery-point-created<br>-accelerator-ec2-managedinstance-association-compliance-status-check<br>-accelerator-ec2-no-amazon-key-pair<br>-accelerator-ec2-resources-protected-by-backup-plan<br>-accelerator-efs-last-backup-recovery-point-created<br>-accelerator-efs-resources-protected-by-backup-plan<br>-accelerator-eks-cluster-secrets-encrypted<br>-accelerator-elb-custom-security-policy-ssl-check<br>-accelerator-fms-shield-resource-policy-check<br>-accelerator-fsx-resources-protected-by-backup-plan<br>-accelerator-iam-customer-policy-blocked-kms-actions<br>-accelerator-iam-inline-policy-blocked-kms-actions<br>-accelerator-iam-policy-blacklisted-check<br>-accelerator-iam-policy-in-use<br>-accelerator-iam-role-managed-policy-check<br>-accelerator-mq-cloudwatch-audit-logging-enabled<br>-accelerator-rds-in-backup-plan<br>-accelerator-rds-last-backup-recovery-point-created<br>-accelerator-redshift-audit-logging-enabled<br>-accelerator-redshift-cluster-configuration-check<br>-accelerator-s3-account-level-public-access-blocks<br>-accelerator-s3-bucket-policy-not-more-permissive<br>-accelerator-s3-resources-protected-by-backup-plan<br>-accelerator-shield-drt-access<br>-accelerator-storagegateway-resources-protected-by-backup-plan<br>-accelerator-virtualmachine-resources-protected-by-backup-plan<br> |
| Amazon CloudWatch       | Enabled           | <u>Custom metrics and alarms</u> <br> RootAccountUsage<br>IAMPolicyChanges<br>CloudTrailChanges<br>ConsoleAuthenticationFailure<br>DisableOrDeleteCMK<br>S3BucketPolicyChanges<br>AWSConfigChanges<br>SecurityGroupChanges<br>NetworkACLChanges<br>NetworkGatewayChanges<br>RouteTableChanges<br>VPCChanges<br>BreakglassAccountUsage<br>\*update CloudWatch Alarm target SNS topics as needed                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

### **Replacements Configuration**

| Configuration Item    | Status  | Detail             |
| --------------------- | ------- | ------------------ |
| AcceleratorHomeRegion | Defined | us-east-1          |
| SecurityHigh Email    | Defined | update placeholder |
| SecurityMedium Email  | Defined | update placeholder |
| SecurityLow Email     | Defined | update placeholder |
| Budget Email          | Defined | update placeholder |

#### AWS Config Custom Rule Details

> **_Disclaimer_:** Testing and validation have been performed on the code in each rule, however, edge cases may exist that are not accounted for.
>
> Review the code for each rule in `config/custom-config-rules/src/` to make sure it meets your needs. Open a GitHub Issue to report bugs or feature requests.

| Custom Rule                                          | Intent                                               | Reference                                                                                                                                                               |
| ---------------------------------------------------- | ---------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| accelerator-attach-ec2-instance-profile              | Ensure EC2 instances have an IAM profile attached.   | [Managing EC2 Instance Profiles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html#instance-profiles-manage-cli-api) |
| accelerator-ec2-instance-profile-permission          | EC2 instance profile should include LZA permissions. | [LZA Administrative Role](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/administrative-role.html)                                        |
| accelerator-s3-bucket-server-side-encryption-enabled | S3 buckets should enforce server-side encryption.    | [Protecting Data with Server-Side Encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/serv-side-encryption.html)                                          |
| accelerator-s3-bucket-enforce-https                  | S3 buckets should require requests to use SSL/TLS    | [Enforce encryption of data in transit](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html#transit)                                     |
| accelerator-elb-logging-enabled                      | ELB access logging should be enabled.                | [Access logs for your ALB](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-access-logs.html)                                          |
|                                                      |