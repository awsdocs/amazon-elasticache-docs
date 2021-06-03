# ElastiCache API permissions: Actions, resources, and conditions reference<a name="IAM.APIReference"></a>

When you set up [access control](IAM.md#IAM.AccessControl) and write permissions policies to attach to an IAM policy \(either idenity\-based or resource\-based\), use the following table as a reference\. The table lists each Amazon ElastiCache API operation and the corresponding actions for which you can grant permissions to perform the action\. You specify the actions in the policy's `Action` field, and you specify a resource value in the policy's `Resource` field\. Unless indicated otherwise, the resource is required\. Some fields include both a required resource and optional resources\. When there is no resource ARN, the resource in the policy is a wildcard \(\*\)\.

You can use condition keys in your ElastiCache policies to express conditions\. To see a list of ElastiCache\-specific condition keys, along with the actions and resource types to which they apply, see [Using condition keys](IAM.ConditionKeys.md)\. For a complete list of AWS\-wide keys, see [Available Keys for Conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\.

**Note**  
To specify an action, use the `elasticache:` prefix followed by the API operation name \(for example, `elasticache:DescribeCacheClusters`\)\.

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window\. To close the window, choose the close button \(**X**\) in the lower\-right corner\.


**Amazon ElastiCache API and required permissions for actions**  

| ElastiCache API operations | Required permissions \(API actions\) | Resources  | 
| --- | --- | --- | 
|  [AddTagsToResource](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_AddTagsToResource.html) | `elasticache:AddTagsToResource` | \(Optional\) Cluster, snapshot | 
|  [AuthorizeCacheSecurityGroupIngress](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_AuthorizeCacheSecurityGroupIngress.html) | `elasticache:AuthorizeCacheSecurityGroupIngress` | Security group | 
|  [CreateCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheCluster.html) |  `elasticache:CreateCacheCluster` `elasticache:AddTagsToResource`  |  Parameter group\. \(Optional\) Cache cluster, Replication group, Snapshot, Security group Ids and Subnet group Where *my\_bucket*/*snapshot1* is an S3 bucket and snapshot that you want to create the cache cluster from\. | 
|  [CreateCacheParameterGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheParameterGroup.html) | `elasticache:CreateCacheParameterGroup` `elasticache:AddTagsToResource` | Parameter group | 
|  [CreateCacheSecurityGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheSecurityGroup.html) | `elasticache:CreateCacheSecurityGroup` `elasticache:AddTagsToResource` | Security group | 
|  [CreateCacheSubnetGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheSubnetGroup.html) | `elasticache:CreateCacheSubnetGroup` `elasticache:AddTagsToResource` | Subnet group | \* | 
|  [DecreaseNodeGroupsInGlobalReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DecreaseNodeGroupsInGlobalReplicationGroup.html) | `elasticache:DecreaseNodeGroupsInGlobalReplicationGroup` | GlobalReplicationGroup | 
|  [DecreaseReplicaCount](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DecreaseReplicaCount.html) | `elasticache:DecreaseReplicaCount` | Replication group | 
|  [DeleteCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheCluster.html) | `elasticache:DeleteCacheCluster` | Cache cluster\. \(Optional\) Snapshot | 
|  [DeleteCacheParameterGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheParameterGroup.html) | `elasticache:DeleteCacheParameterGroup` | Parameter group | 
|  [DeleteCacheSubnetGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheSubnetGroup.html) | `elasticache:DeleteCacheSubnetGroup` | Subnet group | 
|  [DescribeCacheClusters](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheClusters.html) | `elasticache:DescribeCacheClusters` | Cluster | 
|  [DescribeCacheEngineVersions](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheEngineVersions.html) | `elasticache:DescribeCacheEngineVersions` | No Resource ARN: \* | 
|  [DescribeCacheParameterGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheParameterGroups.html) | `elasticache:DescribeCacheParameterGroups` | Parameter group | 
|  [DescribeCacheParameters](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheParameters.html) | `elasticache:DescribeCacheParameters` | Parameter group | 
|  [DescribeCacheSecurityGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheSecurityGroups.html) | `elasticache:DescribeCacheSecurityGroups` | Security group | 
|  [DescribeCacheSubnetGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheSubnetGroups.html) | `elasticache:DescribeCacheSubnetGroups` | Subnet group | \* | 
|  [DescribeEngineDefaultParameters](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEngineDefaultParameters.html) | `elasticache:DescribeEngineDefaultParameters` | No Resource ARN:\* | 
|  [DescribeEvents](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEvents.html) | `elasticache:DescribeEvents` | No Resource ARN: \* | 
|  [DescribeReservedCacheNodes](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReservedCacheNodes.html) | `elasticache:DescribeReservedCacheNodes` | Reserved\-instance | 
|  [DescribeReservedCacheNodesOfferings](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReservedCacheNodesOfferings.html) | `elasticache:DescribeReservedCacheNodesOfferings` | No Resource ARN: \* | 
|  [DescribeServiceUpdates](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeServiceUpdates.html) | `elasticache:DescribeServiceUpdates` | No Resource ARN: \* | 
|  [ListTagsForResource](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ListTagsForResource.html) | `elasticache:ListTagsForResource` | \(Optional\) Cluster, snapshot | 
|  [ModifyCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html) | `elasticache:ModifyCacheCluster` | Cache cluster\. \(Optional\) Parameter group, Security group | 
|  [ModifyCacheParameterGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheParameterGroup.html) | `elasticache:ModifyCacheParameterGroup` | Parameter group | 
|  [ModifyCacheSubnetGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheSubnetGroup.html) | `elasticache:ModifyCacheSubnetGroup` | Subnet group | 
|  [ModifyReplicationGroupShardConfiguration](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroupShardConfiguration.html) | `elasticache:ModifyReplicationGroupShardConfiguration` | Replication group | 
|  [PurchaseReservedCacheNodesOffering](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_PurchaseReservedCacheNodesOffering.html) | `elasticache:PurchaseReservedCacheNodesOffering` `elasticache:AddTagsToResource` | Reserved\-instance | 
|  [RebootCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RebootCacheCluster.html) | `elasticache:RebootCacheCluster` | Cluster | 
|  [RemoveTagsFromResource](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RemoveTagsFromResource.html) | `elasticache:RemoveTagsFromResource` | \(Optional\) Cluster, snapshot | 
|  [ResetCacheParameterGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ResetCacheParameterGroup.html) | `elasticache:ResetCacheParameterGroup` | Parameter group | 
|  [RevokeCacheSecurityGroupIngress](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RevokeCacheSecurityGroupIngress.html) | `elasticache:RevokeCacheSecurityGroupIngress` | No Resource ARN: \* | 