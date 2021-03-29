# Redis nodes and shards<a name="CacheNodes.NodeGroups"></a>

A shard \(in the API and CLI, a node group\) is a hierarchical arrangement of nodes, each wrapped in a cluster\. Shards support replication\. Within a shard, one node functions as the read/write primary node\. All the other nodes in a shard function as read\-only replicas of the primary node\. Redis version 3\.2 and later support multiple shards within a cluster \(in the API and CLI, a replication group\)\. This support enables partitioning your data in a Redis \(cluster mode enabled\) cluster\. 

The following diagram illustrates the differences between a Redis \(cluster mode disabled\) cluster and a Redis \(cluster mode enabled\) cluster\.

![\[Image: Redis (cluster mode disabled) & Redis (cluster mode enabled) shards (API/CLI: node groups)\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-NodeGroups.png)

Both Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\) support replication via shards\. The API operation [DescribeReplicationGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReplicationGroups.html) \(CLI: [describe\-replication\-groups](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-replication-groups.html)\) lists the node groups with the member nodes, the node's role within the node group, and also other information\.

When you create a Redis cluster, you specify whether you want to create a cluster with clustering enabled\. Redis \(cluster mode disabled\) clusters never have more than one shard, which can be scaled horizontally by adding \(up to a total of five\) or deleting read replica nodes\. For more information, see [High availability using replication groups](Replication.md), [Adding a read replica, for Redis \(Cluster Mode Disabled\) replication groups](Replication.AddReadReplica.md) or [Deleting a read replica, for Redis \(Cluster Mode Disabled\) replication groups ](Replication.RemoveReadReplica.md)\. Redis \(cluster mode disabled\) clusters can also scale vertically by changing node types\. For more information, see [Scaling Redis \(Cluster Mode Disabled\) clusters with replica nodes](Scaling.RedisReplGrps.md)\.

When you create a Redis \(cluster mode enabled\) cluster, you specify from 1 to 250 shards\. 

The node or shard limit can be increased to a maximum of 500 per cluster if the Redis engine version is 5\.0\.6 or higher\. For example, you can choose to configure a 500 node cluster that ranges between 83 shards \(one primary and 5 replicas per shard\) and 500 shards \(single primary and no replicas\)\. Make sure there are enough available IP addresses to accommodate the increase\. Common pitfalls include the subnets in the subnet group have too small a CIDR range or the subnets are shared and heavily used by other clusters\. For more information, see [Creating a subnet group](SubnetGroups.Creating.md)\.

 For versions below 5\.0\.6, the limit is 250 per cluster\.

To request a limit increase, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) and choose the limit type **Nodes per cluster per instance type**\. 

After a Redis \(cluster mode enabled\) cluster is created, it can be altered \(scaled in or out\)\. For more information, see [Scaling ElastiCache for Redis clusters](Scaling.md) and [Replacing nodes](CacheNodes.NodeReplacement.md)\. 

When you create a new cluster, you can seed it with data from the old cluster so it doesn't start out empty\. This approach works only if the cluster group has the same number of shards as the old cluster\. Doing this can be helpful if you need change your node type or engine version\. For more information, see [Making manual backups](backups-manual.md) and [Restoring from a backup with optional cluster resizing](backups-restoring.md)\.