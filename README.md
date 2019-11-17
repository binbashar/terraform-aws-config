Enables AWS Config and adds managed config rules with good defaults.

The following AWS Config Rules are supported:

* acm-certificate-expiration-check: Ensure ACM Certificates in your account are marked for expiration within the specified number of days.
* cloudtrail-enabled: Ensure CloudTrail is enabled.
* ec2-volume-inuse-check: Checks whether EBS volumes are attached to EC2 instances
* guardduty-enabled-centralized: Checks whether Amazon GuardDuty is enabled in your AWS account and region.
* iam-password-policy: Ensure the account password policy for IAM users meets the specified requirements.
* iam-user-no-policies-check: Ensure that none of your IAM users have policies attached; IAM users must inherit permissions from IAM groups or roles.
* instances-in-vpc: Ensure all EC2 instances run in a VPC.
* root-account-mfa-enabled: Ensure root AWS account has MFA enabled.
* rds-storage-encrypted: Checks whether storage encryption is enabled for your RDS DB instances.
* s3-bucket-public-write-prohibited: Checks that your S3 buckets do not allow public write access.

## Terraform Versions

Terraform 0.12. Pin module version to ~> 2.x Submit pull-requests to master branch.

Terraform 0.11. Pin module version to ~> 1.5.1. Submit pull-requests to terraform011 branch.

## Usage

**Note: This module sets up AWS IAM Roles and Policies, which are globally namespaced. If you plan to have multiple instances of AWS Config, make sure they have unique values for `config_name`.**

```hcl
module "aws_config" {
  source = "trussworks/config/aws"

  config_name        = "my-aws-config"
  config_logs_bucket = "my-aws-logs"
}
```

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| config\_logs\_bucket | The S3 bucket for AWS Config logs. | string | n/a | yes |
| acm\_days\_to\_expiration | Specify the number of days before the rule flags the ACM Certificate as noncompliant. | string | `"14"` | no |
| aggregate\_organization | Aggregate compliance data by organization | string | `"false"` | no |
| ami\_required\_tag\_key\_value | Tag key and value which AMI has to have to be compliant: Example: tag-key:tag-value | string | `""` | no |
| check\_acm\_certificate\_expiration\_check | Enable acm-certificate-expiration-check | string | `"true"` | no |
| check\_approved\_amis\_by\_tag | Enable approved-amis-by-tag | string | `"false"` | no |
| check\_cloud\_trail\_encryption | Enable cloud-trail-encryption-enabled rule | string | `"false"` | no |
| check\_cloud\_trail\_log\_file\_validation | Enable cloud-trail-log-file-validation-enabled rule | string | `"false"` | no |
| check\_ec2\_encrypted\_volumes | Enable ec2-encrypted-volumes | string | `"false"` | no |
| check\_ec2\_volume\_inuse\_check | Enable ec2-volume-inuse-check | string | `"true"` | no |
| check\_eip\_attached | Enable eip-attached rule | string | `"false"` | no |
| check\_guard\_duty | Enable guardduty-enabled-centralized rule | string | `"false"` | no |
| check\_iam\_group\_has\_users\_check | Enable iam-group-has-users-check | string | `"true"` | no |
| check\_instances\_in\_vpc | Enable instances-in-vpc | string | `"true"` | no |
| check\_multi\_region\_cloud\_trail | Enable multi-region-cloud-trail-enabled rule | string | `"false"` | no |
| check\_rds\_public\_access | Enable rds-instance-public-access-check rule | string | `"false"` | no |
| check\_rds\_storage\_encrypted | Enable rds-storage-encrypted | string | `"false"` | no |
| check\_required\_tags | Enable required-tags rule | string | `"false"` | no |
| config\_aggregator\_name | The name of the aggregator. | string | `"organization"` | no |
| config\_delivery\_frequency | The frequency with which AWS Config delivers configuration snapshots. | string | `"Six_Hours"` | no |
| config\_logs\_prefix | The S3 prefix for AWS Config logs. | string | `"config"` | no |
| config\_max\_execution\_frequency | The maximum frequency with which AWS Config runs evaluations for a rule. | string | `"TwentyFour_Hours"` | no |
| config\_name | The name of the AWS Config instance. | string | `"aws-config"` | no |
| password\_max\_age | Number of days before password expiration. | string | `"90"` | no |
| password\_min\_length | Password minimum length. | string | `"14"` | no |
| password\_require\_lowercase | Require at least one lowercase character in password. | string | `"true"` | no |
| password\_require\_numbers | Require at least one number in password. | string | `"true"` | no |
| password\_require\_symbols | Require at least one symbol in password. | string | `"true"` | no |
| password\_require\_uppercase | Require at least one uppercase character in password. | string | `"true"` | no |
| password\_reuse\_prevention | Number of passwords before allowing reuse. | string | `"24"` | no |
| required\_tags | A map of required resource tags. Format is tagNKey, tagNValue, where N is int. Values are optional. | map(string) | `{}` | no |
| required\_tags\_resource\_types | Resource types to check for tags. | list(string) | `[]` | no |

<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->

## Developer Setup

Install dependencies (macOS)

```shell
brew install pre-commit go terraform terraform-docs
```

### Testing

[Terratest](https://github.com/gruntwork-io/terratest) is being used for
automated testing with this module. Tests in the `test` folder can be run
locally by running the following command:

```text
make test
```

Or with aws-vault:

```text
AWS_VAULT_KEYCHAIN_NAME=<NAME> aws-vault exec <PROFILE> -- make test
```
