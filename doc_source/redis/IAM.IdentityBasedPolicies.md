# Using identity\-based policies \(IAM policies\) for Amazon ElastiCache<a name="IAM.IdentityBasedPolicies"></a>

This topic provides examples of identity\-based policies in which an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\)\. 

**Important**  
We recommend that you first read the topics that explain the basic concepts and options to manage access to Amazon ElastiCache resources\. For more information, see [Overview of managing access permissions to your ElastiCache resources](IAM.Overview.md)\. 

The sections in this topic cover the following:
+ [Permissions required to use the Amazon ElastiCache console](#IAM.IdentityBasedPolicies.MinCONPolicies)
+ [AWS\-managed \(predefined\) policies for Amazon ElastiCache](#IAM.IdentityBasedPolicies.PredefinedPolicies)
+ [Customer\-managed policy examples](#IAM.IdentityBasedPolicies.CustomerManagedPolicies)

The following shows an example of a permissions policy\.

```
{
   "Version": "2012-10-17",
   "Statement": [{
       "Sid": "AllowClusterPermissions",
       "Effect": "Allow",
       "Action": [
          "elasticache:CreateCacheCluster",
          "elasticache:CreateReplicationGroup",
          "elasticache:DescribeCacheClusters",
          "elasticache:ModifyCacheCluster"],
       "Resource": "*"
       },
       {
         "Sid": "AllowUserToPassRole",
         "Effect": "Allow",
         "Action": [ "iam:PassRole" ],
       "Resource": "arn:aws:iam::123456789012:role/EC2-roles-for-cluster"
       }
   ]
}
```

The policy has two statements:
+ The first statement grants permissions for the Amazon ElastiCache actions \(`elasticache:CreateCacheCluster`, `elasticache:DescribeCacheClusters`, `elasticache:ModifyCacheCluster` 
+ The second statement grants permissions for the IAM action \(`iam:PassRole`\) on the IAM role name specified at the end of the `Resource` value\.

The policy doesn't specify the `Principal` element because in an identity\-based policy you don't specify the principal who gets the permission\. When you attach policy to a user, the user is the implicit principal\. When you attach a permissions policy to an IAM role, the principal identified in the role's trust policy gets the permissions\. 

For a table showing all of the Amazon ElastiCache API actions and the resources that they apply to, see [ElastiCache API permissions: Actions, resources, and conditions reference](IAM.APIReference.md)\. 

## Permissions required to use the Amazon ElastiCache console<a name="IAM.IdentityBasedPolicies.MinCONPolicies"></a>

The permissions reference table lists the Amazon ElastiCache API operations and shows the required permissions for each operation\. For more information about ElastiCache API operations, see [ElastiCache API permissions: Actions, resources, and conditions reference](IAM.APIReference.md)\. 

 To use the Amazon ElastiCache console, first grant permissions for additional actions as shown in the following permissions policy\. 

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Sid": "MinPermsForECConsole",
        "Effect": "Allow",
        "Action": [
            "elasticache:Describe*",
            "elasticache:List*",
            "ec2:DescribeAvailabilityZones",
            "ec2:DescribeVpcs",
            "ec2:DescribeAccountAttributes",
            "ec2:DescribeSecurityGroups",
            "cloudwatch:GetMetricStatistics",
            "cloudwatch:DescribeAlarms",
            "s3:ListAllMyBuckets",
            "sns:ListTopics",
            "sns:ListSubscriptions" ],
        "Resource": "*"
        }
    ]
}
```

The ElastiCache console needs these additional permissions for the following reasons:
+ Permissions for the ElastiCache actions enable the console to display ElastiCache resources in the account\.
+ The console needs permissions for the `ec2` actions to query Amazon EC2 so it can display Availability Zones, VPCs, security groups, and account attributes\.
+ The permissions for `cloudwatch` actions enable the console to retrieve Amazon CloudWatch metrics and alarms, and display them in the console\.
+ The permissions for `sns` actions enable the console to retrieve Amazon Simple Notification Service \(Amazon SNS\) topics and subscriptions, and display them in the console\.

## AWS\-managed \(predefined\) policies for Amazon ElastiCache<a name="IAM.IdentityBasedPolicies.PredefinedPolicies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. Managed policies grant necessary permissions for common use cases so you can avoid having to investigate what permissions are needed\. For more information, see [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\. 

The following AWS managed policies, which you can attach to users in your account, are specific to ElastiCache:
+ **AmazonElastiCacheReadOnlyAccess** \- Grants read\-only access to Amazon ElastiCache resources\.
+ **AmazonElastiCacheFullAccess** \- Grants full access to Amazon ElastiCache resources\.

You can also create your own custom IAM policies to allow permissions for Amazon ElastiCache API actions\. You can attach these custom policies to the IAM users or groups that require those permissions\. 

## Customer\-managed policy examples<a name="IAM.IdentityBasedPolicies.CustomerManagedPolicies"></a>

If you are not using a default policy and choose to use a custom\-managed policy, ensure one of two things\. Either you should have permissions to call `iam:createServiceLinkedRole` \(for more information, see [Example 5: Allow a user to call IAM CreateServiceLinkedRole API](#create-service-linked-role-policy)\)\. Or you should have created an ElastiCache service\-linked role\. 

When combined with the minimum permissions needed to use the Amazon ElastiCache console, the example policies in this section grant additional permissions\. The examples are also relevant to the AWS SDKs and the AWS CLI\. For more information about what permissions are needed to use the ElastiCache console, see [Permissions required to use the Amazon ElastiCache console](#IAM.IdentityBasedPolicies.MinCONPolicies)\.

For instructions on setting up IAM users and groups, see [Creating Your First IAM User and Administrators Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\. 

**Important**  
Always test your IAM policies thoroughly before using them in production\. Some ElastiCache actions that appear simple can require other actions to support them when you are using the ElastiCache console\. For example, `elasticache:CreateCacheCluster` grants permissions to create ElastiCache cache clusters\. However, to perform this operation, the ElastiCache console uses a number of `Describe` and `List` actions to populate console lists\.

**Topics**
+ [Example 1: Allow a user to create and manage security groups](#example-allow-network-admin-access)
+ [Example 2: Allow a user read\-only access to ElastiCache resources](#example-allow-list-current-elasticache-resources)
+ [Example 3: Allow a user to perform common ElastiCache system administrator tasks](#example-allow-specific-elasticache-actions)
+ [Example 4: Allow a user to access all ElastiCache API actions](#allow-unrestricted-access)
+ [Example 5: Allow a user to call IAM CreateServiceLinkedRole API](#create-service-linked-role-policy)
+ [Example 6: Allow a user to connect to replication group using IAM authentication](#iam-connect-policy)

### Example 1: Allow a user to create and manage security groups<a name="example-allow-network-admin-access"></a>

The following policy grants permissions for the security group's specific ElastiCache actions\. Typically, you attach this type of permissions policy to the system administrators group\.

```
{
   "Version": "2012-10-17",
   "Statement":[{
      "Sid": "SecGrpAllows",
      "Effect":"Allow",
      "Action":[
          "elasticache:CreateCacheSecurityGroup",
          "elasticache:DeleteCacheSecurityGroup",
          "elasticache:DescribeCacheSecurityGroup",
          "elasticache:AuthorizeCacheSecurityGroupIngress",
          "elasticache:RevokeCacheSecurityGroupIngress"],
      "Resource":"*"
      }
   ]
}
```

### Example 2: Allow a user read\-only access to ElastiCache resources<a name="example-allow-list-current-elasticache-resources"></a>

The following policy grants permissions ElastiCache actions that allow a user to list resources\. Typically, you attach this type of permissions policy to a managers group\.

```
{
   "Version": "2012-10-17",
   "Statement":[{
      "Sid": "ECUnrestricted",
      "Effect":"Allow",
      "Action": [
          "elasticache:Describe*",
          "elasticache:List*"],
      "Resource":"*"
      }
   ]
}
```

### Example 3: Allow a user to perform common ElastiCache system administrator tasks<a name="example-allow-specific-elasticache-actions"></a>

Common system administrator tasks include modifying cache clusters, parameters, and parameter groups\. A system administrator may also want to get information about the ElastiCache events\. The following policy grants a user permissions to perform ElastiCache actions for these common system administrator tasks\. Typically, you attach this type of permissions policy to the system administrators group\.

```
{
   "Version": "2012-10-17",
   "Statement":[{
      "Sid": "ECAllowSpecific",
      "Effect":"Allow",
      "Action":[
          "elasticache:ModifyCacheCluster",
          "elasticache:RebootCacheCluster",
          "elasticache:DescribeCacheClusters",
          "elasticache:DescribeEvents",
          "elasticache:ModifyCacheParameterGroup",
          "elasticache:DescribeCacheParameterGroups",
          "elasticache:DescribeCacheParameters",
          "elasticache:ResetCacheParameterGroup",
          "elasticache:DescribeEngineDefaultParameters"],
      "Resource":"*"
      }
   ]
}
```

### Example 4: Allow a user to access all ElastiCache API actions<a name="allow-unrestricted-access"></a>

The following policy allows a user to access all ElastiCache actions\. We recommend that you grant this type of permissions policy only to an administrator user\. 

```
{
   "Version": "2012-10-17",
   "Statement":[{
      "Sid": "ECAllowSpecific",
      "Effect":"Allow",
      "Action":[
          "elasticache:*" ],
      "Resource":"*"
      }
   ]
}
```

### Example 5: Allow a user to call IAM CreateServiceLinkedRole API<a name="create-service-linked-role-policy"></a>

The following policy allows user to call the IAM `CreateServiceLinkedRole` API\. We recommend that you grant this type of permissions policy to the user who invokes mutative ElastiCache operations\.

```
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Sid":"CreateSLRAllows",
      "Effect":"Allow",
      "Action":[
        "iam:CreateServiceLinkedRole"
      ],
      "Resource":"*",
      "Condition":{
        "StringLike":{
          "iam:AWSServiceName":"elasticache.amazonaws.com"
        }
      }
    }
  ]
}
```

### Example 6: Allow a user to connect to replication group using IAM authentication<a name="iam-connect-policy"></a>

The following policy allows any user to connect to any replication group, using IAM authentication, from the IP address 123\.45\.167\.89\.

```
{
  "Version" : "2012-10-17",
  "Statement" :
  [
    {
      "Effect" : "Allow",
      "Action" : ["elasticache:Connect"],
      "Resource" : [
        "arn:aws:elasticache:us-east-1:123456789012:replicationgroup:*",
      ],
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": "123.45.167.89"
        }
      }
    },
    {
      "Effect" : "Allow",
      "Action" : ["elasticache:Connect"],
      "Resource" : [
        "arn:aws:elasticache:us-east-1:123456789012:user:*"
      ]
    }
  ]
}
```