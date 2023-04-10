# Step 3: Test the Lambda function<a name="Lambda.TestLambdaFunction"></a>

In this step, you invoke the Lambda function manually using the `invoke` command\. When the Lambda function executes, it generates a UUID and writes it to the ElastiCache cluster that you specified in your Lambda code\. The Lambda function then retrieves the item from the cache\. 

1. Invoke the Lambda function \(`AccessMemCache`\) using the AWS Lambda `invoke` command\. 

   For Linux, macOS, or Unix:

   ```
   aws lambda invoke \
   --function-name AccessMemCache  \
   --region us-east-1 \
   --profile adminuser \
   output.txt
   ```

   For Windows:

   ```
   aws lambda invoke ^
   --function-name AccessMemCache  ^
   --region us-east-1 ^
   --profile adminuser ^
   output.txt
   ```

1. Verify that the Lambda function executed successfully as follows:
   + Review the output\.txt file\.
   + Review the results in the AWS Lambda console\.
   + Verify the results in CloudWatch Logs\.