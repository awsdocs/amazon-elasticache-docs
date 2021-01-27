# Working with Shards<a name="Shards"></a>

A shard \(API/CLI: node group\) is a collection of one to six Redis nodes\. A Redis \(cluster mode disabled\) cluster will never have more than one shard\. Redis \(cluster mode enabled\) clusters can have from 1 to 250 shards\. You can create a cluster with higher number of shards and lower number of replicas totaling up to 90 nodes per cluster\. This cluster configuration can range from 90 shards and 0 replicas to 15 shards and 5 replicas, which is the maximum number of replicas allowed\. The cluster's data is partitioned across the cluster's shards\. If there is more than one node in a shard, the shard implements replication with one node being the read/write primary node and the other nodes read\-only replica nodes\.

The node or shard limit can be increased to a maximum of 500 per cluster if the Redis engine version is 5\.0\.6 or higher\. For example, you can choose to configure a 500 node cluster that ranges between 83 shards \(one primary and 5 replicas per shard\) and 500 shards \(single primary and no replicas\)\. Make sure there are enough available IP addresses to accommodate the increase\. Common pitfalls include the subnets in the subnet group have too small a CIDR range or the subnets are shared and heavily used by other clusters\. For more information, see [Creating a Subnet Group](SubnetGroups.Creating.md)\.

 For versions below 5\.0\.6, the limit is 250 per cluster\.

To request a limit increase, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) and choose the limit type **Nodes per cluster per instance type**\. 

When you create a Redis \(cluster mode enabled\) cluster using the ElastiCache console, you specify the number of shards in the cluster and the number of nodes in the shards\. For more information, see [Creating a Redis \(Cluster Mode Enabled\) Cluster \(Console\)](Clusters.Create.CON.RedisCluster.md)\. If you use the ElastiCache API or AWS CLI to create a cluster \(called *replication group* in the API/CLI\), you can configure the number of nodes in a shard \(API/CLI: node group\) independently\. For more information, see the following: 
+ API: [CreateReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)
+ CLI: [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

Each node in a shard has the same compute, storage and memory specifications\. The ElastiCache API lets you control shard\-wide attributes, such as the number of nodes, security settings, and system maintenance windows\.

![\[Image: Redis shard configurations.\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCacheClusters-CSN-RedisShards.png)

*Redis shard configurations*