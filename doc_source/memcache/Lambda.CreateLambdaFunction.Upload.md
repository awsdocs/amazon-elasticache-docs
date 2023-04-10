# Step 2\.3: Upload the deployment package \(create the Lambda function\)<a name="Lambda.CreateLambdaFunction.Upload"></a>

In this step, you create the Lambda function \(`AccessMemCached`\) using the `create-function` AWS CLI command\. 

At the command prompt, run the following Lambda CLI `create-function` command using the **adminuser** profile\.

You need to update the following `create-function` command by providing the \.zip file path and the execution role ARN\. The `--runtime` parameter value can be `python2.7`, `nodejs`, and `java8`, depending on the language you used to author your code\.

For Linux, macOS, or Unix:

```
$ aws lambda create-function \
--function-name AccessMemCached  \
--region us-east-1 \
--zip-file fileb://path-to/app.zip \
--role execution-role-arn \
--handler app.handler \
--runtime python3.8  \
--timeout 30 \
--vpc-config SubnetIds=comma-separated-vpc-subnet-ids,SecurityGroupIds=default-security-group-id \
--memory-size 1024
```

For Windows:

```
$ aws lambda create-function ^
--function-name AccessMemCached  ^
--region us-east-1 ^
--zip-file fileb://path-to/app.zip ^
--role execution-role-arn ^
--handler app.handler ^
--runtime python3.8  ^
--timeout 30 ^
--vpc-config SubnetIds=comma-separated-vpc-subnet-ids,SecurityGroupIds=default-security-group-id ^
--memory-size 1024
```

You can find the subnet IDs and the default security group ID of your VPC from the VPC console\.

Optionally, you can upload the \.zip file to an Amazon S3 bucket in the same AWS region, and then specify the bucket and object name in the preceding command\. You need to replace the `--zip-file` parameter with the `--code` parameter, as shown following: 

```
--code S3Bucket=bucket-name,S3Key=zip-file-object-key
```

You can also create the Lambda function using the AWS Lambda console\. When creating the function, choose a VPC for the Lambda and then select the subnets and security groups from the provided fields\. 

**Next Step**

[Step 3: Test the Lambda function](Lambda.TestLambdaFunction.md)