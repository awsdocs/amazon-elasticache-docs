# Setting Up<a name="set-up"></a>

Following, you can find topics that describe the one\-time actions you must take to start using ElastiCache\.

**Topics**
+ [Create Your AWS Account](#elasticache-create-aws-account)
+ [Set Up Your Permissions \(New ElastiCache Users Only\)](#elasticache-set-up-permissions)

## Create Your AWS Account<a name="elasticache-create-aws-account"></a>

To use Amazon ElastiCache, you must have an active AWS account and permissions to access ElastiCache and other AWS resources\.

If you don't already have an AWS account, create one now\. AWS accounts are free\. You are not charged for signing up for an AWS service, only for using AWS services\.

**To create an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

## Set Up Your Permissions \(New ElastiCache Users Only\)<a name="elasticache-set-up-permissions"></a>

Amazon ElastiCache creates and uses service\-linked roles to provision resources and access other AWS resources and services on your behalf\. For ElastiCache to create a service\-linked role for you, use the AWS\-managed policy named `AmazonElastiCacheFullAccess`\. This role comes preprovisioned with permission that the service requires to create a service\-linked role on your behalf\.

You might decide not to use the default policy and instead to use a custom\-managed policy\. In this case, make sure that you have either permissions to call `iam:createServiceLinkedRole` or that you have created the ElastiCache service\-linked role\. 

For more information, see the following:
+ [Creating a New Policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) \(IAM\)
+ [AWS Managed \(Predefined\) Policies for Amazon ElastiCache](IAM.IdentityBasedPolicies.md#IAM.IdentityBasedPolicies.PredefinedPolicies)
+ [Using Service\-Linked Roles for Amazon ElastiCache](using-service-linked-roles.md)