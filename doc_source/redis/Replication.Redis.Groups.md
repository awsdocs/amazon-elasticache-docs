# Understanding Redis Replication<a name="Replication.Redis.Groups"></a>

Redis implements replication in two ways: 
+ With a single shard that contains all of the cluster's data in each node—Redis \(cluster mode disabled\)
+ With data partitioned across up to 90 shards—Redis \(cluster mode enabled\)

Each shard in a replication group has a single read/write primary node and up to 5 read\-only replica nodes\. You can create a cluster with higher number of shards and lower number of replicas totaling up to 90 nodes per cluster\. This cluster configuration can range from 90 shards and 0 replicas to 15 shards and 5 replicas, which is the maximum number of replicas allowed\.

**Note**  
The node or shard limit can be increased to a maximum of 250 per cluster\. To request a limit increase, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) and select limit type "Nodes per cluster per instance type”\. 

**Topics**
+ [Redis \(Cluster Mode Disabled\)](#Replication.Redis.Groups.Classic)
+ [Redis \(cluster mode enabled\)](#Replication.Redis.Groups.Cluster)

## Redis \(Cluster Mode Disabled\)<a name="Replication.Redis.Groups.Classic"></a>

A Redis \(cluster mode disabled\) cluster has a single shard, inside of which is a collection of Redis nodes; one primary read/write node and up to five secondary, read\-only replica nodes\. Each read replica maintains a copy of the data from the cluster's primary node\. Asynchronous replication mechanisms are used to keep the read replicas synchronized with the primary\. Applications can read from any node in the cluster\. Applications can write only to the primary node\. Read replicas improve read throughput and guard against data loss in cases of a node failure\.

![\[Image: Redis (cluster mode disabled) cluster with a single shard and replica nodes\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCacheClusters-CSN-Redis-Replicas.png)

*Redis \(cluster mode disabled\) cluster with a single shard and replica nodes*

You can use Redis \(cluster mode disabled\) clusters with replica nodes to scale your Redis solution for ElastiCache to handle applications that are read\-intensive or to support large numbers of clients that simultaneously read from the same cluster\.

All of the nodes in a Redis \(cluster mode disabled\) cluster must reside in the same region\. To improve fault tolerance, you can provision read replicas in multiple Availability Zones within that region\.

When you add a read replica to a cluster, all of the data from the primary is copied to the new node\. From that point on, whenever data is written to the primary, the changes are asynchronously propagated to all the read replicas\.

To improve fault tolerance and reduce write downtime, enable Multi\-AZ with Automatic Failover for your Redis \(cluster mode disabled\) cluster with replicas\. For more information, see [Minimizing Downtime: Multi\-AZ with Automatic Failover](AutoFailover.md)\.

You can change the roles of the nodes within the Redis \(cluster mode disabled\) cluster, with the primary and one of the replicas exchanging roles\. You might decide to do this for performance tuning reasons\. For example, with a web application that has heavy write activity, you can choose the node that has the lowest network latency\. For more information, see [Promoting a Read Replica to Primary, for Redis \(cluster mode disabled\) Replication Groups](Replication.PromoteReplica.md)\.

## Redis \(cluster mode enabled\)<a name="Replication.Redis.Groups.Cluster"></a>

A Redis \(cluster mode enabled\) cluster is comprised of from 1 to 90 shards \(API/CLI: node groups\)\. Each shard has a primary node and up to five read\-only replica nodes\. The configuration can range from 90 shards and 0 replicas to 15 shards and 5 replicas, which is the maximum number or replicas allowed\. Each read replica in a shard maintains a copy of the data from the shard's primary\. Asynchronous replication mechanisms are used to keep the read replicas synchronized with the primary\. Applications can read from any node in the cluster\. Applications can write only to the primary nodes\. Read replicas enhance read scalability and guard against data loss\. Data is partitioned across the shards in a Redis \(cluster mode enabled\) cluster\.

**Note**  
The node/shard limit can be increased to a maximum of 250 per cluster\. To request a limit increase, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) and include the instance type in the request\.

Applications use the Redis \(cluster mode enabled\) cluster's *configuration endpoint* to connect with the nodes in the cluster\. For more information, see [Finding Connection Endpoints](Endpoints.md)\.

![\[Image: Redis (cluster mode enabled) cluster with multiple shards and replica nodes\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCacheClusters-CSN-RedisClusters.png)

*Redis \(cluster mode enabled\) cluster with multiple shards and replica nodes*

All of the nodes in a Redis \(cluster mode enabled\) cluster must reside in the same region\. To improve fault tolerance, you can provision both primaries and read replicas in multiple Availability Zones within that region\.

Multi\-AZ with Automatic Failover is required for all Redis \(cluster mode enabled\) clusters\. For more information, see [Minimizing Downtime: Multi\-AZ with Automatic Failover](AutoFailover.md)\.

Currently, in Redis \(cluster mode enabled\), there are some limitations\.
+ You cannot manually promote any of the replica nodes to primary\.
+ Multi\-AZ with Automatic Failover is required\.
+ You can only change the structure of a cluster, the node type, and the number of nodes by restoring from a backup\. For more information, see [Restoring From a Backup with Optional Cluster Resizing](backups-restoring.md)\. The number of shards in a Redis \(cluster mode enabled\) cluster can be changed dynamically, while the cluster continues to serve read and write requests\. For more information, see [Online Resharding and Shard Rebalancing for Redis \(cluster mode enabled\)](redis-cluster-resharding-online.md)\.