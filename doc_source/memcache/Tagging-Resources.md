# Tagging your ElastiCache resources<a name="Tagging-Resources"></a>

To help you manage your clusters and other ElastiCache resources, you can assign your own metadata to each resource in the form of tags\. Tags enable you to categorize your AWS resources in different ways, for example, by purpose, owner, or environment\. This is useful when you have many resources of the same type—you can quickly identify a specific resource based on the tags that you've assigned to it\. This topic describes tags and shows you how to create them\.

**Warning**  
As a best practice, we recommend that you do not include sensitive data in your tags\.

## Tag basics<a name="Tagging-basics"></a>

A tag is a label that you assign to an AWS resource\. Each tag consists of a key and an optional value, both of which you define\. Tags enable you to categorize your AWS resources in different ways, for example, by purpose or owner\. For example, you could define a set of tags for your account's ElastiCache clusters that helps you track each instance's owner and user group\.

We recommend that you devise a set of tag keys that meets your needs for each resource type\. Using a consistent set of tag keys makes it easier for you to manage your resources\. You can search and filter the resources based on the tags you add\. For more information about how to implement an effective resource tagging strategy, see the [AWS whitepaper Tagging Best Practices](https://d1.awsstatic.com/whitepapers/aws-tagging-best-practices.pdf)\.

Tags don't have any semantic meaning to ElastiCache and are interpreted strictly as a string of characters\. Also, tags are not automatically assigned to your resources\. You can edit tag keys and values, and you can remove tags from a resource at any time\. You can set the value of a tag to `null`\. If you add a tag that has the same key as an existing tag on that resource, the new value overwrites the old value\. If you delete a resource, any tags for the resource are also deleted\. Furthermore, if you add or delete tags on a replication group, all nodes in that replication group will also have their tags added or removed\.

 You can work with tags using the AWS Management Console, the AWS CLI, and the ElastiCache API\.

If you're using IAM, you can control which users in your AWS account have permission to create, edit, or delete tags\. For more information, see [Resource\-level permissions](IAM.ResourceLevelPermissions.md)\.

## Resources you can tag<a name="Tagging-your-resources"></a>

You can tag most ElastiCache resources that already exist in your account\. The table below lists the resources that support tagging\. If you're using the AWS Management Console, you can apply tags to resources by using the [Tag Editor](https://docs.aws.amazon.com/ARG/latest/userguide/tag-editor.html)\. Some resource screens enable you to specify tags for a resource when you create the resource; for example, a tag with a key of Name and a value that you specify\. In most cases, the console applies the tags immediately after the resource is created \(rather than during resource creation\)\. The console may organize resources according to the **Name** tag, but this tag doesn't have any semantic meaning to the ElastiCache service\.

 Additionally, some resource\-creating actions enable you to specify tags for a resource when the resource is created\. If tags cannot be applied during resource creation, we roll back the resource creation process\. This ensures that resources are either created with tags or not created at all, and that no resources are left untagged at any time\. By tagging resources at the time of creation, you can eliminate the need to run custom tagging scripts after resource creation\. 

 If you're using the Amazon ElastiCache API, the AWS CLI, or an AWS SDK, you can use the `Tags` parameter on the relevant ElastiCache API action to apply tags\. They are:
+ `CreateCacheCluster`
+ `CreateCacheParameterGroup`
+ `CreateCacheSecurityGroup`
+ `CreateCacheSubnetGroup`
+ `PurchaseReservedCacheNodesOffering`

The following table describes the ElastiCache resources that can be tagged, and the resources that can be tagged on creation using the ElastiCache API, the AWS CLI, or an AWS SDK\.


**Tagging support for ElastiCache resources**  

| Resource | Supports tags | Supports tagging on creation | 
| --- | --- | --- | 
| parametergroup | Yes | Yes | 
| securitygroup | Yes | Yes | 
| subnetgroup | Yes | Yes | 
| cluster | Yes | Yes | 
| reserved\-instance | Yes | Yes | 

You can apply tag\-based resource\-level permissions in your IAM policies to the ElastiCache API actions that support tagging on creation to implement granular control over the users and groups that can tag resources on creation\. Your resources are properly secured from creation—tags that are applied immediately to your resources\. Therefore any tag\-based resource\-level permissions controlling the use of resources are immediately effective\. Your resources can be tracked and reported on more accurately\. You can enforce the use of tagging on new resources, and control which tag keys and values are set on your resources\.

For more information, see [Tagging resources examples](#Tagging-your-resources-example)\.

 For more information about tagging your resources for billing, see [Monitoring costs with cost allocation tags](Tagging.md)\.

## Tag restrictions<a name="Tagging-restrictions"></a>

The following basic restrictions apply to tags:
+ Maximum number of tags per resource – 50
+ For each resource, each tag key must be unique, and each tag key can have only one value\.
+ Maximum key length – 128 Unicode characters in UTF\-8\.
+ Maximum value length – 256 Unicode characters in UTF\-8\.
+ Although ElastiCache allows for any character in its tags, other services can be restrictive\. The allowed characters across services are: letters, numbers, and spaces representable in UTF\-8, and the following characters: \+ \- = \. \_ : / @
+ Tag keys and values are case\-sensitive\.
+ The `aws:` prefix is reserved for AWS use\. If a tag has a tag key with this prefix, then you can't edit or delete the tag's key or value\. Tags with the `aws:` prefix do not count against your tags per resource limit\.

You can't terminate, stop, or delete a resource based solely on its tags; you must specify the resource identifier\. For example, to delete snapshots that you tagged with a tag key called `DeleteMe`, you must use the `DeleteSnapshot` action with the resource identifiers of the snapshots, such as `snap-1234567890abcdef0`\.

For more information on ElastiCache resources you can tag, see [Resources you can tag](#Tagging-your-resources)\.

## Tagging resources examples<a name="Tagging-your-resources-example"></a>
+ Creating a Cache Cluster using tags\.

  ```
  aws elasticache create-cache-cluster \
  --cluster-id testing-tags \
  --cluster-description cluster-test \
  --cache-subnet-group-name test \
  --cache-node-type cache.t2.micro \
  --engine redis \
  --tags Key="project",Value="XYZ" Key="Elasticache",Value="Service"
  ```

## Tag\-Based access control policy examples<a name="Tagging-access-control"></a>

1. Allowing `AddTagsToResource` action to a cluster only if the cluster has the tag Project=XYZ\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "elasticache:AddTagsToResource",
               "Resource": [
                   "arn:aws:elasticache:*:*:cluster:*"
               ],
               "Condition": {
                   "ForAllValues:StringEquals": {
                       "aws:ResourceTag/Project": "XYZ"
                   }
               }
           }
       ]
   }
   ```

1. Allowing to `RemoveTagsFromResource` action from a replication group if it contains the tags Project and Service and keys are different from Project and Service\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "elasticache:RemoveTagsFromResource",
               "Resource": [
                   "arn:aws:elasticache:*:*:replicationgroup:*"
               ],
               "Condition": {
                   "ForAllValues:StringEquals": {
                       "aws:ResourceTag/Service": "Elasticache",
                       "aws:ResourceTag/Project": "XYZ"
                   },                
                   "ForAnyValue:StringNotEqualsIgnoreCase": {
                       "aws:TagKeys": [
                           "Project",
                           "Service"
                       ]
                   }
               }
           }
       ]
   }
   ```

1. Allowing `AddTagsToResource` to any resource only if tags are different from Project and Service\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "elasticache:AddTagsToResource",
               "Resource": [
                   "arn:aws:elasticache:*:*:*:*"
               ],
               "Condition": {
                   "ForAllValues:StringNotEqualsIgnoreCase": {
                       "aws:TagKeys": [ 
                           "Service", 
                           "Project" 
                       ]
                   }
               }
           }
       ]
   }
   ```

For more information, see [Using condition keys](IAM.ConditionKeys.md)\.