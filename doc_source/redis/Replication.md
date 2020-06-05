# High Availability Using Replication Groups<a name="Replication"></a>

Single\-node Amazon ElastiCache Redis clusters are in\-memory entities with limited data protection services \(AOF\)\. If your cluster fails for any reason, you lose all the cluster's data\. However, if you're running the Redis engine, you can group 2 to 6 nodes into a cluster with replicas where 1 to 5 read\-only nodes contain replicate data of the group's single read/write primary node\. In this scenario, if one node fails for any reason, you do not lose all your data since it is replicated in one or more other nodes\. Due to replication latency, some data may be lost if it is the primary read/write node that fails\.

As seen in the following graphic, the replication structure is contained within a shard \(called *node group* in the API/CLI\) which is contained within a Redis cluster\. Redis \(cluster mode disabled\) clusters always have one shard\. Redis \(cluster mode enabled\) clusters can have up to 90 shards with the cluster's data partitioned across the shards\. You can create a cluster with higher number of shards and lower number of replicas totaling up to 90 nodes per cluster\. This cluster configuration can range from 90 shards and 0 replicas to 15 shards and 5 replicas, which is the maximum number of replicas allowed\. 

**Note**  
The node or shard limit can be increased to a maximum of 250 per cluster\. To request a limit increase, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) and select limit type "Nodes per cluster per instance type”\. 

![\[Image: Redis (cluster mode disabled) cluster has one shard and 0 to 5 replica nodes\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCacheClusters-CSN-Redis-Replicas.png)

*Redis \(cluster mode disabled\) cluster has one shard and 0 to 5 replica nodes*

If the cluster with replicas has Multi\-AZ enabled and the primary node fails, the primary fails over to a read replica\. Because the data is updated on the replica nodes asynchronously, there may be some data loss due to latency in updating the replica nodes\. For more information, see [Mitigating Failures when Running Redis](FaultTolerance.md#FaultTolerance.Redis)\.

**Topics**
+ [Understanding Redis Replication](Replication.Redis.Groups.md)
+ [Replication: Redis \(Cluster Mode Disabled\) vs\. Redis \(Cluster Mode Enabled\)](Replication.Redis-RedisCluster.md)
+ [Minimizing Downtime in ElastiCache for Redis with Multi\-AZ](AutoFailover.md)
+ [How Synchronization and Backup are Implemented](Replication.Redis.Versions.md)
+ [Creating a Redis Replication Group](Replication.CreatingRepGroup.md)
+ [Viewing a Replication Group's Details](Replication.ViewDetails.md)
+ [Finding Replication Group Endpoints](Replication.Endpoints.md)
+ [Modifying a Replication Group](Replication.Modify.md)
+ [Deleting a Replication Group](Replication.DeletingRepGroup.md)
+ [Changing the Number of Replicas](increase-decrease-replica-count.md)
+ [Promoting a Read Replica to Primary, for Redis \(cluster mode disabled\) Replication Groups](Replication.PromoteReplica.md)