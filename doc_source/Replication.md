# ElastiCache Replication \(Redis\)<a name="Replication"></a>

Single\-node Amazon ElastiCache Redis clusters are in\-memory entities with limited data protection services \(AOF\)\. if your cluster fails for any reason, you risk lose all the cluster's data\. However, if you're running the Redis engine, you can group 2 to 6 nodes into a cluster with replicas where 1 to 5 read\-only nodes contain replicate data of the group's single read/write primary node\. In this scenario, if one node fails for any reason you do not lose all your data since it is replicated in one or more other nodes\.

As seen in the following graphic, the replication structure is contained within a shard \(called *node group* in the API/CLI\) which is contained within a Redis cluster\. Redis \(cluster mode disabled\) clusters always have one shard\. Redis \(cluster mode enabled\) clusters can have up to 15 shards with the cluster's data partitioned across the shards\.

![\[Image: Redis (cluster mode disabled) cluster has one shard and 1 to 5 replica nodes\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCacheClusters-CSN-Redis-Replicas.png)

*Redis \(cluster mode disabled\) cluster has one shard and 1 to 5 replica nodes*

If the cluster with replicas has Multi\-AZ with automatic failover enabled and the primary node fails, the primary fails over to a read replica\. Because the data is updated on the replica nodes asynchronously, there may be some data loss due to latency in updating the replica nodes\. For more information, see [Mitigating Failures when Running Redis](FaultTolerance.md#FaultTolerance.Redis)\.


+ [Redis Replication](Replication.Redis.Groups.md)
+ [Replication: Redis \(cluster mode disabled\) vs\. Redis \(cluster mode enabled\)](Replication.Redis-RedisCluster.md)
+ [Replication: Multi\-AZ with Automatic Failover \(Redis\)](AutoFailover.md)
+ [How Synchronization and Backup are Implemented](Replication.Redis.Versions.md)
+ [Creating a Redis Cluster with Replicas](Replication.CreatingRepGroup.md)
+ [Viewing a Replication Group's Details](Replication.ViewDetails.md)
+ [Finding Replication Group Endpoints](Replication.Endpoints.md)
+ [Modifying a Cluster with Replicas](Replication.Modify.md)
+ [Deleting a Cluster with Replicas](Replication.DeletingRepGroup.md)
+ [Adding a Read Replica to a Redis Cluster](Replication.AddReadReplica.md)
+ [Promoting a Read\-Replica to Primary](Replication.PromoteReplica.md)
+ [Deleting a Read Replica](Replication.RemoveReadReplica.md)