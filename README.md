![image](https://github.com/johnadewumi1/Remediate_Public_S3/blob/main/images/Architecture_Diagram.png.png)
# Project Statement
Your Senior Management is concerned about engineers creating public accessible S3 buckets. They want you to track situations like this and if you find any, they want to remediate them. After the remediation is complete, they want you to send a notification to the security department about which buckets were remediated.
You determine what your Lambda function should do.
Your solution should:


1. Detect any public accessible S3 Buckets (Hint: Use AWS Config)

2. Remediate the bucket(s) and make private.

3. Send an SNS notification of which bucket(s) were remediated.(use Eventbridge)

Note: AWS Config is not a free service. Be sure to delete your solutions after completion.


## Project Solution Ideas:
- Create aws config with rule checking for aws s3 bucket with public access in the same region with a cloudformation template (blockaccess.yml) that consist
  - The RemediationConfiguration to remediate the buckets
  - The sns topic for update of  s3 bucket to be compliant
- Create a Eventbridge to trigger sns when nocompliance bucket are created
- Create S3 bucket that has public access with cloudfomation template (s3_cfn) attached.


## Thing to note:
config needs to:
    - Check for non-compliant s3 buckets
    - Remediate the s3 bucket by making it private

If you haven't already created an Amazon SNS topic, then follow the instructions for Getting started with Amazon SNS. https://docs.aws.amazon.com/sns/latest/dg/sns-getting-started.html.

Note: The Amazon SNS topic must be in the same Region as your AWS Config service.

## Amazon EventBridge with SNS Topic setup.

1. Choose Create rule
2. In Define Pattern, choose Event pattern.
3. In Event Matching pattern, choose Custom pattern.
4. In the Event pattern preview pane, copy and paste the following example event pattern: use Event_pattern.json template

![image](https://github.com/johnadewumi1/Remediate_Public_S3/blob/main/images/event_pattern.png)
  
  The above is used to trigger the topic form event bridge wehenever we have a non compliant bucket 
  
 5. Choose Save.
 6. In Select targets, choose SNS topic.
 7. In Topic, choose your SNS topic created with the cloudformation.
 8. Expand Configure input, and then choose Input transformer.
 9.  In the Input Path text box, copy and paste the following example path:
  
 - {
    "awsRegion": "$.detail.awsRegion",
    "resourceId": "$.detail.resourceId",
    "awsAccountId": "$.detail.awsAccountId",
    "compliance": "$.detail.newEvaluationResult.complianceType",
    "rule": "$.detail.configRuleName",
    "time": "$.detail.newEvaluationResult.resultRecordedTime",
    "resourceType": "$.detail.resourceType"
}

10.  In the Input Template text box, copy and paste the following template:

- "On <time> AWS Config rule <rule> evaluated the <resourceType> with Id <resourceId> in the account <awsAccountId> region <awsRegion> as <compliance> For more details open the AWS Config console at https://console.aws.amazon.com/config/home?region=<awsRegion>#/timeline/<resourceType>/<resourceId>/configuration"

  
  
 11. Choose Create.
  
 12.  When an event type triggers, you receive an SNS email notification with the custom fields populated from step 11 similar to the following:
  
![image](https://user-images.githubusercontent.com/63524577/140666171-dad79836-6ccd-427a-bdaf-77eb82132d16.png)

