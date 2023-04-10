# Tutorial: Configuring a Lambda function to access amazon ElastiCache in an Amazon VPC<a name="Lambda"></a>

In this tutorial, you do the following:
+ Create an Amazon ElastiCache cluster in your default Amazon Virtual Private Cloud \(Amazon VPC\) in the us\-east\-1 region\. 
+ Create a Lambda function to access the ElastiCache cluster\. When you create the Lambda function, you provide subnet IDs in your Amazon VPC and a VPC security group to allow the Lambda function to access resources in your VPC\. For illustration in this tutorial, the Lambda function generates a UUID, writes it to the cache, and retrieves it from the cache\.
+ Invoke the Lambda function manually and verify that it accessed the ElastiCache cluster in your VPC\.

**Important**  
 The tutorial uses the default Amazon VPC in the us\-east\-1 region in your account\. For more information about Amazon VPC, see [How to Get Started with Amazon VPC](https://docs.aws.amazon.com//AmazonVPC/latest/UserGuide/VPC_Introduction.html#howto) in the *Amazon VPC User Guide*\.

**Topics**
+ [Step 1: Create an ElastiCache cluster](Lambda.CreateECCluster.md)
+ [Step 2: Create a Lambda function](Lambda.CreateLambdaFunction.md)
+ [Step 3: Test the Lambda function](Lambda.TestLambdaFunction.md)

**Get Started**

[Step 1: Create an ElastiCache cluster](Lambda.CreateECCluster.md)