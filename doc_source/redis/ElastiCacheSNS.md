# Event Notifications and Amazon SNS<a name="ElastiCacheSNS"></a>

ElastiCache can publish messages using Amazon Simple Notification Service \(SNS\) when significant events happen on a cache cluster\. This feature can be used to refresh the server\-lists on client machines connected to individual cache node endpoints of a cache cluster\.

**Note**  
For more information on Amazon Simple Notification Service \(SNS\), including information on pricing and links to the Amazon SNS documentation, see the [Amazon SNS product page](http://aws.amazon.com/sns)\.

Notifications are published to a specified Amazon SNS *topic*\. The following are requirements for notifications:
+ Only one topic can be configured for ElastiCache notifications\.
+ The AWS account that owns the Amazon SNS topic must be the same account that owns the cache cluster on which notifications are enabled\.
+ The Amazon SNS topic you are publishing to cannot be encrypted\.
**Note**  
It is possible to attach an encrypted \(at\-rest\) Amazon SNS topic to the cluster\. However, the status of the topic from the ElastiCache console will show as inactive, which effectively disassociates the topic from the cluster when ElastiCache pushes messages to the topic\. 

## ElastiCache Events<a name="ElastiCacheSNS.Events"></a>

The following ElastiCache events trigger Amazon SNS notifications\. For information on event details, see [Viewing ElastiCache events](ECEvents.Viewing.md)\.


| Event Name | Message | Description | 
| --- | --- | --- | 
|  ElastiCache:AddCacheNodeComplete  |  ElastiCache:AddCacheNodeComplete : cache\-cluster  |  A cache node has been added to the cache cluster and is ready for use\.  | 
|  ElastiCache:AddCacheNodeFailed due to insufficient free IP addresses  |  ElastiCache:AddCacheNodeFailed : cluster\-name  |  A cache node could not be added because there are not enough available IP addresses\.  | 
|  ElastiCache:CacheClusterParametersChanged  |  ElastiCache:CacheClusterParametersChanged : cluster\-name  |  One or more cache cluster parameters have been changed\.  | 
|  ElastiCache:CacheClusterProvisioningComplete  |  ElastiCache:CacheClusterProvisioningComplete cluster\-name\-0001\-005  |  The provisioning of a cache cluster is completed, and the cache nodes in the cache cluster are ready to use\.  | 
|  ElastiCache:CacheClusterProvisioningFailed due to incompatible network state  |  ElastiCache:CacheClusterProvisioningFailed : cluster\-name  |  An attempt was made to launch a new cache cluster into a nonexistent virtual private cloud \(VPC\)\.  | 
| ElastiCache:CacheClusterScalingComplete  | `CacheClusterScalingComplete : cluster-name` | Scaling for cache\-cluster completed successfully\. | 
| ElastiCache:CacheClusterScalingFailed | ElastiCache:CacheClusterScalingFailed : *cluster\-name* | Scale\-up operation on cache\-cluster failed\.  | 
|  ElastiCache:CacheClusterSecurityGroupModified  |  ElastiCache:CacheClusterSecurityGroupModified : cluster\-name  |  One of the following events has occurred: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/ElastiCacheSNS.html)  | 
|  ElastiCache:CacheNodeReplaceStarted  |  ElastiCache:CacheNodeReplaceStarted : cluster\-name  |  ElastiCache has detected that the host running a cache node is degraded or unreachable and has started replacing the cache node\.  The DNS entry for the replaced cache node is not changed\.  In most instances, you do not need to refresh the server\-list for your clients when this event occurs\. However, some cache client libraries may stop using the cache node even after ElastiCache has replaced the cache node; in this case, the application should refresh the server\-list when this event occurs\.  | 
|  ElastiCache:CacheNodeReplaceComplete  |  ElastiCache:CacheNodeReplaceComplete : cluster\-name  |  ElastiCache has detected that the host running a cache node is degraded or unreachable and has completed replacing the cache node\.  The DNS entry for the replaced cache node is not changed\.  In most instances, you do not need to refresh the server\-list for your clients when this event occurs\. However, some cache client libraries may stop using the cache node even after ElastiCache has replaced the cache node; in this case, the application should refresh the server\-list when this event occurs\.  | 
|  ElastiCache:CacheNodesRebooted  |  ElastiCache:CacheNodesRebooted : cluster\-name  |  One or more cache nodes has been rebooted\. Message \(Memcached\): `"Cache node %s shutdown"` Then a second message: `"Cache node %s restarted"`  | 
|  ElastiCache:CertificateRenewalComplete \(Redis only\)  |  ElastiCache:CertificateRenewalComplete  |  The Amazon CA certificate was successfully renewed\.  | 
|  ElastiCache:CreateReplicationGroupComplete  |  ElastiCache:CreateReplicationGroupComplete : cluster\-name  |  The replication group was successfully created\.  | 
|  ElastiCache:DeleteCacheClusterComplete  |  ElastiCache:DeleteCacheClusterComplete : cluster\-name  |  The deletion of a cache cluster and all associated cache nodes has completed\.  | 
| ElastiCache:FailoverComplete \(Redis only\) | `ElastiCache:FailoverComplete : mycluster` | Failover over to a replica node was successful\.  | 
|  ElastiCache:ReplicationGroupIncreaseReplicaCountFinished  |  ElastiCache:ReplicationGroupIncreaseReplicaCountFinished : cluster\-name\-0001\-005  |  The number of replicas in the cluster has been increased\.   | 
|  ElastiCache:ReplicationGroupIncreaseReplicaCountStarted  |  ElastiCache:ReplicationGroupIncreaseReplicaCountStarted : cluster\-name\-0003\-004  |  The process of adding replicas to your cluster has begun\.   | 
|  ElastiCache:NodeReplacementCanceled  |  ElastiCache:NodeReplacementCanceled : cluster\-name  |  A node in your cluster that was scheduled for replacement is no longer scheduled for replacement\.   | 
|  ElastiCache:NodeReplacementRescheduled  |  ElastiCache:NodeReplacementRescheduled : cluster\-name  |  A node in your cluster previously scheduled for replacement has been rescheduled for replacement during the new window described in the notification\.  For information on what actions you can take, see [Replacing nodes](CacheNodes.NodeReplacement.md)\.  | 
|  ElastiCache:NodeReplacementScheduled  |  ElastiCache:NodeReplacementScheduled : cluster\-name  |  A node in your cluster is scheduled for replacement during the window described in the notification\.  For information on what actions you can take, see [Replacing nodes](CacheNodes.NodeReplacement.md)\.  | 
|  ElastiCache:RemoveCacheNodeComplete  |  ElastiCache:RemoveCacheNodeComplete : cluster\-name  |  A cache node has been removed from the cache cluster\.  | 
| ElastiCache:ReplicationGroupScalingComplete | `ElastiCache:ReplicationGroupScalingComplete : cluster-name` | Scale\-up operation on replication group completed successfully\.  | 
| ElastiCache:ReplicationGroupScalingFailed | `"Failed applying modification to cache node type to %s."` | Scale\-up operation on replication group failed\.  | 
| ElastiCache:ServiceUpdateAvailableForNode | `"Service update is available for cache node %s."` | A self\-service update is available for the node\.  | 
|  ElastiCache:SnapshotComplete \(Redis only\)  |  ElastiCache:SnapshotComplete : cluster\-name  |  A cache snapshot has completed successfully\.  | 
|  ElastiCache:SnapshotFailed \(Redis only\)  |  SnapshotFailed : cluster\-name  |  A cache snapshot has failed\. See the cluster’s cache events for more a detailed cause\. If you describe the snapshot, see [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeSnapshots.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeSnapshots.html), the status will be `failed`\.  | 

## Related topics<a name="ElastiCacheSNS.SeeAlso"></a>
+ [Viewing ElastiCache events](ECEvents.Viewing.md)