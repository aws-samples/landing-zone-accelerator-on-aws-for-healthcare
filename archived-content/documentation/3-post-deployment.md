# Landing Zone Accelerator on AWS for Healthcare - Post Deployment

# 3. Post Deployment Steps

## 3.1 Access AWS IAM Identity Center and configure your identity source

### 3.1.1 Delegate administration of Identity Center

By default, the Identity Center instance created by Control Tower is in the management account of the AWS Organizations. You can choose to delegate administration of IAM Identity Center to a member account in AWS Organizations, thereby extending the ability to manage IAM Identity Center from outside the management account.

1. Update the iam-configs.yaml to include the `identityCenter` class along with `name` and `delegatedAdminAccount` properties. Refer to LZA [documentation](https://awslabs.github.io/landing-zone-accelerator-on-aws/latest/typedocs/latest/classes/_aws_accelerator_config.IdentityCenterConfig.html) for guidance. Do not include `identityCenterPermissionSets` or `identityCenterAssignments` at this point.
2. For the delegated administrator account, you can using an existing account already configured in your accounts-config.yaml or add a new `workloadAccounts` account.
3. Follow previously mentioned steps to move the updated configuration files to the configuration repository location and release the `AWSAccelerator-Pipeline` pipeline.
4. Wait for the successful completion of the `AWSAccelerator-Pipeline` pipeline.

### 3.1.2 Configure identity source (optional)

When you enable IAM Identity Center for the first time, it's automatically configured with an Identity Center directory as your default identity source unless you choose a different identity source.

1. Log into the administration account for AWS IAM Identity Center.
2. If you plan to use the AWS Directory that the reference architecture deploys follow the [IAM Identity centre guidance to configure AD](https://docs.aws.amazon.com/singlesignon/latest/userguide/connectawsad.html). If you plan to use an external IDP follow the [IAM Identity centre guidance to configure an external identity provider](https://docs.aws.amazon.com/singlesignon/latest/userguide/manage-your-identity-source-idp.html).

### 3.1.3 Identity Center permission sets and assignments

Once delegated adminstration for Identity Center and the identity source configurations are complete, permission sets can be configured and assigned to users/groups for accounts in the AWS Organizations.

1. Update the iam-configs.yaml to include any required `identityCenterPermissionSets` and `identityCenterAssignments`
2. Follow previously mentioned steps to move the updated configuration files to the configuration repository location and release the `AWSAccelerator-Pipeline` pipeline.
3. Wait for the successful completion of the `AWSAccelerator-Pipeline` pipeline.

## 3.2 Configure Multi-factor authentication for IAM Identity Center:

Follow guidance on [Multi-factor authentication for Identity Center users](https://docs.aws.amazon.com/singlesignon/latest/userguide/enable-mfa.html).

We recommend the following minimum settings:

- Every time they sign in (always-on)
- Security key and built-in authenticators
- Authenticator apps
- Require them to provide a one-time password sent by email to sign in
- Users can add and manage their own MFA devices

## 3.3 Configure MFA for the breakglass users

The breakglass users are highly privileged user accounts.

Login to the management and follow the [AWS IAM documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable.html) to configure MFA on both breakglass accounts. We recommend that you use hardware MFA for these accounts.

## 3.4 Enable security controls in every default region

During the initial installation only the home region was enabled. We highly recommend guardrail deployment for all AWS regions that are enabled by default. For more details see the [FAQ about AWS Regions](./documentation/FAQ.md#aws-regions).

Edit the `global-config.yaml` file and add all default regions under the `enabledRegions` property. Commit and push your change to your CodeCommit repository and release the Accelerator pipeline.

```
enabledRegions:
  - *HOME_REGION
  - "ap-northeast-1"
  - "ap-northeast-2"
  - "ap-northeast-3"
  - "ap-south-1"
  - "ap-southeast-1"
  - "ap-southeast-2"
  - "eu-central-1"
  - "eu-north-1"
  - "eu-west-1"
  - "eu-west-2"
  - "eu-west-3"
  - "sa-east-1"
  - "us-east-2"
  - "us-west-1"
  - "us-west-2"
```
