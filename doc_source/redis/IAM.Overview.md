# Overview of Managing Access Permissions to Your ElastiCache Resources<a name="IAM.Overview"></a>

Every AWS resource is owned by an AWS account, and permissions to create or access a resource are governed by permissions policies\. An account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\)\. In addition, some services \(such as AWS Lambda\) also support attaching permissions policies to resources\. 

**Note**  
An *account administrator* \(or administrator user\) is a user with administrator privileges\. For more information, see [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

When granting permissions, you decide who is getting the permissions\. You also decide the resources they get permissions for and the specific actions that you want to allow on those resources\.

**Topics**
+ [Amazon ElastiCache Resources and Operations](#IAM.Overview.ResourcesAndOperations)
+ [Understanding Resource Ownership](#access-control-resource-ownership)
+ [Managing Access to Resources](#IAM.Overview.ManagingAccess)
+ [Specifying Policy Elements: Actions, Effects, Resources, and Principals](#IAM.Overview.PolicyElements)
+ [Specifying Conditions in a Policy](#IAM.Overview.Conditions)

## Amazon ElastiCache Resources and Operations<a name="IAM.Overview.ResourcesAndOperations"></a>

In Amazon ElastiCache, the primary resource is a *cache cluster*\.

These resources have unique Amazon Resource Names \(ARNs\) associated with them as shown in the following table\. 


****  

| Resource Type | ARN Format | 
| --- | --- | 
|  Cache Cluster | `arn:aws:elasticache:region:account-id:cluster:resource-name` | 

ElastiCache provides a set of operations to work with ElastiCache resources\. For a list of available operations, see Amazon ElastiCache [Actions](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_Operations.html)\.

## Understanding Resource Ownership<a name="access-control-resource-ownership"></a>

A *resource owner* is the AWS account that created the resource\. That is, the resource owner is the AWS account of the principal entity that authenticates the request that creates the resource\. A *principal entity* can be the root account, an IAM user, or an IAM role\)\. The following examples illustrate how this works:
+ Suppose that you use the root account credentials of your AWS account to create a cache cluster\. In this case, your AWS account is the owner of the resource\. In ElastiCache, the resource is the cache cluster\.
+ Suppose that you create an IAM user in your AWS account and grant permissions to create a cache cluster to that user\. In this case, the user can create a cache cluster\. However, your AWS account, to which the user belongs, owns the cache cluster resource\.
+ Suppose that you create an IAM role in your AWS account with permissions to create a cache cluster\. In this case, anyone who can assume the role can create a cache cluster\. Your AWS account, to which the role belongs, owns the cache cluster resource\. 

## Managing Access to Resources<a name="IAM.Overview.ManagingAccess"></a>

A *permissions policy* describes who has access to what\. The following section explains the available options for creating permissions policies\.

**Note**  
This section discusses using IAM in the context of Amazon ElastiCache\. It doesn't provide detailed information about the IAM service\. For complete IAM documentation, see [What Is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*\. For information about IAM policy syntax and descriptions, see [AWS IAM Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

Policies attached to an IAM identity are referred to as *identity\-based* policies \(IAM policies\)\. Policies attached to a resource are referred to as *resource\-based* policies\. Amazon ElastiCache supports only identity\-based policies \(IAM policies\)\. 

**Topics**
+ [Identity\-Based Policies \(IAM Policies\)](#IAM.Overview.ManagingAccess.IdentityBasedPolicies)
+ [Resource\-Based Policies](#IAM.Overview.ManagingAccess.ResourceBasedPolicies)

### Identity\-Based Policies \(IAM Policies\)<a name="IAM.Overview.ManagingAccess.IdentityBasedPolicies"></a>

You can attach policies to IAM identities\. For example, you can do the following:
+ **Attach a permissions policy to a user or a group in your account** – An account administrator can use a permissions policy that is associated with a particular user to grant permissions\. In this case, the permissions are for that user to create an ElastiCache resource, such as a cache cluster, parameter group, or security group\.
+ **Attach a permissions policy to a role \(grant cross\-account permissions\)** – You can attach an identity\-based permissions policy to an IAM role to grant cross\-account permissions\. For example, the administrator in Account A can create a role to grant cross\-account permissions to another AWS account \(for example, Account B\) or an AWS service as follows:

  1. Account A administrator creates an IAM role and attaches a permissions policy to the role that grants permissions on resources in Account A\.

  1. Account A administrator attaches a trust policy to the role identifying Account B as the principal who can assume the role\. 

  1. Account B administrator can then delegate permissions to assume the role to any users in Account B\. Doing this allows users in Account B to create or access resources in Account A\. In some cases, you might want to grant an AWS service permissions to assume the role\. To support this approach, the principal in the trust policy can also be an AWS service principal\. 

  For more information about using IAM to delegate permissions, see [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\.

The following is an example policy that allows a user to perform the `DescribeCacheClusters` action for your AWS account\. In the current implementation, ElastiCache doesn't support identifying specific resources using the resource ARNs for API actions\. \(This approach is also referred to as resource\-level permissions\)\. Thus, you must specify a wildcard character \(\*\)\.

```
{
   "Version": "2012-10-17",
   "Statement": [{
      "Sid": "DescribeCacheClusters",
      "Effect": "Allow",
      "Action": [
         "elasticache:DescribeCacheClusters"],
      "Resource": "*"
      }
   ]
}
```

For more information about using identity\-based policies with ElastiCache, see [Using Identity\-Based Policies \(IAM Policies\) for Amazon ElastiCache](IAM.IdentityBasedPolicies.md)\. For more information about users, groups, roles, and permissions, see [Identities \(Users, Groups, and Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the *IAM User Guide*\.

### Resource\-Based Policies<a name="IAM.Overview.ManagingAccess.ResourceBasedPolicies"></a>

Other services, such as Amazon S3, also support resource\-based permissions policies\. For example, you can attach a policy to an S3 bucket to manage access permissions to that bucket\. Amazon ElastiCache doesn't support resource\-based policies\. 

## Specifying Policy Elements: Actions, Effects, Resources, and Principals<a name="IAM.Overview.PolicyElements"></a>

For each Amazon ElastiCache resource \(see [Amazon ElastiCache Resources and Operations](#IAM.Overview.ResourcesAndOperations)\), the service defines a set of API operations \(see [Actions](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_Operations.html)\)\. To grant permissions for these API operations, ElastiCache defines a set of actions that you can specify in a policy\. For example, for the ElastiCache snapshot resource, the following actions are defined: `CreateCacheCluster`, `DeleteCacheCluster`, and `DescribeCacheCluster`\. Performing an API operation can require permissions for more than one action\.

The following are the most basic policy elements:
+ **Resource** – In a policy, you use an Amazon Resource Name \(ARN\) to identify the resource to which the policy applies\. For ElastiCache resources, you always use the wildcard character `(*)` in IAM policies\. For more information, see [Amazon ElastiCache Resources and Operations](#IAM.Overview.ResourcesAndOperations)\.
+ **Action** – You use action keywords to identify resource operations that you want to allow or deny\. For example, depending on the specified `Effect`, the `elasticache:CreateCacheCluster` permission allows or denies the user permissions to perform the Amazon ElastiCache `CreateCacheCluster` operation\.
+ **Effect** – You specify the effect when the user requests the specific action—this can be either allow or deny\. If you don't explicitly grant access to \(allow\) a resource, access is implicitly denied\. You can also explicitly deny access to a resource\. For example, you might do this to make sure that a user can't access a resource, even if a different policy grants access\.
+ **Principal** – In identity\-based policies \(IAM policies\), the user that the policy is attached to is the implicit principal\. For resource\-based policies, you specify the user, account, service, or other entity that you want to receive permissions \(applies to resource\-based policies only\)\. ElastiCache doesn't support resource\-based policies\.

To learn more about IAM policy syntax and descriptions, see [AWS IAM Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

For a table showing all of the Amazon ElastiCache API actions, see [ElastiCache API Permissions: Actions, Resources, and Conditions Reference](IAM.APIReference.md)\.

## Specifying Conditions in a Policy<a name="IAM.Overview.Conditions"></a>

When you grant permissions, you can use the IAM policy language to specify the conditions when a policy should take effect\. For example, you might want a policy to be applied only after a specific date\. For more information about specifying conditions in a policy language, see [Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#Condition) in the *IAM User Guide*\. 

To express conditions, you use predefined condition keys\. There are no condition keys specific to Amazon ElastiCache\. However, there are AWS\-wide condition keys that you can use as appropriate\. For a complete list of AWS\-wide keys, see [Available Keys for Conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\. 