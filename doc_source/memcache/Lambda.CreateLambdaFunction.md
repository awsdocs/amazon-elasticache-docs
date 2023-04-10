# Step 2: Create a Lambda function<a name="Lambda.CreateLambdaFunction"></a>

In this step you do the following:

1. Create a Lambda function deployment package using the sample code provided\.

1. Create an IAM role \(execution role\)\. At the time you upload the deployment package, you need to specify this role so that Lambda can assume the role and then execute the function on your behalf\. The permissions policy grants AWS Lambda permissions to set up elastic network interfaces or ENIs to enable your Lambda function to access resources in the VPC\. In this example, your Lambda function accesses an ElastiCache cluster in the VPC\.

1. Create the Lambda function by uploading the deployment package\.

**Next Step**

[Step 2\.1: Create the deployment package](Lambda.CreateLambdaFunction.DeploymentPkg.md)