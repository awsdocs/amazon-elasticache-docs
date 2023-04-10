# ElastiCache API permissions: Actions, resources, and conditions reference<a name="IAM.APIReference"></a>

When you set up [access control](IAM.md) and write permissions policies to attach to an IAM policy \(either idenity\-based or resource\-based\), use the following table as a reference\. The table lists each Amazon ElastiCache API operation and the corresponding actions for which you can grant permissions to perform the action\. You specify the actions in the policy's `Action` field, and you specify a resource value in the policy's `Resource` field\. Unless indicated otherwise, the resource is required\. Some fields include both a required resource and optional resources\. When there is no resource ARN, the resource in the policy is a wildcard \(\*\)\.

You can use condition keys in your ElastiCache policies to express conditions\. To see a list of ElastiCache\-specific condition keys, along with the actions and resource types to which they apply, see [Using condition keys](IAM.ConditionKeys.md)\. For a complete list of AWS\-wide keys, see [Available Keys for Conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\.

**Note**  
To specify an action, use the `elasticache:` prefix followed by the API operation name \(for example, `elasticache:DescribeCacheClusters`\)\.

Use the scroll bars to see the rest of the table\.


**Amazon ElastiCache API and required permissions for actions**  

| ElastiCache API operations | Required permissions \(API actions\) | Resources  | 
| --- | --- | --- | 
|  [AddTagsToResource](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_AddTagsToResource.html) | `elasticache:AddTagsToResource` | \(Optional\) Cluster, snapshot | 
|  [AuthorizeCacheSecurityGroupIngress](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_AuthorizeCacheSecurityGroupIngress.html) | `elasticache:AuthorizeCacheSecurityGroupIngress` | Security group | 
|  [BatchApplyUpdateAction](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_BatchApplyUpdateAction.html) |   `elasticache:BatchApplyUpdateAction` |  \(Optional\) Cluster, replication group | 
| [BatchStopUpdateAction](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_BatchStopUpdateAction.html) |   `elasticache:BatchStopUpdateAction` |  \(Optional\) Cluster, replication group | 
| [CompleteMigration](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CompleteMigration.html) |   `elasticache:CompleteMigration` |  \(Optional\) Cluster, replication group | 
|  [CopySnapshot](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CopySnapshot.html) |  `elasticache:CopySnapshot` `elasticache:AddTagsToResource` `s3:GetBucketLocation` `s3:ListAllMyBuckets` |  Snapshot \(Source, Target\) \* \* | 
|  [CreateCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheCluster.html) |  `elasticache:CreateCacheCluster` `elasticache:AddTagsToResource` `s3:GetObject`  If you use the `SnapshotArns` parameter, each member of the `SnapshotArns` list requires its own `s3:GetObject` permission with the `s3` ARN as its resource\.  |  Parameter group\. \(Optional\) Cache cluster, Replication group, Snapshot, Security group Ids and Subnet group `arn:aws:s3:::my_bucket/snapshot1.rdb` Where *my\_bucket*/*snapshot1* is an S3 bucket and snapshot that you want to create the cache cluster from\. | 
|  [CreateCacheParameterGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheParameterGroup.html) | `elasticache:CreateCacheParameterGroup` `elasticache:AddTagsToResource` | Parameter group | 
|  [CreateCacheSecurityGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheSecurityGroup.html) | `elasticache:CreateCacheSecurityGroup` `elasticache:AddTagsToResource` | Security group | 
|  [CreateCacheSubnetGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheSubnetGroup.html) | `elasticache:CreateCacheSubnetGroup` `elasticache:AddTagsToResource` | Subnet group | 
|  [CreateGlobalReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateGlobalReplicationGroup.html) |  `elasticache:CreateGlobalReplicationGroup` | Global replication group, replication group | 
|  [CreateReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html) |  `elasticache:CreateReplicationGroup` `elasticache:AddTagsToResource` `s3:GetObject`  If you use the `SnapshotArns` parameter, each member of the `SnapshotArns` list requires its own `s3:GetObject` permission with the `s3` ARN as its resource\.  |  Parameter group\. \(Optional\) Replication group, Snapshot, Subnet group, Global Replication Group, Primary cluster Id, Security group Ids `arn:aws:s3:::my_bucket/snapshot1.rdb` Where *my\_bucket*/*snapshot1* is an S3 bucket and snapshot that you want to create the cache cluster from\. | 
|  [CreateSnapshot](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateSnapshot.html) | `elasticache:CreateSnapshot` `elasticache:AddTagsToResource` | Snapshot\. \(Optional\) Cache cluster, Replication group | 
|  [CreateUser](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateUser.html) \(for Redis 6\.0 onward\) | `elasticache:CreateUser` `elasticache:AddTagsToResource` | User | 
|  [CreateUserGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateUserGroup.html) \(for Redis 6\.0 onward\) | `elasticache:CreateUserGroup` `elasticache:AddTagsToResource` | UserGroup | 
|  [DecreaseNodeGroupsInGlobalReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DecreaseNodeGroupsInGlobalReplicationGroup.html) | `elasticache:DecreaseNodeGroupsInGlobalReplicationGroup` | GlobalReplicationGroup | 
|  [DecreaseReplicaCount](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DecreaseReplicaCount.html) | `elasticache:DecreaseReplicaCount` | Replication group | 
|  [DeleteCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheCluster.html) | `elasticache:DeleteCacheCluster` | Cache cluster\. \(Optional\) Snapshot | 
|  [DeleteCacheParameterGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheParameterGroup.html) | `elasticache:DeleteCacheParameterGroup` | Parameter group | 
|  [DeleteCacheSubnetGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheSubnetGroup.html) | `elasticache:DeleteCacheSubnetGroup` | Subnet group | 
|  [DeleteGlobalReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteGlobalReplicationGroup.html) | `elasticache:DeleteGlobalReplicationGroup` | GlobalReplicationGroup | 
|  [DeleteReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteReplicationGroup.html) | `elasticache:DeleteReplicationGroup` | Replication group\. \(Optional\) Snapshot | 
|  [DeleteSnapshot](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteSnapshot.html) | `elasticache:DeleteSnapshot` | Snapshot | 
|  [DeleteUser](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteUser.html) \(for Redis 6\.0 onward\)  | `elasticache:DeleteUser` | User | 
|  [DeleteUserGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteUserGroup.html) \(for Redis 6\.0 onward\)  | `elasticache:DeleteUserGroup` | UserGroup | 
|  [DescribeCacheClusters](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheClusters.html) | `elasticache:DescribeCacheClusters` | Cluster | 
|  [DescribeCacheEngineVersions](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheEngineVersions.html) | `elasticache:DescribeCacheEngineVersions` | No Resource ARN: \* | 
|  [DescribeCacheParameterGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheParameterGroups.html) | `elasticache:DescribeCacheParameterGroups` | Parameter group | 
|  [DescribeCacheParameters](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheParameters.html) | `elasticache:DescribeCacheParameters` | Parameter group | 
|  [DescribeCacheSecurityGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheSecurityGroups.html) | `elasticache:DescribeCacheSecurityGroups` | Security group | 
|  [DescribeCacheSubnetGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheSubnetGroups.html) | `elasticache:DescribeCacheSubnetGroups` | Subnet group | \* | 
|  [DescribeEngineDefaultParameters](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEngineDefaultParameters.html) | `elasticache:DescribeEngineDefaultParameters` | No Resource ARN:\* | 
|  [DescribeEvents](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEvents.html) | `elasticache:DescribeEvents` | No Resource ARN: \* | 
|  [DescribeGlobalReplicationGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeGlobalReplicationGroups.html) | `elasticache:DescribeGlobalReplicationGroups` | GlobalReplicationGroup | 
|  [DescribeGlobalReplicationGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeGlobalReplicationGroups.html) | `elasticache:DescribeGlobalReplicationGroups` | No Resource ARN: \* | 
|  [DescribeReplicationGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReplicationGroups.html) | `elasticache:DescribeReplicationGroups` | ReplicationGroup | 
|  [DescribeReservedCacheNodes](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReservedCacheNodes.html) | `elasticache:DescribeReservedCacheNodes` | Reserved\-instance | 
|  [DescribeReservedCacheNodesOfferings](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReservedCacheNodesOfferings.html) | `elasticache:DescribeReservedCacheNodesOfferings` | No Resource ARN: \* | 
|  [DescribeServiceUpdates](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeServiceUpdates.html) | `elasticache:DescribeServiceUpdates` | No Resource ARN: \* | 
|  [DescribeSnapshots](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeSnapshots.html) | `elasticache:DescribeSnapshots` | Snapshot | 
|  [DescribeUpdateActions](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeUpdateActions.html) | `elasticache:DescribeUpdateActions` | \(Optional\) Cluster, replication group | 
|  [DescribeUsers](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeUsers.html) \(For Redis 6\.0 onward\) | `elasticache:DescribeUsers` | User | 
|  [DescribeUserGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeUserGroups.html) \(For Redis 6\.0 onward\) | `elasticache:DescribeUserGroups` | UserGroup | 
|  [DisassociateGlobalReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DisassociateGlobalReplicationGroup.html) | `elasticache:DisassociateGlobalReplicationGroup` | GlobalReplicationGroup | 
|  [FailoverGlobalReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_FailoverGlobalReplicationGroup.html) | `elasticache:FailoverGlobalReplicationGroup` | GlobalReplicationGroup | 
|  [IncreaseNodeGroupsInGlobalReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_IncreaseNodeGroupsInGlobalReplicationGroup.html) | `elasticache:IncreaseNodeGroupsInGlobalReplicationGroup` | GlobalReplicationGroup | 
|  [ListAllowedNodeTypeModifications](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ListAllowedNodeTypeModifications.html) | `elasticache:ListAllowedNodeTypeModifications` | \(Optional\) Cluster, replication group | 
|  [IncreaseReplicaCount](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_IncreaseReplicaCount.html) | `elasticache:IncreaseReplicaCount` | Replication group | 
|  [ListTagsForResource](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ListTagsForResource.html) | `elasticache:ListTagsForResource` | \(Optional\) Cluster, snapshot | 
|  [ModifyCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html) | `elasticache:ModifyCacheCluster` | Cache cluster\. \(Optional\) Parameter group, Security group | 
|  [ModifyCacheParameterGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheParameterGroup.html) | `elasticache:ModifyCacheParameterGroup` | Parameter group | 
|  [ModifyCacheSubnetGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheSubnetGroup.html) | `elasticache:ModifyCacheSubnetGroup` | Subnet group | 
|  [ModifyGlobalReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyGlobalReplicationGroup.html) | `elasticache:ModifyGlobalReplicationGroup` | GlobalReplicationGroup | 
|  [ModifyReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroup.html) | `elasticache:ModifyReplicationGroup` | Replication group\. \(Optional\) Parameter group, Security group | 
|  [ModifyReplicationGroupShardConfiguration](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroupShardConfiguration.html) | `elasticache:ModifyReplicationGroupShardConfiguration` | Replication group | 
|  [ModifyUser](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyUser.html) \(for Redis 6\.0 onward\)  | `elasticache:ModifyUser` | User | 
|  [ModifyUserGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyUserGroup.html) \(for Redis 6\.0 onward\) | `elasticache:ModifyUserGroup` | UserGroup | 
|  [PurchaseReservedCacheNodesOffering](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_PurchaseReservedCacheNodesOffering.html) | `elasticache:PurchaseReservedCacheNodesOffering` `elasticache:AddTagsToResource` | Reserved\-instance | 
|  [RebalanceSlotsInGlobalReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RebalanceSlotsInGlobalReplicationGroup.html) | `elasticache:RebalanceSlotsInGlobalReplicationGroup` | GlobalReplicationGroup | 
|  [RebootCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RebootCacheCluster.html) | `elasticache:RebootCacheCluster` | Cluster | 
|  [RemoveTagsFromResource](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RemoveTagsFromResource.html) | `elasticache:RemoveTagsFromResource` | \(Optional\) Cluster, snapshot | 
|  [ResetCacheParameterGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ResetCacheParameterGroup.html) | `elasticache:ResetCacheParameterGroup` | Parameter group | 
|  [RevokeCacheSecurityGroupIngress](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RevokeCacheSecurityGroupIngress.html) | `elasticache:RevokeCacheSecurityGroupIngress` | No Resource ARN: \* | 
|  [StartMigration](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_StartMigration.html) | `elasticache:StartMigration` | Replication group | 
|  [TestFailover](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_TestFailover.html) | `elasticache:TestFailover` | Replication group | 