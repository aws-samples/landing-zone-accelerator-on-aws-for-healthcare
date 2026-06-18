# Landing Zone Accelerator on AWS for Healthcare

> **⚠️ This repository is archived.** The configuration files in this repo are no longer actively maintained. See [Current Recommendations](#current-recommendations) below.

## What This Repository Was

This repository provided a sample set of configuration files for deploying the [Landing Zone Accelerator on AWS (LZA)](https://github.com/awslabs/landing-zone-accelerator-on-aws) with controls aligned to healthcare compliance frameworks, including HIPAA, NCSC, ENS High, C5, and Fascicolo Sanitario Elettronico.

The configuration established guardrails through Service Control Policies (SCPs), AWS Config rules, Security Hub standards, and network architecture patterns designed for organizations handling Protected Health Information (PHI).

## Current Recommendations

For new LZA deployments, the recommended starting point is the **Universal Configuration**:

| Resource | Purpose |
|----------|---------|
| [LZA Universal Configuration](https://github.com/aws/lza-universal-configuration) | Current recommended baseline configuration for LZA deployments |
| [LZA Universal Configuration Docs](https://github.com/aws/lza-universal-configuration/tree/main/docs) | Documentation and guidance for the universal config |

### HIPAA-Eligible Services SCP

This repository included a Service Control Policy that restricts usage to AWS services that are [HIPAA-eligible](https://aws.amazon.com/compliance/hipaa-eligible-services-reference/). That policy is now maintained in the AWS SCP examples repository:

➡️ [Service Control Policy Examples — Services in Scope for Compliance](https://github.com/aws-samples/service-control-policy-examples/tree/main/Services-in-scope-compliance)

Use the version in that repository rather than the copy in this repo's `archived-content/` directory.

## Archived Content

The original configuration files, documentation, and architecture diagrams have been preserved in the [`archived-content/`](archived-content/) directory for reference by existing users. This content is not actively maintained.

- [`archived-content/README.md`](archived-content/README.md) — Original README with full documentation
- [`archived-content/config/`](archived-content/config/) — LZA configuration YAML and policy files
- [`archived-content/documentation/`](archived-content/documentation/) — Installation and post-deployment guides
- [`archived-content/images/`](archived-content/images/) — Architecture and network diagrams

## License

This library is licensed under the MIT-0 License. See the [LICENSE](LICENSE) file.
