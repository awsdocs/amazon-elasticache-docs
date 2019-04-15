# Scaling Redis \(cluster mode disabled\) Clusters with Replica Nodes<a name="Scaling.RedisReplGrps"></a>

A Redis cluster with replica nodes \(called *replication group* in the API/CLI\) provides high availability via replication that has Multi\-AZ with automatic failover enabled\. A cluster with replica nodes is a logical collection of up to six Redis clusters where one node, the Primary, is able to serve both read and write requests\. All the other nodes in the cluster are read\-only replicas of the Primary\. Data written to the Primary is asynchronously replicated to all the read replicas in the cluster\. Because Redis \(cluster mode disabled\) does not support partitioning your data across multiple clusters, each node in a Redis \(cluster mode disabled\) replication group contains the entire cache dataset\. Redis \(cluster mode enabled\) clusters support partitioning your data across up to 90 shards\.

To change the data capacity of your cluster you must scale it up to a larger node type, or down to a smaller node type\.

To change the read capacity of your cluster, add more read replicas, up to a maximum of 5, or remove read replicas\.

The ElastiCache scaling up process is designed to make a best effort to retain your existing data and requires successful Redis replication\. For Redis clusters with replicas, we recommend that sufficient memory be made available to Redis as described in the topic [Ensuring You Have Sufficient Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)\. 

 The scaling down process is completely manual and makes no attempt at data retention other than what you do\.

**Related Topics**
+ [High Availability Using Replication Groups](Replication.md)
+ [Replication: Redis \(cluster mode disabled\) vs\. Redis \(cluster mode enabled\)](Replication.Redis-RedisCluster.md)
+ [Minimizing Downtime: Multi\-AZ with Automatic Failover](AutoFailover.md)
+ [Ensuring You Have Sufficient Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)

**Topics**
+ [Scaling Up Redis Clusters with Replicas](Scaling.RedisReplGrps.ScaleUp.md)
+ [Scaling Down Redis Clusters with Replicas](Scaling.RedisReplGrps.ScaleDown.md)
+ [Increasing Read Capacity](Scaling.RedisReplGrps.ScaleOut.md)
+ [Decreasing Read Capacity](Scaling.RedisReplGrps.ScaleIn.md)