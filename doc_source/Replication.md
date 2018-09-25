# High Availability Using Replication Groups<a name="Replication"></a>

Single\-node Amazon ElastiCache Redis clusters are in\-memory entities with limited data protection services \(AOF\)\. If your cluster fails for any reason, you lose all the cluster's data\. However, if you're running the Redis engine, you can group 2 to 6 nodes into a cluster with replicas where 1 to 5 read\-only nodes contain replicate data of the group's single read/write primary node\. In this scenario, if one node fails for any reason, you do not lose all your data since it is replicated in one or more other nodes\. Due to replication latency, some data may be lost if it is the primary read/write node that fails\.

As seen in the following graphic, the replication structure is contained within a shard \(called *node group* in the API/CLI\) which is contained within a Redis cluster\. Redis \(cluster mode disabled\) clusters always have one shard\. Redis \(cluster mode enabled\) clusters can have up to 15 shards with the cluster's data partitioned across the shards\.

![\[Image: Redis (cluster mode disabled) cluster has one shard and 1 to 5 replica nodes\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCacheClusters-CSN-Redis-Replicas.png)

*Redis \(cluster mode disabled\) cluster has one shard and 1 to 5 replica nodes*

If the cluster with replicas has Multi\-AZ with Automatic Failover enabled and the primary node fails, the primary fails over to a read replica\. Because the data is updated on the replica nodes asynchronously, there may be some data loss due to latency in updating the replica nodes\. For more information, see [Mitigating Failures when Running Redis](FaultTolerance.md#FaultTolerance.Redis)\.

**Topics**
+ [Understanding Redis Replication](Replication.Redis.Groups.md)
+ [Replication: Redis \(cluster mode disabled\) vs\. Redis \(cluster mode enabled\)](Replication.Redis-RedisCluster.md)
+ [Minimizing Downtime: Multi\-AZ with Automatic Failover](AutoFailover.md)
+ [How Synchronization and Backup are Implemented](Replication.Redis.Versions.md)
+ [Creating a Redis Replication Group](Replication.CreatingRepGroup.md)
+ [Viewing a Replication Group's Details](Replication.ViewDetails.md)
+ [Finding Replication Group Endpoints](Replication.Endpoints.md)
+ [Modifying a Replication Group](Replication.Modify.md)
+ [Deleting a Replication Group](Replication.DeletingRepGroup.md)
+ [Changing the Number of Replicas](increase-decrease-replica-count.md)
+ [Promoting a Read Replica to Primary, for Redis \(cluster mode disabled\) Replication Groups](Replication.PromoteReplica.md)