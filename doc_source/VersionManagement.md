# Upgrading Engine Versions<a name="VersionManagement"></a>

You can control if and when the protocol\-compliant software powering your cache cluster is upgraded to new versions that are supported by ElastiCache\. This level of control enables you to maintain compatibility with specific Memcached or Redis versions, test new versions with your application before deploying in production, and perform version upgrades on your own terms and timelines\.

Because version upgrades might involve some compatibility risk, they don't occur automatically\. You must initiate them\. 

You initiate version upgrades to your cluster or replication group by modifying it and specifying a new engine version\. For more information, see [Modifying an ElastiCache Cluster](Clusters.Modify.md) or [Modifying a Cluster with Replicas](Replication.Modify.md)\.

**Important**  
You can upgrade to a newer engine version, but you can’t downgrade to an older engine version\. If you want to use an older engine version, you must delete the existing cluster and create it anew with the older engine version\. 
Although engine version management functionality is intended to give you as much control as possible over how patching occurs, ElastiCache reserves the right to patch your cluster on your behalf in the unlikely event of a critical security vulnerability in the system or cache software\.
Redis \(cluster mode enabled\) does not support changing engine versions\.
ElastiCache does not support switching between cluster enabled and cluster disabled\.

## Important Notes on Memcached Engine Upgrades<a name="VersionManagement.Memcached"></a>

Because the Memcached engine does not support persistence, Memcached engine version upgrades are always a disruptive process which clears all cache data in the cluster\.

## Important Notes on Redis Engine Upgrades<a name="VersionManagement.Redis"></a>

The Amazon ElastiCache engine upgrade process is designed to make a best effort to retain your existing data and requires successful Redis replication\. 

**Important**  
If you want to upgrade your engine from Redis 2\.x to Redis 3\.x you can do so, but you cannot upgrade from Redis \(cluster mode disabled\) to Redis \(cluster mode enabled\)\. To upgrade to Redis \(cluster mode enabled\), you must create a new Redis \(cluster mode enabled\) cluster\. You can seed this new cluster using a Redis \(cluster mode disabled\) snapshot if both the old and new clusters have the same number of shards \(API/CLI: node groups\)\.

+ For single Redis clusters and clusters with Multi\-AZ disabled, we recommend that sufficient memory be made available to Redis as described in [Ensuring You Have Sufficient Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)\. Please note that in these cases, the primary is unavailable to service requests during the upgrade process\.

+ For Redis clusters with Multi\-AZ enabled, in addition to the preceding, we also recommend that you schedule engine upgrades during periods of low incoming write traffic\. The primary continues to be available to service requests during the upgrade process, except for a few minutes when a failover is initiated\.

**Blocked Redis Engine Upgrades**  
As shown in the following table, your Redis engine upgrade operation is blocked if you have a pending scale up operation\.


****  

|  Pending Operations  |  Blocked Operations  | 
| --- | --- | 
| Scale up | Immediate engine upgrade | 
| Engine upgrade | Immediate scale up | 
| Scale up and engine upgrade | Immediate scale up | 
| Scale up and engine upgrade | Immediate engine upgrade | 

**To resolve a blocked engine upgrade, do one of the following**

+ Schedule your Redis engine upgrade operation for the next maintenance window by clearing the **Apply immediately** check box \(CLI use: `--no-apply-immediately`, API use: `ApplyImmediately=false`\)\.

   

+ Wait until your next maintenance window \(or after\) to perform your Redis engine upgrade operation\.

   

+ Add the Redis scale up operation to this cluster modification with the **Apply Immediately** check box chosen \(CLI use: `--apply-immediately`, API use: `ApplyImmediately=true`\)\. \(This effectively cancels the engine upgrade during the next maintenance window by performing it immediately\.\)

## How to Upgrade Engine Versions<a name="VersionManagement.HowTo"></a>

You initiate version upgrades to your cluster or replication group by modifying it using the ElastiCache console, the AWS CLI, or the ElastiCache API and specifying a newer engine version\. For more information, see the following topics\.

**Important**  
Remember, for Redis \(cluster mode enabled\) you cannot modify clusters or replication groups\.


****  

|  | Clusters | Replication Groups | 
| --- | --- | --- | 
| Using the console | [Modifying a Cluster \(Console\)](Clusters.Modify.md#Clusters.Modify.CON) | [Modifying a Redis Cluster \(Console\)](Replication.Modify.md#Replication.Modify.CON) | 
| Using the AWS CLI | [Modifying a Cache Cluster \(AWS CLI\)](Clusters.Modify.md#Clusters.Modify.CLI) | [Modifying a Replication Group \(AWS CLI\)](Replication.Modify.md#Replication.Modify.CLI) | 
| Using the ElastiCache API | [Modifying a Cache Cluster \(ElastiCache API\)](Clusters.Modify.md#Clusters.Modify.API) | [Modifying a Replication Group \(ElastiCache API\)](Replication.Modify.md#Replication.Modify.API) | 