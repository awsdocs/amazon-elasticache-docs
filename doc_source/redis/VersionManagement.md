# Upgrading Engine Versions<a name="VersionManagement"></a>

You can control if and when the protocol\-compliant software powering your cache cluster is upgraded to new versions that are supported by ElastiCache\. This level of control enables you to maintain compatibility with specific versions, test new versions with your application before deploying in production, and perform version upgrades on your own terms and timelines\.

Because version upgrades might involve some compatibility risk, they don't occur automatically\. You must initiate them\. 

You initiate engine version upgrades to your cluster or replication group by modifying it and specifying a new engine version\. For more information, see the following:
+ [Modifying an ElastiCache Cluster](Clusters.Modify.md)
+ [Modifying a Replication Group](Replication.Modify.md)

**Important**  
You can upgrade to a newer engine version, but you can't downgrade to an older engine version\. If you want to use an older engine version, you must delete the existing cluster and create it anew with the older engine version\. 
Engine version management is designed so that you can have as much control as possible over how patching occurs\. However, ElastiCache reserves the right to patch your cluster on your behalf in the unlikely event of a critical security vulnerability in the system or cache software\.
Starting with Redis engine version 5\.0\.5, you can upgrade your cluster version with minimal downtime\. The cluster is available for reads during the entire upgrade and is available for writes for most of the upgrade duration, except during the failover operation which lasts a few seconds\.
You can also upgrade your ElastiCache clusters with versions earlier than 5\.0\.5\. The process involved is the same but may incur longer failover time during DNS propagation \(30s\-1m\)\. 
ElastiCache for Redis doesn't support switching between Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\)\.
The Amazon ElastiCache for Redis engine upgrade process is designed to make a best effort to retain your existing data and requires successful Redis replication\. 
You can't upgrade directly from Redis \(cluster mode disabled\) to Redis \(cluster mode enabled\) when you upgrade your engine\. The following procedure shows you how to upgrade from Redis \(cluster mode disabled\) to Redis \(cluster mode enabled\)\.  
Make a backup of your Redis \(cluster mode disabled\) cluster or replication group\. For more information, see [Making Manual Backups](backups-manual.md)\.
Use the backup to create and seed a Redis \(cluster mode enabled\) cluster with one shard \(node group\)\. Specify the new engine version and enable cluster mode when creating the cluster or replication group\. For more information, see [Seeding a New Cluster with an Externally Created Backup](backups-seeding-redis.md)\.
Delete the old Redis \(cluster mode disabled\) cluster or replication group\. For more information, see [Deleting a Cluster](Clusters.Delete.md) or [Deleting a Replication Group](Replication.DeletingRepGroup.md)\.
Scale the new Redis \(cluster mode enabled\) cluster or replication group to the number of shards \(node groups\) that you need\. For more information, see [Scaling Clusters in Redis \(Cluster Mode Enabled\)](scaling-redis-cluster-mode-enabled.md)
For single Redis clusters and clusters with Multi\-AZ disabled, we recommend that sufficient memory be made available to Redis as described in [Ensuring That You Have Enough Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)\. In these cases, the primary is unavailable to service requests during the upgrade process\.
For Redis clusters with Multi\-AZ enabled, we also recommend that you schedule engine upgrades during periods of low incoming write traffic\. When upgrading to Redis 5\.0\.5 or above, the primary cluster continues to be available to service requests during the upgrade process\. When upgrading to Redis 5\.0\.4 or below, you may notice a brief interruption of a few seconds associated with the DNS update\.  
Clusters and replication groups with multiple shards are processed and patched as follows:  
All shards are processed in parallel\. Only one upgrade operation is performed on a shard at any time\.
In each shard, all replicas are processed before the primary is processed\. If there are fewer replicas in a shard, the primary in that shard might be processed before the replicas in other shards are finished processing\.
Across all the shards, primary nodes are processed in series\. Only one primary node is upgraded at a time\.
If encryptions is enabled on your current cluster or replication group, you cannot upgrade to an engine version that does not support encryption, such as from 3\.2\.6 to 3\.2\.10\.

## How to Upgrade Engine Versions<a name="VersionManagement.HowTo"></a>

You initiate version upgrades to your cluster or replication group by modifying it using the ElastiCache console, the AWS CLI, or the ElastiCache API and specifying a newer engine version\. For more information, see the following topics\.


****  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/VersionManagement.html)

## Resolving Blocked Redis Engine Upgrades<a name="resolving-blocked-engine-upgrades"></a>

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