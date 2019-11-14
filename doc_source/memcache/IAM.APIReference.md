# ElastiCache API Permissions: Actions, Resources, and Conditions Reference<a name="IAM.APIReference"></a>

When you set up [access control](IAM.md#IAM.AccessControl) and write permissions policies to attach to an IAM identity \(identity\-based policies\), use the following table as a reference\. The table lists each Amazon ElastiCache API operation and the corresponding actions for which you can grant permissions to perform the action\. You specify the actions in the policy's `Action` field, and you specify a wildcard character \(\*\) as the resource value in the policy's `Resource` field\. 

You can use AWS\-wide condition keys in your ElastiCache policies to express conditions\. For a complete list of AWS\-wide keys, see [Available Keys for Conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\.

**Note**  
To specify an action, use the `elasticache:` prefix followed by the API operation name \(for example, `elasticache:DescribeCacheClusters`\)\. For all ElastiCache actions, specify the wildcard character \(\*\) as the resource\.

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


**Amazon ElastiCache for Memcached API and Required Permissions for Actions**  

| ElastiCache API Operations | Required Permissions \(API Actions\) | Resources | 
| --- | --- | --- | 
|  [AddTagsToResource](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_AddTagsToResource.html) | `elasticache:AddTagsToResource` | `*` | 
|  [AuthorizeCacheSecurityGroupIngress](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_AuthorizeCacheSecurityGroupIngress.html) | `elasticache:AuthorizeCacheSecurityGroupIngress` | `*` | 
|  [CreateCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheCluster.html) |  `elasticache:CreateCacheCluster`  |  `*` Where *my\_bucket*/*snapshot1* is an S3 bucket and snapshot that you want to create the cache cluster from\. | 
|  [CreateCacheParameterGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheParameterGroup.html) | `elasticache:CreateCacheParameterGroup` | `*` | 
|  [CreateCacheSecurityGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheSecurityGroup.html) | `elasticache:CreateCacheSecurityGroup` | `*` | 
|  [CreateCacheSubnetGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheSubnetGroup.html) | `elasticache:CreateCacheSubnetGroup` | `*` | 
|  [DeleteCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheCluster.html) | `elasticache:DeleteCacheCluster` | `*` | 
|  [DeleteCacheParameterGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheParameterGroup.html) | `elasticache:DeleteCacheParameterGroup` | `*` | 
|  [DeleteCacheSecurityGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheSecurityGroup.html) | `elasticache:DeleteCacheSecurityGroup` | `*` | 
|  [DeleteCacheSubnetGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheSubnetGroup.html) | `elasticache:DeleteCacheSubnetGroup` | `*` | 
|  [DescribeCacheClusters](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheClusters.html) | `elasticache:DescribeCacheClusters` | `*` | 
|  [DescribeCacheEngineVersions](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheEngineVersions.html) | `elasticache:DescribeCacheEngineVersions` | `*` | 
|  [DescribeCacheParameterGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheParameterGroups.html) | `elasticache:DescribeCacheParameterGroups` | `*` | 
|  [DescribeCacheParameters](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheParameters.html) | `elasticache:DescribeCacheParameters` | `*` | 
|  [DescribeCacheSecurityGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheSecurityGroups.html) | `elasticache:DescribeCacheSecurityGroups` | `*` | 
|  [DescribeCacheSubnetGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheSubnetGroups.html) | `elasticache:DescribeCacheSubnetGroups` | `*` | 
|  [DescribeEngineDefaultParameters](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEngineDefaultParameters.html) | `elasticache:DescribeEngineDefaultParameters` | `*` | 
|  [DescribeEvents](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEvents.html) | `elasticache:DescribeEvents` | `*` | 
|  [DescribeReservedCacheNodes](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReservedCacheNodes.html) | `elasticache:DescribeReservedCacheNodes` | `*` | 
|  [DescribeReservedCacheNodesOfferings](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReservedCacheNodesOfferings.html) | `elasticache:DescribeReservedCacheNodesOfferings` | `*` | 
|  [ListTagsForResource](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ListTagsForResource.html) | `elasticache:ListTagsForResource` | `*` | 
|  [ModifyCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html) | `elasticache:ModifyCacheCluster` | `*` | 
|  [ModifyCacheParameterGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheParameterGroup.html) | `elasticache:ModifyCacheParameterGroup` | `*` | 
|  [ModifyCacheSubnetGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheSubnetGroup.html) | `elasticache:ModifyCacheSubnetGroup` | `*` | 
|  [ModifyReplicationGroupShardConfiguration](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroupShardConfiguration.html) | `elasticache:ModifyReplicationGroupShardConfiguration` | `*` | 
|  [PurchaseReservedCacheNodesOffering](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_PurchaseReservedCacheNodesOffering.html) | `elasticache:PurchaseReservedCacheNodesOffering` | `*` | 
|  [RebootCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RebootCacheCluster.html) | `elasticache:RebootCacheCluster` | `*` | 
|  [RemoveTagsFromResource](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RemoveTagsFromResource.html) | `elasticache:RemoveTagsFromResource` | `*` | 
|  [ResetCacheParameterGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ResetCacheParameterGroup.html) | `elasticache:ResetCacheParameterGroup` | `*` | 
|  [RevokeCacheSecurityGroupIngress](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RevokeCacheSecurityGroupIngress.html) | `elasticache:RevokeCacheSecurityGroupIngress` | `*` | 