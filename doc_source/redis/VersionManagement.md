# Upgrading engine versions<a name="VersionManagement"></a>

You can control if and when the protocol\-compliant software powering your cache cluster is upgraded to new versions that are supported by ElastiCache\. This level of control enables you to maintain compatibility with specific versions, test new versions with your application before deploying in production, and perform version upgrades on your own terms and timelines\.

Because version upgrades might involve some compatibility risk, they don't occur automatically\. You must initiate them\. 

You initiate engine version upgrades to your cluster or replication group by modifying it and specifying a new engine version\. For more information, see the following:
+ [Modifying an ElastiCache cluster](Clusters.Modify.md)
+ [Modifying a replication group](Replication.Modify.md)

## Upgrade considerations<a name="VersionManagement-upgrade-considerations"></a>

Consider the following when choosing to upgrade:
+ Engine version management is designed so that you can have as much control as possible over how patching occurs\. However, ElastiCache reserves the right to patch your cluster on your behalf in the unlikely event of a critical security vulnerability in the system or cache software\.
+ Beginning with Redis 6\.0, ElastiCache for Redis will offer a single version for each Redis OSS minor release, rather than offering multiple patch versions\.
+ Starting with Redis engine version 5\.0\.6, you can upgrade your cluster version with minimal downtime\. The cluster is available for reads during the entire upgrade and is available for writes for most of the upgrade duration, except during the failover operation which lasts a few seconds\.
+ You can also upgrade your ElastiCache clusters with versions earlier than 5\.0\.6\. The process involved is the same but may incur longer failover time during DNS propagation \(30s\-1m\)\. 
+ Beginning with Redis 7, ElastiCache for Redis supports switching between Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\)\.
+ The Amazon ElastiCache for Redis engine upgrade process is designed to make a best effort to retain your existing data and requires successful Redis replication\. 
+ When upgrading the engine, ElastiCache for Redis will terminate existing client connections\. To minimize downtime during engine upgrades, we recommend you implement [best practices for Redis clients](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/BestPractices.Clients.html) with error retries and exponential backoff and the best practices for [minimizing downtime during maintenance](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/BestPractices.MinimizeDowntime.html)\. 
+ You can't upgrade directly from Redis \(cluster mode disabled\) to Redis \(cluster mode enabled\) when you upgrade your engine\. The following procedure shows you how to upgrade from Redis \(cluster mode disabled\) to Redis \(cluster mode enabled\)\.

**To upgrade from a Redis \(cluster mode disabled\) to Redis \(cluster mode enabled\) engine version**

  1. Make a backup of your Redis \(cluster mode disabled\) cluster or replication group\. For more information, see [Making manual backups](backups-manual.md)\.

  1. Use the backup to create and seed a Redis \(cluster mode enabled\) cluster with one shard \(node group\)\. Specify the new engine version and enable cluster mode when creating the cluster or replication group\. For more information, see [Seeding a new cluster with an externally created backup](backups-seeding-redis.md)\.

  1. Delete the old Redis \(cluster mode disabled\) cluster or replication group\. For more information, see [Deleting a cluster](Clusters.Delete.md) or [Deleting a replication group](Replication.DeletingRepGroup.md)\.

  1. Scale the new Redis \(cluster mode enabled\) cluster or replication group to the number of shards \(node groups\) that you need\. For more information, see [Scaling clusters in Redis \(Cluster Mode Enabled\)](scaling-redis-cluster-mode-enabled.md)
+ When upgrading major engine versions, for example from 5\.0\.6 to 6\.0, you need to also choose a new parameter group that is compatible with the new engine version\.
+ For single Redis clusters and clusters with Multi\-AZ disabled, we recommend that sufficient memory be made available to Redis as described in [Ensuring that you have enough memory to create a Redis snapshot](BestPractices.BGSAVE.md)\. In these cases, the primary is unavailable to service requests during the upgrade process\.
+ For Redis clusters with Multi\-AZ enabled, we also recommend that you schedule engine upgrades during periods of low incoming write traffic\. When upgrading to Redis 5\.0\.6 or above, the primary cluster continues to be available to service requests during the upgrade process\. 

  Clusters and replication groups with multiple shards are processed and patched as follows:
  + All shards are processed in parallel\. Only one upgrade operation is performed on a shard at any time\.
  + In each shard, all replicas are processed before the primary is processed\. If there are fewer replicas in a shard, the primary in that shard might be processed before the replicas in other shards are finished processing\.
  + Across all the shards, primary nodes are processed in series\. Only one primary node is upgraded at a time\.
+ If encryption is enabled on your current cluster or replication group, you cannot upgrade to an engine version that does not support encryption, such as from 3\.2\.6 to 3\.2\.10\.

## How to upgrade engine versions<a name="VersionManagement.HowTo"></a>

You initiate version upgrades to your cluster or replication group by modifying it using the ElastiCache console, the AWS CLI, or the ElastiCache API and specifying a newer engine version\. For more information, see the following topics\.


****  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/VersionManagement.html)

## Resolving blocked Redis engine upgrades<a name="resolving-blocked-engine-upgrades"></a>

As shown in the following table, your Redis engine upgrade operation is blocked if you have a pending scale up operation\.


****  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/VersionManagement.html)

**To resolve a blocked Redis engine upgrade**
+ Do one of the following:
  + Schedule your Redis engine upgrade operation for the next maintenance window by clearing the **Apply immediately** check box\. 

    With the CLI, use `--no-apply-immediately`\. With the API, use `ApplyImmediately=false`\.
  + Wait until your next maintenance window \(or after\) to perform your Redis engine upgrade operation\.
  + Add the Redis scale up operation to this cluster modification with the **Apply Immediately** check box chosen\. 

    With the CLI, use `--apply-immediately`\. With the API, use `ApplyImmediately=true`\. 

    This approach effectively cancels the engine upgrade during the next maintenance window by performing it immediately\.