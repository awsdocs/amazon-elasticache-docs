# Shards<a name="Shards"></a>

A shard \(API/CLI: node group\) is a collection of one to six Redis nodes\. A Redis \(cluster mode disabled\) cluster will never have more than one shard\. Redis \(cluster mode enabled\) clusters can have from 1 to 15 shards\. The cluster's data is partitioned across the cluster's shards\. If there is more than one node in a shard, the shard implements replication with one node being the read/write primary node and the other nodes read\-only replica nodes\.

When you create a Redis \(cluster mode enabled\) cluster using the ElastiCache console, you specify the number of shards in the cluster and the number of nodes in the shards\. For more information, see [Creating a Redis \(cluster mode enabled\) Cluster \(Console\)](Clusters.Create.CON.RedisCluster.md)\. If you use the ElastiCache API or AWS CLI to create a cluster \(called *replication group* in the API/CLI\), you can configure the number of nodes in a shard \(API/CLI: node group\) independently\. For more information, see: 
+ API: [CreateReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)
+ CLI: [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

Each node in a shard has the same compute, storage and memory specifications\. The ElastiCache API lets you control shard\-wide attributes, such as the number of nodes, security settings, and system maintenance windows\.

![\[Image: Redis shard configurations.\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCacheClusters-CSN-RedisShards.png)

*Redis shard configurations*