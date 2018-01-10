# Mitigating Failures<a name="FaultTolerance"></a>

When planning your Amazon ElastiCache implementation, you should plan so that failures have a minimal impact upon your application and data\. The topics in this section cover approaches you can take to protect your application and data from failures\.


+ [Mitigating Failures when Running Memcached](#FaultTolerance.Memcached)
+ [Mitigating Failures when Running Redis](#FaultTolerance.Redis)
+ [Recommendations](#FaultTolerance.Recommendations)

## Mitigating Failures when Running Memcached<a name="FaultTolerance.Memcached"></a>

When running the Memcached engine, you have the following options for minimizing the impact of a failure\. There are two types of failures to address in your failure mitigation plans: node failure and Availability Zone failure\.

### Mitigating Node Failures<a name="FaultTolerance.Memcached.Node"></a>

To mitigate the impact of a node failure, spread your cached data over more nodes\. Because Memcached does not support replication, a node failure will always result in some data loss from your cluster\.

When you create your Memcached cluster you can create it with 1 to 20 nodes, or more by special request\. Partitioning your data across a greater number of nodes means you'll lose less data if a node fails\. For example, if you partition your data across 10 nodes, any single node stores approximately 10% of your cached data\. In this case, a node failure loses approximately 10% of your cache which needs to be replaced when a replacement node is created and provisioned\. If the same data were cached in 3 larger nodes, the failure of a node would lose approximately 33% of your cached data\.

If you need more than 20 nodes in a Memcached cluster, or more than 100 nodes total in a region, please fill out the ElastiCache Limit Increase Request form at [https://aws\.amazon\.com/contact\-us/elasticache\-node\-limit\-request/](https://aws.amazon.com/contact-us/elasticache-node-limit-request/)\.

For information on specifying the number of nodes in a Memcached cluster, go to [Creating a Cluster \(Console\): Memcached](Clusters.Create.CON.Memcached.md)\.

### Mitigating Availability Zone Failures<a name="FaultTolerance.Memcached.AZ"></a>

To mitigate the impact of an Availability Zone failure, locate your nodes in as many Availability Zones as possible\. In the unlikely event of an AZ failure, you will lose the data cached in that AZ, not the data cached in the other AZs\.

**Why so many nodes?**  
*If my region has only 3 Availability Zones, why do I need more than 3 nodes since if an AZ fails I lose approximately one\-third of my data?*

This is an excellent question\. Remember that we’re attempting to mitigate two distinct types of failures, node and Availability Zone\. You’re right, if your data is spread across Availability Zones and one of the zones fails, you will lose only the data cached in that AZ, irrespective of the number of nodes you have\. However, if a node fails, having more nodes will reduce the proportion of data lost\.

There is no "magic formula" for determining how many nodes to have in your cluster\. You must weight the impact of data loss vs\. the likelihood of a failure vs\. cost, and come to your own conclusion\.

For information on specifying the number of nodes in a Memcached cluster, go to [Creating a Cluster \(Console\): Memcached](Clusters.Create.CON.Memcached.md)\.

For more information on regions and Availability Zones, go to [Choosing Regions and Availability Zones](RegionsAndAZs.md)\.

## Mitigating Failures when Running Redis<a name="FaultTolerance.Redis"></a>

When running the Redis engine, you have the following options for minimizing the impact of a node or Availability Zone failure\.

### Mitigating Node Failures<a name="FaultTolerance.Redis.Cluster"></a>

To mitigate the impact of Redis node failures, you have the following options:


+ [Mitigating Failures: Redis Append Only Files \(AOF\)](#FaultTolerance.Redis.Cluster.AOF)
+ [Mitigating Failures: Redis Replication Groups](#FaultTolerance.Redis.Cluster.Replication)

#### Mitigating Failures: Redis Append Only Files \(AOF\)<a name="FaultTolerance.Redis.Cluster.AOF"></a>

When AOF is enabled for Redis, whenever data is written to your Redis cluster, a corresponding transaction record is written to a Redis append only file \(AOF\)\. If your Redis process restarts, ElastiCache creates a replacement cluster and provisions it\. You can then run the AOF against the cluster to repopulate it with data\.

Some of the shortcomings of using Redis AOF to mitigate cluster failures are:

+ It is time consuming\.

  Creating and provisioning a cluster can take several minutes\. Depending upon the size of the AOF, running it against the cluster will add even more time during which your application cannot access your cluster for data, forcing it to hit the database directly\.

   

+ The AOF can get big\.

  Because every write to your cluster is written to a transaction record, AOFs can become very large, larger than the \.rdb file for the dataset in question\. Because ElastiCache relies on the local instance store, which is limited in size, enabling AOF can cause out\-of\-disk\-space issues\. You can avoid out\-of\-disk\-space issues by using a replication group with Multi\-AZ enabled\.

   

+ Using AOF cannot protect you from all failure scenarios\.

  For example, if a node fails due to a hardware fault in an underlying physical server, ElastiCache will provision a new node on a different server\. In this case, the AOF is not available and cannot be used to recover the data\.

For more information, see [Redis Append Only Files \(AOF\)](RedisAOF.md)\.

#### Mitigating Failures: Redis Replication Groups<a name="FaultTolerance.Redis.Cluster.Replication"></a>

A Redis replication group is comprised of a single primary node which your application can both read from and write to, and from 1 to 5 read\-only replica nodes\. Whenever data is written to the primary node it is also asynchronously updated on the read replica nodes\. 

**When a read replica fails**

1. ElastiCache detects the failed read replica\.

1. ElastiCache takes the failed node off line\.

1. ElastiCache launches and provisions a replacement node in the same AZ\.

1. The new node synchronizes with the Primary node\.

During this time your application can continue reading and writing using the other nodes\.

**Redis Multi\-AZ with Automatic Failover**  
You can enable Multi\-AZ with automatic failover on your Redis replication groups\. Whether you enable Multi\-AZ with auto failover or not, a failed Primary will be detected and replaced automatically\. How this takes place varies whether or not Multi\-AZ is or is not enabled\.

**When Multi\-AZ with Auto Failover is enabled**

1. ElastiCache detects the Primary node failure\.

1. ElastiCache promotes the read replica node with the least replication lag to primary node\.

1. The other replicas sync with the new primary node\.

1. ElastiCache spins up a read replica in the failed primary's AZ\.

1. The new node syncs with the newly promoted primary\.

Failing over to a replica node is generally faster than creating and provisioning a new Primary node\. This means your application can resume writing to your Primary node sooner than if Multi\-AZ were not enabled\.

For more information, see [Replication: Multi\-AZ with Automatic Failover \(Redis\)](AutoFailover.md)\.

**When Multi\-AZ with Auto Failover is disabled**

1. ElastiCache detects Primary failure\.

1. ElastiCache takes the Primary offline\.

1. ElastiCache creates and provisions a new Primary node to replace the failed Primary\.

1. ElastiCache syncs the new Primary with one of the existing replicas\.

1. When the sync is finished, the new node functions as the cluster's Primary node\.

During this process, steps 1 through 4, your application cannot write to the Primary node\. However, your application can continue reading from your replica nodes\.

For added protection, we recommend that you launch the nodes in your replication group in different Availability Zones \(AZs\)\. If you do this, an AZ failure will only impact the nodes in that AZ and not the others\.

For more information, see [ElastiCache Replication \(Redis\)](Replication.md)\.

### Mitigating Availability Zone Failures<a name="FaultTolerance.Redis.AZ"></a>

To mitigate the impact of an Availability Zone failure, locate your nodes in as many Availability Zones as possible\.

No matter how many nodes you have, if they are all located in the same Availability Zone, a catastrophic failure of that AZ results in your losing all your cache data\. However, if you locate your nodes in multiple AZs, a failure of any AZ results in your losing only the nodes in that AZ\. 

Any time you lose a node you can experience a performance degradation since read operations are now shared by fewer nodes\. This performance degradation will continue until the nodes are replaced\. Because your data is not partitioned across Redis nodes, you risk some data loss only when the primary node is lost\.

For information on specifying the Availability Zones for Redis nodes, go to [Creating a Redis \(cluster mode disabled\) Cluster \(Console\)](Clusters.Create.CON.Redis.md)\.

For more information on regions and Availability Zones, go to [Choosing Regions and Availability Zones](RegionsAndAZs.md)\.

## Recommendations<a name="FaultTolerance.Recommendations"></a>

There are two types of failures you need to plan for, individual node failures and broad Availability Zone failures\. The best failure mitigation plan will address both kinds of failures\.

### Minimizing the Impact of Failures<a name="FaultTolerance.Recommendations.NodeFailure"></a>

To minimize the impact of a node failure, we recommend that your implementation use multiple nodes in each shard and distribute the nodes across multiple availability zones\.

If you're running Memcached and partitioning your data across nodes, the more nodes you use the smaller the data loss if any one node fails\.

If you’re running Redis, we also recommend that you enable Multi\-AZ on your replication group so that ElastiCache will automatically fail over to a replica if the primary node fails\.

### Minimizing the Impact of Availability Zone Failures<a name="FaultTolerance.Recommendations.AZFailure"></a>

To minimize the impact of an availability zone failure, we recommend launching your nodes in as many different availability zones as are available\. Spreading your nodes evenly across AZs will minimize the impact in the unlikely event of an AZ failure\.

### Other precautions<a name="FaultTolerance.Recommendations.Other"></a>

If you're running Redis, then in addition to the above, we recommend that you schedule regular backups of your cluster\. Backups \(snapshots\) create a \.rdb file you can use to restore your cluster in case of failure or corruption\. For more information, see [ElastiCache Backup and Restore \(Redis\)](backups.md)\.