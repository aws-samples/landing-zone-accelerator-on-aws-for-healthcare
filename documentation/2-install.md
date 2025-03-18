# Landing Zone Accelerator on AWS for Healthcare - Installation

# 2. Deploy Landing Zone Accelerator using AWS Control Tower

## 2.1 Configure AWS Control Tower

**Note:** Starting with version v1.7.0 of Landing Zone Accelerator, Control Tower can be setup as part of the LZA installation by setting the appropriate parameters when deploying the CloudFormation stack in the next step.

You first need to [enable AWS Organizations in your home Region](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_org_create.html) by going to the [AWS Organizations console](https://console.aws.amazon.com/organizations/v2) and choosing **Create an organization**. If required, Organizations automatically sends a verification email to the address that is associated with your management account.

## 2.2 Deploy the installer CloudFormation stack

Click the **Launch Solution** button on [Step 1. Launch the stack](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/step-1.-launch-the-stack.html) page. **Ensure the Region is set to your desired home Region, as it typically defaults to US East (N. Virginia)**

Name the stack `AWSAccelerator-InstallerStack` and review the template’s parameters and enter or adjust the default values as needed. For example:

| Parameter                                         | Value                                                             |
| ------------------------------------------------- | ----------------------------------------------------------------- |
| **Manual Approval Stage notification email list** | Use the "Pipeline Approval" email defined in the prerequisites.   |
| **Management Account Email**                      | Use the "Management Account" email defined in the prerequisites.  |
| **Log Archive Account Email**                     | Use the "Log Archive Account" email defined in the prerequisites. |
| **Audit Account Email**                           | Use the "Security Account" email defined in the prerequisites.    |
| **Control Tower Environment**                     | Set this to "Yes"                                                 |
| **Configuration Repository Location**             | Set this to "S3" or "CodeCommit". \*\*See below\*\*               |

\*\*AWS CodeCommit is no longer available to new customers. Existing customers of AWS CodeCommit can continue to use the service as normal. If the management account is a new AWS account, you will need to select S3 as the configuration respository location.\*\*

Leave all other values as default, unless you have specific reasons to customize.

## 2.3 Wait for pipeline to complete

[Step 2. Await initial environment deployment](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/step-2.-await-initial-environment-deployment.html)

- Wait for the successful completion of the `AWSAccelerator-Pipeline` pipeline.

## 2.4 Verify Control Tower deployment and update configuration

After Control Tower deployment you should have a Security and Infrastructure OU as well as the three mandatory accounts.

### 2.4.1 Disable Control Tower VPC creation

Go to Control Tower Account Factory and edit the Network configuration

- Set the Maximum number of private subnets to 0
- Uncheck all regions for VPC creations (VPC creation will be handled by the accelerator)

## 2.5 Deploy the LZA for Healthcare reference architecture

Depending on the `Configuration Repository Location` parameter selected during the deployment, the Landing Zone Accelerator on AWS solution deploys either an AWS CodeCommit repository called `aws-accelerator-config` or an Amazon S3 bucket called `aws-accelerator-config-<ACCOUNT_ID>-<REGION>`. Each repository configuration location will contain six required and customizable YAML configuration files. The YAML files are pre-populated with a minimal configuration for the solution.

[Step 3. Update the configuration files](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/step-3.-update-the-configuration-files.html). The configuration files found in this repo's '[config](./config/)' should replace the files in the configuration repository location after adjusting environment specific configurations.

We recommend you read the LZA guidance on [using the configuration files](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/using-configuration-files.html), before continuing with the deployment of the reference architecture.

We recommend you go through every configuration file included in the Landing Zone Accelerator on AWS for Healthcare samples configs and confirm the default values correspond to your needs. Pay careful attention to any comments provided in the configuration files. To facilitate future updates of the reference configuration, we suggest you keep the same file structure and comment out parts that you don't need instead of removing them.

**Note:** Starting with version v1.7.0 of Landing Zone Accelerator, the LZA includes integration with AWS Control Tower baseline APIs and the proces of creating organizational units is automated when new OUs are included in the organizations-config.yaml file.

### 2.5.1 Mandatory customization

Using the IDE of your choice, in your local `aws-accelerator-config` repo, update the following values:

- replacements-config.yaml - This file contains global variables that can be referenced from all other configuration files. Review the value of each variable to confirm it is appropriate to your deployment.
- accounts-config.yaml - Update the config email addresses to match the email addresses you assigned in the prerequisites section.

#### 2.5.1a Changing the home Region

If you are changing the home region from _us-east-1_ to different region, you need to make the following configuration file modifications.

- global-config.yaml - **homeRegion: &HOME_REGION us-east-1** must be updated from _us-east-1_ to the region you are using as your home region, e.g. _homeRegion: &HOME_REGION us-west-2_
- networking-config.yaml - **homeRegion: &HOME_REGION us-east-1** must be updated from _us-east-1_ to the region you are using as your home region
- security-config.yaml - **homeRegion: &HOME_REGION us-east-1** must be updated from _us-east-1_ to the region you are using as your home region

#### 2.5.1.b Changing the accelerator prefix

If you changed the [accelerator prefix](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/step-1.-launch-the-stack.html) from **AWSAccelerator** during the LZA deployment, you need to make the following configuration file modifications.

- **dynamic-partitioning/log-filters.json** - update the acceleratorPrefix to _<custom prefix>_. For example if your prefix is _HCLS-Prod_, the config file should look like the following:

```
[
  { "logGroupPattern": "/HCLS-Prod-SecurityHub", "s3Prefix": "security-hub" }
]
```

### 2.5.2 Network customization

It is common for customers to want to assert control over their networking, based on existing on-premises requirements, such as CIDR ranges and the specific workload requirements, e.g. a VPN to integrate with on-premises services.

If you are deploying a demo environment for experimentation purposes, and don't need to perform any specific customization such as defining specific CIDR ranges that don't overlap with on-premises networks, you may wish to skip to the section on running the pipeline.

#### 2.5.2a Customizing the shared network

By default reference architecture deploys a fully working shared network, isolated between development, development and production environments. The following section describes how to modify the CIDR ranges for the shared networking if necessary.

The shared network makes use of a contiguous CIDR. The is currently specified as `10.0.0.0/8`. This is subdivided into

| CIDR range                | Purpose            | Notes                                                                                                                                             |
| ------------------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| 10.1.0.0/22               | Network-Endpoints  | This range is allocated to the Network-Endpoints VPC.                                                                                             |
| 10.2.0.0/22               | Network-Inspection | This range is allocated to the Network-Inspection VPC                                                                                             |
| 10.3.0.0/16               | EIS-Prod           | This range is allocated to the EIS-Prod VPC                                                                                                       |
| 10.4.0.0/16               | HIS-Dev            | This range is allocated to the HIS-Dev VPC                                                                                                        |
| 10.5.8.0 - 10.255.255.255 |                    | These are the remaining CIDRs from `10.0.0.0/8`. These are used used to checkout non-overlapping ranges for use with local Sandbox account VPC's. |

## 2.6 Running the pipeline

- Depending on the selected configuration repository location, complete either of the following:
  - Commit all of the changes to the `aws-accelerator-config` [AWS CodeCommit repository](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/step-3.-update-the-configuration-files.html#using-codecommit).
  - Compress the configuration files into a new archived file named `aws-accelerator-config.zip` and upload to [S3](https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/step-3.-update-the-configuration-files.html#using-s3).
- When finished editing the configuration files, navigate to the AWS CodePipeline console. Select `AWSAccelerator-Pipeline`, then Release change. This initiates a new pipeline instantiation and deploy the configuration changes to your environment.
- Wait for the successful completion of the `AWSAccelerator-Pipeline` pipeline.

# 3. Post deployment steps

After successful execution of the pipeline move to the [post deployment steps](3-post-deployment.md)
