# Sample AWS Landing Zone Customizations

This repo contains sample Service Control Policies (SCPs) and CloudFormation templates for customizing a Landing Zone in AWS Control Tower as part of the [Customizations for AWS Control Tower](https://aws.amazon.com/solutions/implementations/customizations-for-aws-control-tower/) solution by AWS.

The SCPs, CloudFormation templates, and a [manifest file](manifest.yaml) will be bundled as a Configuration Package in a zip file for deployment to the Landing Zone in Control Tower. To deploy the Configuration File, the zip file will be uploaded to an S3 bucket created as part of the Customizations for AWS Control Tower solution. The AWS CodePipeline will then deploy SCPs and CloudFormation StackSets to appropriate member AWS Accounts.

The following diagram shows architecture of the Customizations for AWS Control Tower solution

![Customizations for AWS Control Tower solution](https://d1.awsstatic.com/aws-answers/answers-images/customizations-for-aws-control-tower-architecture-diagram.00c3c6e503a5254fe8990d5d952f3bcf3d60d077.png)

# Prerequisites

* AWS CLI
* AWS Credentials (Access Key/Secret Access Key)

# Building

To build the Configuration Package, a zip file will need to be created that contains the manifest file, the policy files for SCPs, and CloudFormation templates stored in the `custom-control-tower-configuration` folder in this repo.

:exclamation: Zip file must be named `custom-control-tower-configuration.zip` in order to trigger the AWS CodePipeline in Customizations for AWS Control Tower solution.

To build the zip file:

```bash
mkdir dist
zip -r dist/custom-control-tower-configuration.zip custom-control-tower-configuration/
```

# Deploying

To deploy the Configuration Package to the Customizations for AWS Control Tower solution, the zip file will need to be copied to the `custom-control-tower-configuration-${AWS::AccountId}-${AWS::Region}` S3 bucket.

```bash
aws s3 cp custom-control-tower-configuration/custom-control-tower-configuration.zip s3://CUSTOM_CONTROL_TOWER_CONFIGURATION_BUCKET_NAME/custom-control-tower-configuration.zip
```

# CI/CD

GitHub Actions can be used to build the Configuration Package and deploy it to the Customizations for AWS Control Tower solution.

To configure, modify [](.github/workflows/main.yml):

1. Set `AWS_REGION` variable to the region where the Customizations for AWS Control Tower solution is deployed, default: `us-east-1`
2. Set `CUSTOM_CONTROL_TOWER_CONFIGURATION_BUCKET_NAME` to the bucket created by solution to store Configuration Packages, default: `custom-control-tower-configuration-${AWS::AccountId}-${AWS::Region}`

Set the following secrets in your GitHub repo:

1. AWS_ACCESS_KEY_ID
2. AWS_SECRET_ACCESS_KEY
3. AWS_ACCOUNT_ID
