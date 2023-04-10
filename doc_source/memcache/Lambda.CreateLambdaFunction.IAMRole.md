# Step 2\.2: Create the IAM role \(execution role\)<a name="Lambda.CreateLambdaFunction.IAMRole"></a>

In this step, you create an AWS Identity and Access Management \(IAM\) role using the following predefined role type and access policy: 
+ AWS service role of the type **AWS Lambda** – This role grants AWS Lambda permissions to assume the role\.
+ **AWSLambdaVPCAccessExecutionRole** – This is the access permissions policy that you attach to the role\. The policy grants permission for the EC2 actions that AWS Lambda needs to manage ENIs\. You can view this AWS\-managed policy in IAM console\.

For more information about IAM user roles, see [Roles \(Delegation and Federation\)](https://docs.aws.amazon.com//IAM/latest/UserGuide/id_roles.html) in the *IAM User Guide*\.

Use the following procedure to create the IAM role\.

**To create an IAM \(execution\) role**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\. 

1. Choose **Roles** and then **Create role**\.
   + Under **Trusted entity type**, choose **AWS Service**, and then choose **Lambda** under **Common use cases**\. This grants the AWS Lambda service permissions to assume the role\. Choose **Next**\.
   + Under **Add permissions**, enter **lambda\-vpc\-execution\-role** and then choose **Next**\.
   + In **Role Name**, use a name that is unique within your AWS account \(for example, **lambda\-vpc\-execution\-role**\)\.
   + Choose **Create role**\.

1. Write down the role ARN\. You will need it in the next step when you create your Lambda function\.

**Next Step**

[Step 2\.3: Upload the deployment package \(create the Lambda function\)](Lambda.CreateLambdaFunction.Upload.md)