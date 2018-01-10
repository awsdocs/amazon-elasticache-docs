# ElastiCache for Redis Versions<a name="SelectEngine.RedisVersions"></a>

If you enable at\-rest, in\-transit encryption, and Redis AUTH when you create a Redis cluster using ElastiCache for Redis version 3\.2\.6, you can use Amazon ElastiCache for Redis to build HIPAA\-compliant applications\. You can store healthcare\-related information, including protected health information \(PHI\), under an executed Business Associate Agreement \(BAA\) with AWS\. AWS Services in Scope have been fully assessed by a third\-party auditor and result in a certification, attestation of compliance, or Authority to Operate \(ATO\)\. For more information, see the following topics:

+ [AWS Cloud Compliance](https://aws.amazon.com/compliance/)

+ [HIPAA Compliance](https://aws.amazon.com/compliance/hipaa-compliance/)

+ [AWS Services in Scope by Compliance Program](https://aws.amazon.com/compliance/services-in-scope/)

+ [Amazon ElastiCache for Redis Data Encryption](encryption.md)

+ [Authenticating Users with AUTH \(Redis\)](auth.md)

Supported ElastiCache for Redis versions

**Note**  
Because the newer Redis versions provide a better and more stable user experience, Redis versions 2\.6\.13, 2\.8\.6, and 2\.8\.19 are deprecated when using the ElastiCache console\. We recommend against using these Redis versions\. If you need to use one of them, work with the AWS CLI or ElastiCache API\.  
For more information, see the following topics:  


|  | AWS CLI | ElastiCache API | 
| --- | --- | --- | 
| **Create Cluster** | [Creating a Cache Cluster \(AWS CLI\)](Clusters.Create.CLI.md) This action cannot be used to create a replication group with cluster mode enabled\. | [Creating a Cache Cluster \(ElastiCache API\)](Clusters.Create.API.md)  This action cannot be used to create a replication group with cluster mode enabled\.  | 
| **Modify Cluster** | [Modifying a Cache Cluster \(AWS CLI\)](Clusters.Modify.md#Clusters.Modify.CLI)  This action cannot be used to create a replication group with cluster mode enabled\.  | [Modifying a Cache Cluster \(ElastiCache API\)](Clusters.Modify.md#Clusters.Modify.API)  This action cannot be used to create a replication group with cluster mode enabled\. | 
| **Create Replication Group** | [Creating a Redis \(cluster mode disabled\) Cluster with Replicas from Scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.CLI) [Creating a Redis \(cluster mode enabled\) Cluster with Replicas from Scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.CLI)  | [Creating a Redis \(cluster mode disabled\) Cluster with Replicas from Scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.API) [Creating a Redis \(cluster mode enabled\) Cluster with Replicas from Scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.API) | 
| **Modify Replication Group** | [Modifying a Replication Group \(AWS CLI\)](Replication.Modify.md#Replication.Modify.CLI)  This action cannot be used to create a replication group with cluster mode enabled\. | [Modifying a Replication Group \(ElastiCache API\)](Replication.Modify.md#Replication.Modify.API)  This action cannot be used to create a replication group with cluster mode enabled\. | 

## ElastiCache for Redis Version 3\.2\.10 \(Enhanced\)<a name="SelectEngine.RedisVersions.3-2-10"></a>

Amazon ElastiCache for Redis introduces the next major version of the Redis engine supported by Amazon ElastiCache\. ElastiCache for Redis 3\.2\.10 introduces online cluster resizing to add or remove shards from the cluster while it continues to serve incoming I/O requests\. ElastiCache for Redis 3\.2\.10 users have all the functionality of earlier Redis versions except the ability to encrypt their data which is currently only available in version 3\.2\.6\. 


**Comparing ElastiCache for Redis versions 3\.2\.6 and 3\.2\.10**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/SelectEngine.RedisVersions.html)

For more information, see:

+ [Online Resharding and Shard Rebalancing for ElastiCache for Redis—Redis \(cluster mode enabled\)](scaling-redis-cluster-mode-enabled.md#redis-cluster-resharding-online)

+ [Best Practices: Online Resharding](best-practices-online-resharding.md)

## ElastiCache for Redis Version 3\.2\.6 \(Enhanced\)<a name="SelectEngine.RedisVersions.3-2-6"></a>

Amazon ElastiCache for Redis introduces the next major version of the Redis engine supported by Amazon ElastiCache\. ElastiCache for Redis 3\.2\.6 users have all the functionality of earlier Redis versions plus the option to encrypt their data\. For more information, see:

+ [Amazon ElastiCache for Redis In\-Transit Encryption](in-transit-encryption.md)

+ [Amazon ElastiCache for Redis At\-Rest Encryption](at-rest-encryption.md)

+ [HIPAA Compliance for Amazon ElastiCache for Redis](elasticache-compliance-hipaa.md)

## ElastiCache for Redis Version 3\.2\.4 \(Enhanced\)<a name="SelectEngine.RedisVersions.3-2-4"></a>

Amazon ElastiCache for Redis version 3\.2\.4 introduces the next major version of the Redis engine supported by Amazon ElastiCache\. ElastiCache for Redis 3\.2\.4 users have all the functionality of earlier Redis versions available to them plus the option to run in *cluster mode* or *non\-cluster mode*\. The following table summarizes \.


**Comparing Redis 3\.2\.4 Non\-Cluster Mode and Cluster Mode**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/SelectEngine.RedisVersions.html)

**Notes:**

+ **Partitioning** – the ability to split your data across 2 to 15 node groups \(shards\) with replication support for each node group\.

+ **Geospatial indexing** – Redis 3\.2\.4 introduces support for geospatial indexing via six GEO commands\. For more information, see the Redis GEO\* command documentation [Redis Commands: GEO](http://redis.io/commands#geo) on the Redis Commands page \(filtered for GEO\)\.

For information about additional Redis 3 features, see [Redis 3\.2 release notes](https://raw.githubusercontent.com/antirez/redis/3.2/00-RELEASENOTES) and [Redis 3\.0 release notes](https://raw.githubusercontent.com/antirez/redis/3.0/00-RELEASENOTES)\.

Currently ElastiCache managed Redis \(cluster mode enabled\) does not support the following Redis 3\.2 features:

+ Replica migration

+ Cluster rebalancing

+ Lua debugger

ElastiCache disables the following Redis 3\.2 management commands:

+ `cluster meet`

+ `cluster replicate`

+ `cluster flushslots`

+ `cluster addslots`

+ `cluster delslots`

+ `cluster setslot`

+ `cluster saveconfig`

+ `cluster forget`

+ `cluster failover`

+ `cluster bumpepoch`

+ `cluster set-config-epoch`

+ `cluster reset`

For information about Redis 3\.2\.4 parameters, see [Redis 3\.2\.4 Parameter Changes](ParameterGroups.Redis.md#ParameterGroups.Redis.3-2-4)\.

## ElastiCache for Redis Version 2\.8\.24 \(Enhanced\)<a name="SelectEngine.RedisVersions.2-8-24"></a>

Redis improvements added since version 2\.8\.23 include bug fixes and logging of bad memory access addresses\. For more information, see [Redis 2\.8 release notes](https://raw.githubusercontent.com/antirez/redis/2.8/00-RELEASENOTES)\. 

## ElastiCache for Redis Version 2\.8\.23 \(Enhanced\)<a name="SelectEngine.RedisVersions.2-8-23"></a>

Redis improvements added since version 2\.8\.22 include bug fixes\. For more information, see [Redis 2\.8 release notes](https://raw.githubusercontent.com/antirez/redis/2.8/00-RELEASENOTES)\. This release also includes support for the new parameter `close-on-slave-write` which, if enabled, disconnects clients who attempt to write to a read\-only replica\.

For more information on Redis 2\.8\.23 parameters, see [Redis 2\.8\.23 \(Enhanced\) Added Parameters](ParameterGroups.Redis.md#ParameterGroups.Redis.2-8-23) in the ElastiCache User Guide\.

## ElastiCache for Redis Version 2\.8\.22 \(Enhanced\)<a name="SelectEngine.RedisVersions.2-8-22"></a>

Redis improvements added since version 2\.8\.21 include the following:

+ Support for forkless backups and synchronizations, which allows you to allocate less memory for backup overhead and more for your application\. For more information, see [How Synchronization and Backup are Implemented](Replication.Redis.Versions.md)\. The forkless process can impact both latency and throughput\. In the case of high write throughput, when a replica re\-syncs, it may be unreachable for the entire time it is syncing\.

+ In the event of a failover, replication groups now recover faster because replicas perform partial syncs with the primary rather than full syncs whenever possible\. Additionally, both the primary and replicas no longer use the disk during syncs, providing further speed gains\.

+ Support for two new CloudWatch metrics\. 

  + `ReplicationBytes` – The number of bytes a replication group's primary cluster is sending to the read replicas\.

  + `SaveInProgress` – A binary value that indicates whether or not there is a background save process running\.

   For more information, see [Metrics for Redis](CacheMetrics.Redis.md)\.

+ A number of critical bug fixes in replication PSYNC behavior\. For more information, see [Redis 2\.8 release notes](https://raw.githubusercontent.com/antirez/redis/2.8/00-RELEASENOTES)\.

+ To maintain enhanced replication performance in Multi\-AZ replication groups and for increased cluster stability, non\-ElastiCache replicas are no longer supported\.

+ To improve data consistency between the primary cluster and replicas in a replication group, the replicas no longer evict keys independent of the primary cluster\.

+ Redis configuration variables `appendonly` and `appendfsync` are not supported on Redis version 2\.8\.22 and later\.

+ In low\-memory situations, clients with a large output buffer may be disconnected from a replica cluster\. If disconnected, the client needs to reconnect\. Such situations are most likely to occur for PUBSUB clients\.

## ElastiCache for Redis Version 2\.8\.21<a name="SelectEngine.RedisVersions.2-8-21"></a>

Redis improvements added since version 2\.8\.19 include a number of bug fixes\. For more information, see [Redis 2\.8 release notes](https://raw.githubusercontent.com/antirez/redis/2.8/00-RELEASENOTES)\.

## ElastiCache for Redis Version 2\.8\.19<a name="SelectEngine.RedisVersions.2-8-19"></a>

Redis improvements added since version 2\.8\.6 include the following:

+ Support for HyperLogLog\. For more information, go to [Redis new data structure: HyperLogLog](http://antirez.com/news/75)\.

+ The sorted set data type now has support for lexicographic range queries with the new commands `ZRANGEBYLEX`, `ZLEXCOUNT`, and `ZREMRANGEBYLEX`\.

+ To prevent a primary node from sending stale data to replica nodes, the master SYNC fails if a background save \(`bgsave`\) child process is aborted\.

+ Support for the *HyperLogLogBasedCommands* CloudWatch metric\. For more information, see [Metrics for Redis](CacheMetrics.Redis.md)\.

## ElastiCache for Redis Version 2\.8\.6<a name="SelectEngine.RedisVersions.2-8-6"></a>

Redis improvements added since version 2\.6\.13 include the following:

+ Improved resiliency and fault tolerance for read replicas\.

+ Support for partial resynchronization\.

+ Support for user\-defined minimum number of read replicas that must be available at all times\.

+ Full support for pub/sub—notifying clients of events on the server\.

+ Automatic detection of a primary node failure and failover of your primary node to a secondary node\.

## ElastiCache for Redis Version 2\.6\.13<a name="SelectEngine.RedisVersions.2-6-13"></a>

Redis version 2\.6\.13 was the initial version of Redis supported by Amazon ElastiCache\. Multi\-AZ with automatic failover is not supported on Redis 2\.6\.13\.