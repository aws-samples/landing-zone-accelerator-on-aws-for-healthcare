# Landing Zone Accelerator on AWS for Healthcare - Prerequisites

There are three parts to this guide:

1. Prerequisites - This includes gathering information, external dependencies for the initial deployment and the initial AWS Organizations management account setup.
2. Install - Initial deployment of LZA, deployment of healthcare reference architecture and customization.
3. Post Deployment - Considerations to continue development of landing zone.

# 1. Prerequisites

This reference architecture uses Landing Zone Accelerator on AWS (LZA) as its deployment engine. We therefore assume foundational knowledge of [Landing Zone Accelerator on AWS (LZA)](https://aws.amazon.com/solutions/implementations/landing-zone-accelerator-on-aws/). If you are not familiar with LZA we recommend you work through the [LZA immersion day](https://catalog.workshops.aws/landing-zone-accelerator/en-US) prior to deploying the reference architecture.

To support the LZA and reference architecture deployments, you will need to identify the following parameters and resources.

## 1.1 Identify your home AWS Region

The home AWS Region will be the Region in which you will most often operate in.

## 1.2 Email addresses

You will need to provision the following 6 unique email addresses for the accounts that the reference architecture will create.

| Account              | Purpose                                                                                                                                                                                                                                                                                                                                                                            | Example                         |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------- |
| Management Account   | This account is designated when first creating an AWS Organizations organization. It is a privileged account where all AWS Organizations global configuration management and billing consolidation occurs. The account will be used to create the other two mandatory accounts required by LZA.                                                                                    | lz-dev+management@example.com   |
| Audit Account        | This account is used to centralize all security operations and management activities. This account is typically used as a delegated administrator of centralized security services such as Amazon Macie, Amazon GuardDuty, and AWS Security Hub. NOTE: The LZA configuration files refer to this as the Audit account, but it serves the function of the Security Tooling account. | lz-dev+security@example.com     |
| Log Archive Account  | This account is used for centralized logging of AWS service logs.                                                                                                                                                                                                                                                                                                                  | lz-dev+log-archive@example.com  |
| Network Prod Account | This account is used for centralized networking that allows network administrators to govern shared networks and internally shared network services for the organization. For example, access to on-premises services via a VPN.                                                                                                                                                   | lz-dev+network-prod@example.com |
| HIS Dev Account      | This is an example account of how an account hierarchy can be designed. The networking-config.yaml includes a VPC deployment into this account. If you choose to update/edit this account, update the networking-config.yaml                                                                                                                                                       | lz-dev+his-dev@example.com      |
| EIS Prod Account     | This is an example account of how an account hierarchy can be designed. The networking-config.yaml includes a VPC deployment into this account. If you choose to update/edit this account, update the networking-config.                                                                                                                                                           | lz-dev+eis-prod@example.com     |

Additionally, you will need to allocate the following 5 email addresses used for operational notification purposes.

| Name                   | Purpose                                                                                               | Example email                             |
| ---------------------- | ----------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| High security events   | Events marked as CRITICAL or HIGH will be sent to this email address.                                 | lz-dev+security-alerts-high@example.com   |
| Medium security events | Events marked as MEDIUM will be sent to this email address.                                           | lz-dev+security-alerts-medium@example.com |
| Low security events    | Events marked as LOW will be sent to this email address.                                              | lz-dev+security-alerts-low@example.com    |
| AWS budgets alerts     | Any alerts related to the spending limits applied to the accounts will be sent to this email address. | lz-dev+budget-alerts@example.com          |
| Pipeline Approval      | An optional LZA pipeline approval stage may be enabled which requires an email for notification       | lz-dev+pipeline-approval@example.com      |

## 1.3 GitHub account

If you are using the GitHub source for the LZA code, you will need a GitHub account to generate a GitHub API key in a later step.

## 1.4 Reference architecture management account setup

You will need to create a new AWS Account for the reference architecture deployment. You can follow the [AWS guidance on setting up a new AWS account](https://aws.amazon.com/resources/create-account/).

### 1.4.1 Run EC2 instance in the management account

To run the installation in new accounts you need to pre-warm the account so that AWS automatically sets the correct quotas. To do this, launch small EC2 instance t3.medium for 15 minutes. Then terminate the instance and delete its security group.

### 1.4.2 Set security, operations, and billing contact information

Follow the guidance on [configuring account level contacts](https://docs.aws.amazon.com/prescriptive-guidance/latest/aws-startup-security-baseline/acct-01.html) to set the security and billing information. You can either use the email addresses you allocated above for "High security events", "LZ operators" and "AWS budgets alerts" or define additional contacts.

### 1.4.3 Verify email of the root user

After creating the account e-mail is sent to the root user with the request to verify the e-mail address. Confirm by clicking the link in the email.

### 1.4.4 Enable MFA on the root user and restrict access

We recommend that you use a hardware multi-factor authentication (MFA) device. Follow the guidance on [Require MFA to login](https://docs.aws.amazon.com/prescriptive-guidance/latest/aws-startup-security-baseline/acct-05.html) to configure MFA on the root user. Also review guidance on [restricting the use of the root user](https://docs.aws.amazon.com/prescriptive-guidance/latest/aws-startup-security-baseline/acct-02.html), only use the root user for the specific tasks that require it.

### 1.4.5 Create a local IAM deployment user account

- [Configure console access for each user](https://docs.aws.amazon.com/prescriptive-guidance/latest/aws-startup-security-baseline/acct-03.html) - you only need to follow the steps to create a local IAM user
- [Require MFA to login](https://docs.aws.amazon.com/prescriptive-guidance/latest/aws-startup-security-baseline/acct-05.html) - **Note:** You only need to configure the IAM user you created when restricting root user access, with MFA (In the post deployment steps you will configure your IdP where you can govern MFA access)

### 1.4.6 [optional] Increase the quota for AWS accounts

If your deployment will have more than 10 AWS, access the [service quota console for AWS Organizations](https://console.aws.amazon.com/servicequotas/home?region=us-east-1#!/services/organizations/quotas), to request an increase to the default maximum number of accounts.

### 1.4.7 Verify the quota for Lambda functions

In the Service quota console look for Service = Lambda and verify that quota on Concurrent executions = 1000. If it is smaller or 'not available' make sure you executed steps from point 1.4.1. If it did not help use the button 'Request increase at account-level' to request 1000. If it is not increased after 24 hours please contact [Support Center](https://console.aws.amazon.com/support/home).

### 1.4.8 Update AWS CodeBuild concurrency quota

Follow the prerequisite step to [update AWS CodeBuild concurrency quota](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/prerequisites#update-codebuild-conncurrency-quota).

### 1.4.9 Ensure your home AWS Region is accessible

Follow the prerequisite step to [ensure your global region is accessible](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/prerequisites#ensure-your-global-region-is-accessible).

### 1.4.10 Enable AWS Cost Explorer

Following the guidance on [enabling AWS Cost Explorer](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-enable.html).

### 1.4.11 Configure your GitHub token secret

If you are using the GitHub source for the LZA code, you will need to follow the prerequisite step to [store a github token in AWS Secrets Manager](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/prerequisites.html#create-a-github-personal-access-token-and-store-in-secrets-manager)

# 2. Deploy LZA

We recommend you first read the [LZA guidance on troubleshooting and known issues](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/troubleshooting.html) prior to running the installation.

Before deploying the Landing Zone Accelerator on AWS, you need to choose a method to centralize the management of resources provisioned by this solution. You can use either AWS Control Tower or AWS Organizations for the management capabilities.

Use the following steps to deploy the LZA solution using AWS Control Tower.

- [Install LZA using AWS Control Tower](2-install.md)
