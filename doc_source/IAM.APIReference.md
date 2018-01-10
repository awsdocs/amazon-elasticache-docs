# ElastiCache API Permissions: Actions, Resources, and Conditions Reference<a name="IAM.APIReference"></a>

When you are setting up [Access Control](IAM.md#IAM.AccessControl) and writing permissions policies that you can attach to an IAM identity \(identity\-based policies\), you can use the following table as a reference\. The table lists each Amazon ElastiCache API operation and the corresponding actions for which you can grant permissions to perform the action\. You specify the actions in the policy's `Action` field, and you specify a wildcard character \(\*\) as the resource value in the policy's `Resource` field\. 

You can use AWS\-wide condition keys in your ElastiCache policies to express conditions\. For a complete list of AWS\-wide keys, see [Available Keys for Conditions](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\.

**Note**  
To specify an action, use the `elasticache:` prefix followed by the API operation name \(for example, `elasticache:DescribeSnapshots`\)\. For all ElastiCache actions, specify the wildcard character \(\*\) as the resource\.

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


**Amazon ElastiCache API and Required Permissions for Actions**  

| ElastiCache API Operations | Required Permissions \(API Actions\) | Resources | 
| --- | --- | --- | 
|  [AddTagsToResource](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_AddTagsToResource.html) | `elasticache:AddTagsToResource` | `*` | 
|  [AuthorizeCacheSecurityGroupIngress](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_AuthorizeCacheSecurityGroupIngress.html) | `elasticache:AuthorizeCacheSecurityGroupIngress` | `*` | 
|  [CopySnapshot](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CopySnapshot.html) |  `elasticache:CopySnapshot` `s3:ListAllMyBuckets` |  `*` `*` | 
|  [CreateCacheCluster](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheCluster.html) |  `elasticache:CreateCacheCluster` `s3:GetObject`  If you use the `SnapshotArns` parameter, each member of the `SnapshotArns` list requires its own `s3:GetObject` permission with the `s3` ARN as its resource\.  |  `*` `arn:aws:s3:::my_bucket/snapshot1.rdb` Where *my\_bucket*/*snapshot1* is an S3 bucket and snapshot that you want to create the cache cluster from\. | 
|  [CreateCacheParameterGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheParameterGroup.html) | `elasticache:CreateCacheParameterGroup` | `*` | 
|  [CreateCacheSecurityGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheSecurityGroup.html) | `elasticache:CreateCacheSecurityGroup` | `*` | 
|  [CreateCacheSubnetGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheSubnetGroup.html) | `elasticache:CreateCacheSubnetGroup` | `*` | 
|  [CreateReplicationGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html) |  `elasticache:CreateReplicationGroup` `s3:GetObject`  If you use the `SnapshotArns` parameter, each member of the `SnapshotArns` list requires its own `s3:GetObject` permission with the `s3` ARN as its resource\.  |  `*` `arn:aws:s3:::my_bucket/snapshot1.rdb` Where *my\_bucket*/*snapshot1* is an S3 bucket and snapshot that you want to create the cache cluster from\. | 
|  [CreateSnapshot](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateSnapshot.html) | `elasticache:CreateSnapshot` | `*` | 
|  [DeleteCacheCluster](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheCluster.html) | `elasticache:DeleteCacheCluster` | `*` | 
|  [DeleteCacheParameterGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheParameterGroup.html) | `elasticache:DeleteCacheParameterGroup` | `*` | 
|  [DeleteCacheSecurityGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheSecurityGroup.html) | `elasticache:DeleteCacheSecurityGroup` | `*` | 
|  [DeleteCacheSubnetGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheSubnetGroup.html) | `elasticache:DeleteCacheSubnetGroup` | `*` | 
|  [DeleteReplicationGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteReplicationGroup.html) | `elasticache:DeleteReplicationGroup` | `*` | 
|  [DeleteSnapshot](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteSnapshot.html) | `elasticache:DeleteSnapshot` | `*` | 
|  [DescribeCacheClusters](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheClusters.html) | `elasticache:DescribeCacheClusters` | `*` | 
|  [DescribeCacheEngineVersions](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheEngineVersions.html) | `elasticache:DescribeCacheEngineVersions` | `*` | 
|  [DescribeCacheParameterGroups](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheParameterGroups.html) | `elasticache:DescribeCacheParameterGroups` | `*` | 
|  [DescribeCacheParameters](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheParameters.html) | `elasticache:DescribeCacheParameters` | `*` | 
|  [DescribeCacheSecurityGroups](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheSecurityGroups.html) | `elasticache:DescribeCacheSecurityGroups` | `*` | 
|  [DescribeCacheSubnetGroups](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheSubnetGroups.html) | `elasticache:DescribeCacheSubnetGroups` | `*` | 
|  [DescribeEngineDefaultParameters](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEngineDefaultParameters.html) | `elasticache:DescribeEngineDefaultParameters` | `*` | 
|  [DescribeEvents](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEvents.html) | `elasticache:DescribeEvents` | `*` | 
|  [DescribeReplicationGroups](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReplicationGroups.html) | `elasticache:DescribeReplicationGroups` | `*` | 
|  [DescribeReservedCacheNodes](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReservedCacheNodes.html) | `elasticache:DescribeReservedCacheNodes` | `*` | 
|  [DescribeReservedCacheNodesOfferings](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReservedCacheNodesOfferings.html) | `elasticache:DescribeReservedCacheNodesOfferings` | `*` | 
|  [DescribeSnapshots](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeSnapshots.html) | `elasticache:DescribeSnapshots` | `*` | 
|  [ListTagsForResource](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ListTagsForResource.html) | `elasticache:ListTagsForResource` | `*` | 
|  [ModifyCacheCluster](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html) | `elasticache:ModifyCacheCluster` | `*` | 
|  [ModifyCacheParameterGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheParameterGroup.html) | `elasticache:ModifyCacheParameterGroup` | `*` | 
|  [ModifyCacheSubnetGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheSubnetGroup.html) | `elasticache:ModifyCacheSubnetGroup` | `*` | 
|  [ModifyReplicationGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroup.html) | `elasticache:ModifyReplicationGroup` | `*` | 
|  [PurchaseReservedCacheNodesOffering](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_PurchaseReservedCacheNodesOffering.html) | `elasticache:PurchaseReservedCacheNodesOffering` | `*` | 
|  [RebootCacheCluster](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RebootCacheCluster.html) | `elasticache:RebootCacheCluster` | `*` | 
|  [RemoveTagsFromResource](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RemoveTagsFromResource.html) | `elasticache:RemoveTagsFromResource` | `*` | 
|  [ResetCacheParameterGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ResetCacheParameterGroup.html) | `elasticache:ResetCacheParameterGroup` | `*` | 
|  [RevokeCacheSecurityGroupIngress](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RevokeCacheSecurityGroupIngress.html) | `elasticache:RevokeCacheSecurityGroupIngress` | `*` | 
|  [TestFailover](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_TestFailover.html) | `elasticache:TestFailover` | `*` | 