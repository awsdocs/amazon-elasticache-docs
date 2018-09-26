# Event Notifications and Amazon SNS<a name="ElastiCacheSNS"></a>

ElastiCache can publish messages using Amazon Simple Notification Service \(SNS\) when significant events happen on a cache cluster\. This feature can be used to refresh the server\-lists on client machines connected to individual cache node endpoints of a cache cluster\.

**Note**  
For more information on Amazon Simple Notification Service \(SNS\), including information on pricing and links to the Amazon SNS documentation, see the [Amazon SNS product page](https://aws.amazon.com/sns)\.

Notifications are published to a specified Amazon SNS *topic*\. The following are requirements for notifications:
+ Only one topic can be configured for ElastiCache notifications\.
+ The AWS account that owns the Amazon SNS topic must be the same account that owns the cache cluster on which notifications are enabled\.

## Example ElastiCache SNS Notification<a name="ElastiCache.SNS.Sample"></a>

The following example shows an ElastiCache Amazon SNS notification for successfully creating a cache cluster\.

**Example**  

```
{
    "Date": "2015-12-05T01:02:18.336Z",
    "Message": "Cache cluster created",
    "SourceIdentifier": "memcache-ni",
    "SourceType": "cache-cluster"
}
```

## ElastiCache Events<a name="ElastiCacheSNS.Events"></a>

The following ElastiCache events trigger Amazon SNS notifications:


| Event Name | Message | Description | 
| --- | --- | --- | 
|  ElastiCache:AddCacheNodeComplete  |  "Finished modifying number of nodes from %d to %d"  |  A cache node has been added to the cache cluster and is ready for use\.  | 
|  ElastiCache:AddCacheNodeFailed due to insufficient free IP addresses  |  "Failed to modify number of nodes from %d to %d due to insufficient free IP addresses"  |  A cache node could not be added because there are not enough available IP addresses\.  | 
|  ElastiCache:CacheClusterParametersChanged  |  "Updated parameter %s to %s" In case of create, also send `"Updated to use a CacheParameterGroup %s"`  |  One or more cache cluster parameters have been changed\.  | 
|  ElastiCache:CacheClusterProvisioningComplete  |  "Cache cluster created"  |  The provisioning of a cache cluster is completed, and the cache nodes in the cache cluster are ready to use\.  | 
|  ElastiCache:CacheClusterProvisioningFailed due to incompatible network state  |  "Failed to create the cache cluster due to incompatible network state"  |  An attempt was made to launch a new cache cluster into a nonexistent virtual private cloud \(VPC\)\.  | 
| ElastiCache:CacheClusterScalingComplete  | `"Succeeded applying modification to cache node type to %s."` | : Scale up for cache\-cluster completed successfully\. | 
| ElastiCache:CacheClusterScalingFailed | `"Failed applying modification to cache node type to %s."` | Scale\-up operation on cache\-cluster failed\.  | 
|  ElastiCache:CacheClusterSecurityGroupModified  |  "Applied change to security group"  |  One of the following events has occurred: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/ElastiCacheSNS.html)  | 
|  ElastiCache:CacheNodeReplaceComplete  |  "Finished recovery for cache nodes %s"  |  ElastiCache has detected that the host running a cache node is degraded or unreachable and has completed replacing the cache node\.  The DNS entry for the replaced cache node is not changed\.  In most instances, you do not need to refresh the server\-list for your clients when this event occurs\. However, some cache client libraries may stop using the cache node even after ElastiCache has replaced the cache node; in this case, the application should refresh the server\-list when this event occurs\.  | 
|  ElastiCache:CacheNodesRebooted  |  "Cache node %s restarted"  |  One or more cache nodes has been rebooted\. Message \(Memcached\): `"Cache node %s shutdown"` Then a second message: `"Cache node %s restarted"`  | 
|  ElastiCache:DeleteCacheClusterComplete  |  "Cache cluster deleted"  |  The deletion of a cache cluster and all associated cache nodes has completed\.  | 
| ElastiCache:FailoverComplete | `"Failover to replica node %s completed"` | Failover over to a replica node was successful\. | 
|  ElastiCache:NodeReplacementCanceled  |  "The replacement for Cache Cluster ID: %s, Node ID: %s scheduled during the maintenance window from Start Time: %s, End Time: %s has been canceled"  |  A node in your cluster that was scheduled for replacement is no longer scheduled for replacement\.   | 
|  ElastiCache:NodeReplacementRescheduled  |  "The replacement in maintenance window for node with Cache Cluster ID: %s, Node ID: %s has re\-scheduled from Previous Start Time: %s, Previous End Time: %s to New Start Time: %s, New End Time: %s""  |  A node in your cluster previously scheduled for replacement has been rescheduled for replacement during the new window described in the notification\.  For information on what actions you can take, see [Replacing Nodes](CacheNodes.NodeReplacement.md)\.  | 
|  ElastiCache:NodeReplacementScheduled  |  "The node with Cache Cluster ID: %s, Node ID: %s is scheduled for replacement during the maintenance window from Start Time: %s, End Time: %s"  |  A node in your cluster is scheduled for replacement during the window described in the notification\.  For information on what actions you can take, see [Replacing Nodes](CacheNodes.NodeReplacement.md)\.  | 
|  ElastiCache:RemoveCacheNodeComplete  |  "Removed cache nodes %s"  |  A cache node has been removed from the cache cluster\.  | 
|  ElastiCache:SnapshotComplete  |  "Snapshot succeeded for snapshot with ID '%s' of cache cluster with ID '%s'"  |  A cache snapshot has completed successfully\.  | 
|  ElastiCache:SnapshotFailed  |  "Snapshot failed for snapshot with ID '%s' of cache cluster with ID '%s'"  |  A cache snapshot has failed\. See the cluster’s cache events for more a detailed cause\. If you describe the snapshot, see [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeSnapshots.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeSnapshots.html), the status will be `failed`\.  | 

## Related topics<a name="ElastiCacheSNS.SeeAlso"></a>
+ [Viewing ElastiCache Events](ECEvents.Viewing.md)