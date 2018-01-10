# Step 1: Create an AWS Account and Set Up Permissions \[One time\]<a name="GettingStarted.BeforeYouBegin"></a>

In order to use Amazon ElastiCache you must have an active AWS account and permissions to access ElastiCache as well as other AWS resources\.

## Step 1a: Create Your AWS Account<a name="elasticache-create-aws-account"></a>

If you don't already have an AWS account, you'll be prompted to create one when you sign up\. You will only be charged for usage time on AWS services that you sign up for\. 

**To create an AWS account**

1. Open [https://aws\.amazon\.com/](https://aws.amazon.com/), and then choose **Create an AWS Account**\.
**Note**  
This might be unavailable in your browser if you previously signed into the AWS Management Console\. In that case, choose **Sign in to a different account**, and then choose **Create a new AWS account**\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a PIN using the phone keypad\.

## Step 1b: Set Up Your Permissions \(New ElastiCache Customers only\)<a name="elasticache-set-up-permissions"></a>

Amazon ElastiCache creates and uses Service linked roles \(SLR\) to provision resources and access other AWS resources and services on your behalf\. In order for ElastiCache to create the SLR for you, you must use the *AmazonElastiCacheFullAccess* AWS managed policy, which comes pre\-provisioned with permission that the service requires to create a Service Linked Role on your behalf\.

If you are not using the default policy and choose to use a custom managed policy, ensure you have either permissions to call `iam:createServiceLinkedRole` or you have created the ElastiCache Service Linked Role\. 

For more information, see:

+ [Creating a New Policy](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) \(IAM\)

+ [AWS Managed \(Predefined\) Policies for Amazon ElastiCache](IAM.IdentityBasedPolicies.md#IAM.IdentityBasedPolicies.PredefinedPolicies)

+ [Using Service\-Linked Roles for ElastiCache](using-service-linked-roles.md)