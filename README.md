# Remediate_Public_S3
# Project Statement
Your Senior Management is concerned about engineers creating public accessible S3 buckets. They want you to track situations like this and if you find any, they want to remediate them using Lambda. After the remediation is complete, they want you to send a notification to the security department about which buckets were remediated.
You determine what your Lambda function should do.
Your solution should:


1. Detect any public accessible S3 Buckets (Hint: Use AWS Config)

1. Remediate the bucket(s) and make private.

1. Send an SNS notification of which bucket(s) were remediated.(use Eventbridge)

Note: AWS Config is not a free service. Be sure to delete your solutions after completion.


## Project Solution Ideas:
- Create S3 bucket that has public access with cloudfomation template (s3_cfn) attached.
- Create aws config with rule checking for aws s3 bucket with public access in the same region with a cloudformation template that consist
  - the remediation remediation to remediate the buckets
  - the sns topic for update of  s3 bucket to be compliant
- Create a Eventbridge to trigger sns when nocompliance bucket are created


## Thing to note:
Lambda function needs access to:
    - Config to check for non-compliant s3 buckets
    - access to make changes to s3 bucket
    - access to write to sns 


## Research about boto3.
- s3:
  - Delete public_access_to_a_bucket [here](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/s3.html#S3.Client.delete_public_access_block)
    - response = client.delete_public_access_block(
      Bucket='string',
      ExpectedBucketOwner='string'
      )
  - permission lambda needed : s3:PutBucketPublicAccessBlock
- Config
  - get compliance details by config rule [here](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/config.html#ConfigService.Client.get_compliance_details_by_config_rule)
    - response = client.get_compliance_details_by_config_rule(
      ConfigRuleName='string',
      ComplianceTypes=[
      'COMPLIANT'|'NON_COMPLIANT'|'NOT_APPLICABLE'|'INSUFFICIENT_DATA',
      ],
      Limit=123,
      NextToken='string'
      )
- SNS
  - create sns topic
  - create sns subscription that uses that topic
