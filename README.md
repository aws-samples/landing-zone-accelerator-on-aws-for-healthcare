# Landing Zone Accelerator on AWS for Healthcare

## Table of Contents
1. [Overview](#overview)
2. [Security Frameworks](#security-frameworks)
3. [Security Controls](#security-controls)
4. [Organizational Structure](#organizational-structure)
5. [Architecture Diagrams](#architecture-diagrams)
6. [Cost](#cost)
7. [Installation Guide](#installation-guide)
8. [Further Considerations](#further-considerations)
9. [References](#references)
10. [Contributing](#contributing)
11. [License](#license)
12. [Release Version History](#release-version-history)

## Overview

The **Landing Zone Accelerator (LZA) for Healthcare** is an industry specific deployment of the [Landing Zone Accelerator on AWS](https://aws.amazon.com/solutions/implementations/landing-zone-accelerator-on-aws/) solution architected to align with AWS best practices and in conformance with multiple, global compliance frameworks. Built on top of the standard AWS Control Tower accounts, namely `Management`, `Audit`, and `LogArchive`, the LZA for Healthcare deploys additional resources that helps establish platform readiness with security, compliance, and operational capabilities. It is important to note that the Landing Zone Accelerator solution will not, by itself, make you compliant. It provides the foundational infrastructure from which additional complementary solutions can be integrated. You must review, evaluate, assess, and approve the solution in compliance with your organization’s particular security features, tools, and configurations.

## Security Frameworks

The healthcare industry is highly regulated. The LZA for Healthcare provides additional guardrails to help mitigate against the threats faced by healthcare customers. The LZA for Healthcare is not meant to be feature complete for fully compliant, but rather is intended to help accelerate cloud migrations and cloud refactoring efforts by organizations serving the healthcare industry. While much effort has been made to reduce the effort required to manually build a production-ready infrastructure, you will still need to tailor it to your unique business needs.

This solution includes controls from frameworks in various geographies, including HIPAA, NCSC, ENS High, C5, and Fascicolo Sanitario Elettronico. If you are deploying the Landing Zone Accelerator on AWS for Healthcare solution, please consult with your AWS team to understand controls to meet your requirements.

## Security Controls

These controls are created as detective or preventative guardrails in the AWS environment through AWS Config rules or through Service Control Policies (SCPs). Within the file `organization-config.yaml` are sections for declaring SCPs, Tagging Policies, and Backup Policies. SCPs can be highly specific to the organization and its workload(s) and should be reviewed and modified to meet your specific requirements. Sample policies have been provided for the following:

- `Service Control Policies`: A service control policy has been provided in `service-control-policies/scp-hlc-base-root.json` that prevents accounts from leaving your organization or disabling block public access. The `service-control-policies/scp-hlc-hipaa-service.json` policy is an example of a policy that can be used to ensure only HIPAA eligible services can be used in a specific OU or account. It is important to note that SCPs are not automatically updated and that changes to the HIPAA eligible service list will be to be updated. However, this is an example of how your organization can ensure that a select list of AWS services are used for specific use cases.
- `Tagging Policies`: A sample tagging policy has been provided in `tagging-policies/org-tag-policy.json` to show how a `Cost Center` tag can be defined across and AWS Organization. An additional tagging policy, `tagging-policies/healthcare-org-tag-policy.json`, shows how you can further extend these policies to define `Environment Type` for `Prod`, `QA`, and `Dev` workloads, `Data Classification` to track sensitive and non-sentive workloads such as `PHI` and `Company Confidental` and how to enforce them to specific AWS services. The sample policy should be edited to reflect your own organization's cost centers so that resources provisioned by the LZA are automatically tagged in accordance with your business requirements.
- `Backup policies`: A sample backup policy has been provided in `backup-policies/org-backup-policies.json` as an example for how backups can scheduled along with lifecycle and retention management settings.

In the `security-config.yaml` file, AWS security services can be configured such as AWS Config, AWS Security Hub, and enabling storage encryption. Additional alarms and metrics have been provided to inform you of actions within your AWS Cloud environment. For a list of all of the services and settings that can be configured, see the [LZA on AWS Implementation Guide](#references) in the references section below. This file also contains the AWS Config rules that make up the list of detective guardrails used to meet many of the controls from the various frameworks. These rules are implemented through a combination of AWS services:

- AWS Security Hub
  - AWS Foundational Security Best Practices v1.0.0
  - CIS AWS Foundations Benchmark v1.4.0
  - NIST Special Publication 800-53 Revision 5
- AWS Config
  - Rules included in the [Operational Best Practices for HIPAA Security](https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-hipaa_security.html) sample conformance pack.
- AWS Audit Manager
  - Rules included in the [HIPAA Security Rule: Feb 2003](https://docs.aws.amazon.com/audit-manager/latest/userguide/HIPAA.html) and [HIPAA Omnibus Final Rule](https://docs.aws.amazon.com/audit-manager/latest/userguide/HIPAA-omnibus-rule.html) prebuilt standard frameworks.

The `global-config.yaml` file contains the settings that enable regions, centralized logging using AWS CloudTrail and Amazon CloudWatch Logs and the retention period for those logs to help you meet your specific auditing and monitoring needs.

You are encouraged to review these settings to better understand what has already been configured and what needs to be altered for your specific requirements.

## Organizational Structure

Healthcare LZA accounts are generated and organized as follows:

<!--
```sh
+-- Root
|   +-- Management
|   +-- Security
|       +-- LogArchive
|       +-- Audit
|   +-- Infrastructure
|       +-- Infra-Prod
|       +-- Infra-Dev
|   +-- HIS
|       +-- HIS-Non-Prod
|   +-- EIS
|       +-- EIS-Prod
```
-->

![Healthcare LZA Org Structure](./images/LZA_Healthcare_Org_Structure.png)

The Health Information System (HIS) Organizational Unit (OU) represents the logical construct where workloads that contain sensitive data, such as critical business or Personal Health Information (PHI). Whereas, the Executive Information System (EIS) OU is intended for business workloads that may not require the same regulatory controls. This OU structure is provided for you. However, you are free to change the organizational structure, Organizational Units (OUs), and accounts to meet your specific needs. For additional information about how to best organize your AWS OU and account structure, please referece the Recommended OUs and accounts in the [For further consideration](#for-further-consideration) section below as you begin to experiment with the LZA for Healthcare.

## Architecture Diagrams

AWS LZA for Healthcare Organizational Structure
![Healthcare LZA Architecture](./images/LZA_Healthcare_Summary_Diagram.png)

By default, the LZA for Healthcare builds the above organizational structure, with the exception of the `Management` and `Security` OU, which are predefined by you prior to launching the LZA. The below architecture diagram highlights the key deployments:

- An `Infrastructure` OU
  - Contains `Infra-Dev` OU and `Infra-Prod` OU under `Infrastructure` OU
  - Contains `Network-Prod` account under `Infra-Prod` OU
  - `Network-Prod` account contains a Transit Gateway for infrastructure routing
  - `Network-Prod` contains `Network-Endpoints` VPC and `Network_Inspection` VPC in `us-east-1`
  - Each VPC uses a /22 CIDR block in the 10.0.0.0/8 RFC-1918 range
- A `HIS` OU
  - Contains `HIS-Non-Prod` OU under `HIS` OU
  - Contains `HIS-Dev` account under `HIS-Non-Prod OU`
  - Contains a single VPC in `us-east-1`
  - VPC uses a /16 CIDR block in the 10.0.0.0/8 RFC-1918 range
- A `EIS` OU
  - Contains `EIS-Prod` OU under `HIS` OU
  - Contains `EIS-Prod` account under `EIS-Prod OU`
  - Contains a single VPC in `us-east-1`
  - VPC uses a /16 CIDR block in the 10.0.0.0/8 RFC-1918 range

AWS LZA for Healthcare Network Diagram  
![Healthcare LZA Network Diagram](./images/LZA_Healthcare_Network_Diagram_v3.1.png)

The accounts in the `EIS` and `HIS` OUs represent a standard infrastructure for development or production deployment of your workloads.

The `Infrastructure` OU provides the following specialized functions:

- The `Network-Prod` account contains an `Network-Inspection` VPC for inspecting AWS traffic as well as routing traffic to and from the Internet. If a route table is defined, for example `Network-Main-Segregated`, traffic will flow from the `HIS-Dev` VPC through the `Network-Main` Transit Gateway, where it will can be inspected\* before being blackholed or continuing to the internet or its final destination.
- The `Network` account also contains an `Network-Endpoints` VPC, intended to host centralized VPC interface endpoints; all the spoke VPCs will use these centralized endpoints via Transit Gateway. This central location and corresponding route tables allow you to efficiently design your network and compartmentalize access control accordingly.

**\*Note:** The network-configs.yaml does not provision a network inspection solution for inspecting traffic. Additional configuration is required to deploy a networking inspection appliance or consider using [AWS Network Firewall](https://aws.amazon.com/network-firewall/) along with updating route tables.

## Cost

You are responsible for the cost of the AWS services used while running this solution. As of September 2022, the cost for running this solution using the Landing Zone Accelerator with the healthcare configuration files and AWS Control Tower in the US East (N. Virginia) Region within a test environment with no active workloads is between $400-$500 USD per month. As additional AWS services and workloads are deployed, the cost will increase.

| AWS Service                                      | Cost per month |
| ------------------------------------------------ | -------------- |
| AWS CloudTrail                                   | $4.30          |
| Amazon CloudWatch                                | $8.75          |
| Amazon Config                                    | $35.55         |
| Amazon GuardDuty                                 | $5.75          |
| Amazon AWS Key Management Services (AWS KMS)     | $15.65         |
| Amazon Amazon Route 53                           | $2.00          |
| Amazon Simple Storage Service (Amazon S3)        | $1.48          |
| Amazon Virtual Private Cloud (Amazon VPC)        | $301.56        |
| Amazon AWS Security Hub                          | $44.32         |
| Amazon Secrets Manager                           | $0.48          |
| Amazon Simple Notification Services (Amazon SNS) | $0.42          |
| Total monthly cost                               | $420.26        |

## Installation Guide

To review a general overall summary of the LZA for Healthcare configuration files, see [Config Summary](documentation/config-summary.md).

There are three parts to this guide:

1. [Prerequisites](documentation/1-prereqs.md) - This includes gathering information, external dependencies for the initial deployment and the initial AWS Organizations management account setup.
2. [Install](documentation/2-install.md) - Initial deployment of LZA, deployment of healthcare reference architecture and customization.
3. [Post Deployment](documentation/3-post-deployment.md) - Considerations to continue development of landing zone.

## Further considerations

Although the Healthcare LZA aims to be prescriptive in applying best practices for Healthcare customers, it intentionally avoids being _overly prescriptive_ out of deference to the unique realities for each individual organization. Consider the baseline Healthcare LZA as a good starting point, but bear in mind your objectives as you begin to tailor it for your specific business requirements. From this perspective AWS provides resources that you should consult as you begin customizing your deployment of the Healthcare LZA:

1. This set of configuration files was tested with AWS Control Tower verions 3.3. Starting with AWS Control Tower version 3.0, Control Tower supports the use of an AWS CloudTrail Organization Trail. The global-config.yaml file shows organizationTail set to false because it is enabled through the AWS Control Tower setup.
2. Refer to the [Best Practices] for Organizational Units with AWS Organizations blog post for an overview.
3. [Recommended OUs and accounts]. This section of the `Organizing your AWS Environment Using Multiple Accounts` Whitepaper discusses the deployment of specific-purpose OUs in addition to the foundational ones established by the LZA. For example, you may wish to establish a `Sandbox` OU for experimentation, a `Policy Staging` OU to safely test policy changes before deploying them more broadly, or a `Suspended` OU to hold, constrain, and eventually retire accounts that you no longer need.
4. [AWS Security Reference Architecture] (SRA). The SRA "is a holistic set of guidelines for deploying the full complement of AWS security services in a multi-account environment." This document is aimed at helping you to explore the "big picture" of AWS security and security-related services in order to determine the architectures most suited to your organization's unique security requirements.
5. Transite Gateway Flow logs are not enabled by default, work with your AWS team to determine if enabling TGW Flow logs help you meet your regulatory and organizational requirements.

## References

- LZA on AWS [Implementation Guide]. This is the official documenation of the Landing Zone Accelerator Project and serves as your starting point. Use the instructions in the implementation guide to stand up your environment and then return to this project for Healthcare-specific customization.
- AWS Labs [LZA Accelerator] GitHub Repository. The official codebase of the Landing Zone Accelerator Project.

<!-- Hyperlinks -->

[Best Practices]: https://aws.amazon.com/blogs/mt/best-practices-for-organizational-units-with-aws-organizations/
[Recommended OUs and accounts]: https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/recommended-ous-and-accounts.html
[AWS Security Reference Architecture]: https://docs.aws.amazon.com/prescriptive-guidance/latest/security-reference-architecture/welcome.html
[Implementation Guide]: https://docs.aws.amazon.com/solutions/latest/landing-zone-accelerator-on-aws/landing-zone-accelerator-on-aws.pdf
[LZA Accelerator]: https://github.com/awslabs/landing-zone-accelerator-on-aws
[Operational Best Practices for HIPAA Security]: https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-hipaa_security.html
[VPC Sharing: key considerations and best practices]: https://aws.amazon.com/blogs/networking-and-content-delivery/vpc-sharing-key-considerations-and-best-practices/

## Contributing

See [CONTRIBUTING](CONTRIBUTING.md) for more information.

## License

This library is licensed under the MIT-0 License. See the [LICENSE](LICENSE) file.

## Release Version History
### v1.9.0-a - 10/31/2024
- Changes: This release had been tested using LZA v1.9.0
- Bug fixes: None

### v1.9.0-b - 3/16/2025
- Changes: 
  - Updates to this README.md to reflect reduced configuration cost - as inspection networking is not included by default.
  - Updates to 1-prereqs.md and 2-install.md with additional guidance about home_region setting.
  - Updates to config-summary.md to reflect config rules implemented.
  - Updates config/service-control-policies/scp-hcl-hipaa-service.json and config/service-control-policies/Readme.md to reflect HIPAA Eligible Services list changes as of 12/2024.
- Bug fixes: None

### v1.9.0-c - 5/17/2025
- Changes: 
  - 2/2025
    - Changes SCP Readme to reflect SageMaker service name change.
    - "Amazon SageMaker [excludes Studio Lab, Ground Truth Plus, Public Workforce and Vendor Workforce for all features]" --> "Amazon SageMaker AI [formerly Amazon Sagemaker, excludes Studio Lab, Ground Truth Plus, Public Workforce and Vendor Workforce for all features]"
  - 3/2025
    - Added "AWS Fargate [ECS and EKS engines only]" . 
    - Added additional OpenSearch and SSM APIs.
    - Added opensearch ingestion and opensearch serverless (apis osis:* and aoss:*).
    - Added sms quicksetup (api ssm-quickstetup:*).
  - Add Tools Monitoring / Telemetry. 
        - APIs pi:* and applicationinsights:*
  - Update to reflect 4/16/2025 HIPAA Eligible Services List 
- Bug fixes: None

### v1.9.0-d - 6/17/2025
- Changes: 
  - 6/2025
    - Renames AWS Security Hub to AWS Security Hub CSPM (formerly AWS Security Hub)
    - Adds tool API qdeveloper:*
    - Fixes type in main README "high regulated" to "highly regulated"
- Bug fixes: None
