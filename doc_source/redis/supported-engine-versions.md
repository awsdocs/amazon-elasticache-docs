# Supported ElastiCache for Redis versions<a name="supported-engine-versions"></a>

**Topics**
+ [Redis 7\.0 \(enhanced\)](#redis-version-7.0)
+ [Redis 6\.2 \(enhanced\)](#redis-version-6.2)
+ [Redis 6\.0 \(enhanced\)](#redis-version-6.0)
+ [Redis 5\.0\.6 \(enhanced\)](#redis-version-5-0.6)
+ [Redis 5\.0\.5 \(enhanced\)](#redis-version-5-0.5)
+ [Redis 5\.0\.4 \(enhanced\)](#redis-version-5-0.4)
+ [Redis 5\.0\.3 \(enhanced\)](#redis-version-5-0.3)
+ [Redis 5\.0\.0 \(enhanced\)](#redis-version-5-0)
+ [Redis 4\.0\.10 \(enhanced\)](#redis-version-4-0-10)
+ [Redis 3\.2\.10 \(enhanced\)](#redis-version-3-2-10)
+ [Redis 3\.2\.6 \(enhanced\)](#redis-version-3-2-6)
+ [Redis 3\.2\.4 \(enhanced\)](#redis-version-3-2-4)
+ [Redis 2\.8\.24 \(enhanced\)](#redis-version-2-8-24)
+ [Redis 2\.8\.23 \(enhanced\)](#redis-version-2-8-23)
+ [Redis 2\.8\.22 \(enhanced\)](#redis-version-2-8-22)
+ [Redis 2\.8\.21](#redis-version-2-8-21)
+ [Redis 2\.8\.19](#redis-version-2-8-19)
+ [Redis 2\.8\.6](#redis-version-2-8-6)
+ [Redis 2\.6\.13](#redis-version-2-6-13)

**Note**  
Because the newer Redis versions provide a better and more stable user experience, Redis versions 2\.6\.13, 2\.8\.6, and 2\.8\.19 are deprecated when using the ElastiCache console\. We recommend against using these Redis versions\. If you need to use one of them, work with the AWS CLI or ElastiCache API\.  
For more information, see the following topics:  


|  | AWS CLI | ElastiCache API | 
| --- | --- | --- | 
| **Create Cluster** | [Creating a cluster \(AWS CLI\)](Clusters.Create.md#Clusters.Create.CLI) You can't use this action to create a replication group with cluster mode enabled\. | [Creating a cluster \(ElastiCache API\)](Clusters.Create.md#Clusters.Create.API)  You can't use this action to create a replication group with cluster mode enabled\.  | 
| **Modify Cluster** | [Using the AWS CLI](Clusters.Modify.md#Clusters.Modify.CLI)  You can't use this action to create a replication group with cluster mode enabled\.  | [Using the ElastiCache API](Clusters.Modify.md#Clusters.Modify.API)  You can't use this action to create a replication group with cluster mode enabled\. | 
| **Create Replication Group** | [Creating a Redis \(Cluster Mode Disabled\) replication group from scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.CLI) [Creating a Redis \(Cluster Mode Enabled\) replication group from scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.CLI)  | [Creating a Redis \(cluster mode disabled\) replication group from scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.API) [Creating a replication group in Redis \(Cluster Mode Enabled\) from scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.API) | 
| **Modify Replication Group** | [Using the AWS CLI](Replication.Modify.md#Replication.Modify.CLI)  | [Using the ElastiCache API](Replication.Modify.md#Replication.Modify.API)  | 

## ElastiCache for Redis version 7\.0 \(enhanced\)<a name="redis-version-7.0"></a>

ElastiCache for Redis 7\.0 adds a number of improvements and support for new functionality:
+ [Redis Functions](https://redis.io/docs/manual/programmability/functions-intro/): ElastiCache for Redis 7 adds support for Redis Functions, and provides a managed experience enabling developers to execute [LUA scripts](https://redis.io/docs/manual/programmability/eval-intro/) with application logic stored on the ElastiCache cluster, without requiring clients to re\-send the scripts to the server with every connection\. 
+ [ACL improvements](https://redis.io/docs/management/security/acl/): ElastiCache for Redis 7 adds support for the next version of Redis Access Control Lists \(ACLs\)\. With ElastiCache for Redis 7, clients can now specify multiple sets of permissions on specific keys or keyspaces in Redis\. 
+ [Sharded Pub/Sub](https://redis.io/docs/manual/pubsub/#sharded-pubsub): ElastiCache for Redis 7 adds support to run Redis Pub/Sub functionality in a sharded way when running ElastiCache in Cluster Mode Enabled \(CME\)\. Redis Pub/Sub capabilities enable publishers to issue messages to any number of subscribers on a channel\. With Amazon ElastiCache for Redis 7, channels are bound to a shard in the ElastiCache cluster, eliminating the need to propagate channel information across shards resulting in improved scalability\. 
+ Enhanced I/O multiplexing: ElastiCache for Redis version 7 introduces enhanced I/O multiplexing, which delivers increased throughput and reduced latency for high\-throughput workloads that have many concurrent client connections to an ElastiCache cluster\. For example, when using a cluster of r6g\.xlarge nodes and running 5200 concurrent clients, you can achieve up to 72% increased throughput \(read and write operations per second\) and up to 71% decreaseed P99 latency, compared with ElastiCache for Redis version 6\. 

For more information on the Redis 7\.0 release, see [Redis 7\.0 Release Notes](https://raw.githubusercontent.com/antirez/redis/7.0/00-RELEASENOTES) at Redis on GitHub\.

## ElastiCache for Redis version 6\.2 \(enhanced\)<a name="redis-version-6.2"></a>

ElastiCache for Redis 6\.2 includes performance improvements for TLS\-enabled clusters using x86 node types with 8 vCPUs or more or Graviton2 node types with 4 vCPUs or more\. These enhancements improve throughput and reduce client connection establishment time by offloading encryption to other vCPUs\. With Redis 6\.2, you can also manage access to Pub/Sub channels with Access Control List \(ACL\) rules\.

 With this version, we also introduce support for data tiering on cluster nodes containing locally attached NVMe SSD\. For more information, see [Data tiering](data-tiering.md)\.

Redis engine version 6\.2\.6 also introduces support for native JavaScript Object Notation \(JSON\) format, a simple, schemaless way to encode complex datasets inside Redis clusters\. With JSON support, you can leverage the performance and Redis APIs for applications that operate over JSON\. For more information, see [Getting started with JSON](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/json-gs.html)\. Also included are JSON\-related metrics, `JsonBasedCmds` and `JsonBasedCmdsLatency`, that are incorporated into CloudWatch to monitor the usage of this datatype\. For more information, see [Metrics for Redis](CacheMetrics.Redis.md)\.

You specify the engine version by using 6\.2\. ElastiCache for Redis will automatically invoke the preferred patch version of Redis 6\.2 that is available\. For example, when you create/modify a cache cluster, you set the `--engine-version` parameter to 6\.2\. The cluster will be launched with the current available preferred patch version of Redis 6\.2 at the creation/modification time\. Specifying engine version 6\.x in the API will result in the latest minor version of Redis 6\.

For existing 6\.0 clusters, you can opt\-in to the next auto minor version upgrade by setting the `AutoMinorVersionUpgrade` parameter to `yes` in the `CreateCacheCluster`, `ModifyCacheCluster`, `CreateReplicationGroup` or `ModifyReplicationGroup` APIs\. ElastiCache for Redis will upgrade the minor version of your existing 6\.0 clusters to 6\.2 using self\-service updates\. For more information, see [Self\-service updates in Amazon ElastiCache](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Self-Service-Updates.html)\.

When calling the DescribeCacheEngineVersions API, the `EngineVersion` parameter value will be set to 6\.2 and the actual engine version with the patch version will be returned in the `CacheEngineVersionDescription` field\. 

For more information on the Redis 6\.2 release, see [Redis 6\.2 Release Notes](https://raw.githubusercontent.com/antirez/redis/6.2/00-RELEASENOTES) at Redis on GitHub\.

## ElastiCache for Redis version 6\.0 \(enhanced\)<a name="redis-version-6.0"></a>

Amazon ElastiCache for Redis introduces the next version of the Redis engine, which includes [Authenticating Users with Role Based Access Control](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Clusters.RBAC.html), client\-side caching and significant operational improvements\. 

 Beginning with Redis 6\.0, ElastiCache for Redis will offer a single version for each Redis OSS minor release, rather than offering multiple patch versions\. ElastiCache for Redis will automatically manage the patch version of your running cache clusters, ensuring improved performance and enhanced security\. 

You can also opt\-in to the next auto minor version upgrade by setting the `AutoMinorVersionUpgrade` parameter to `yes` and ElastiCache for Redis will manage the minor version upgrade, through self\-service updates\. For more information, see [Service updates in ElastiCache for Redis](Self-Service-Updates.md)\. 

You specify the engine version by using `6.0`\. ElastiCache for Redis will automatically invoke the preferred patch version of Redis 6\.0 that is available\. For example, when you create/modify a cache cluster, you set the `--engine-version` parameter to 6\.0\. The cluster will be launched with the current available preferred patch version of Redis 6\.0 at the creation/modification time\. Any request with a specific patch version value will be rejected, an exception will be thrown and the process will fail\.

When calling the DescribeCacheEngineVersions API, the `EngineVersion` parameter value will be set to 6\.0 and the actual engine version with the patch version will be returned in the `CacheEngineVersionDescription` field\. 

For more information on the Redis 6\.0 release, see [Redis 6\.0 Release Notes](https://raw.githubusercontent.com/antirez/redis/6.0/00-RELEASENOTES) at Redis on GitHub\.

## ElastiCache for Redis version 5\.0\.6 \(enhanced\)<a name="redis-version-5-0.6"></a>

Amazon ElastiCache for Redis introduces the next version of the Redis engine, which includes bug fixes\. 

For more information, see [Redis 5\.0\.6 Release Notes](https://raw.githubusercontent.com/antirez/redis/5.0/00-RELEASENOTES) at Redis on GitHub\.

## ElastiCache for Redis version 5\.0\.5 \(enhanced\)<a name="redis-version-5-0.5"></a>

Amazon ElastiCache for Redis introduces the next version of the Redis engine;\. It includes online configuration changes for ElastiCache for Redis of auto\-failover clusters during all planned operations\. You can now scale your cluster, upgrade the Redis engine version and apply patches and maintenance updates while the cluster stays online and continues serving incoming requests\. It also includes bug fixes\.

For more information, see [Redis 5\.0\.5 Release Notes](https://raw.githubusercontent.com/antirez/redis/5.0/00-RELEASENOTES) at Redis on GitHub\.

## ElastiCache for Redis version 5\.0\.4 \(enhanced\)<a name="redis-version-5-0.4"></a>

Amazon ElastiCache for Redis introduces the next version of the Redis engine supported by Amazon ElastiCache\. It includes the following enhancements:
+ Engine stability guarantee in special conditions\.
+ Improved Hyperloglog error handling\.
+ Enhanced handshake commands for reliable replication\.
+ Consistent message delivery tracking via `XCLAIM` command\.
+ Improved `LFU `field management in objects\.
+ Enhanced transaction management when using `ZPOP`\. 

For more information, see [Redis 5\.0\.4 Release Notes](https://raw.githubusercontent.com/antirez/redis/5.0/00-RELEASENOTES) at Redis on GitHub\.

## ElastiCache for Redis version 5\.0\.3 \(enhanced\)<a name="redis-version-5-0.3"></a>

Amazon ElastiCache for Redis introduces the next version of the Redis engine supported by Amazon ElastiCache\. It includes the following enhancements:
+ Bug fixes to improve sorted set edge cases, accurate memory usage and more\. For more information, see [Redis 5\.0\.3 release notes](https://raw.githubusercontent.com/antirez/redis/5.0/00-RELEASENOTES)\.
+ Ability to rename commands: ElastiCache for Redis 5\.0\.3 includes a new parameter called `rename-commands` that allows you to rename potentially dangerous or expensive Redis commands that might cause accidental data loss, such as `FLUSHALL` or `FLUSHDB`\. This is similar to the rename\-command configuration in open source Redis\. However, ElastiCache has improved the experience by providing a fully managed workflow\. The command name changes are applied immediately, and automatically propagated across all nodes in the cluster that contain the command list\. There is no intervention required on your part, such as rebooting nodes\. 

  The following examples demonstrate how to modify existing parameter groups\. They include the `rename-commands` parameter, which is a space\-separated list of commands you want to rename:

  ```
  aws elasticache modify-cache-parameter-group --cache-parameter-group-name custom_param_group
  --parameter-name-values "ParameterName=rename-commands,  ParameterValue='flushall restrictedflushall'" --region region
  ```

  In this example, the *rename\-commands* parameter is used to rename the `flushall` command to `restrictedflushall`\.

  To rename multiple commands, use the following:

  ```
  aws elasticache modify-cache-parameter-group --cache-parameter-group-name custom_param_group
  --parameter-name-values "ParameterName=rename-commands,  ParameterValue='flushall restrictedflushall flushdb restrictedflushdb''" --region region
  ```

  To revert any change, re\-run the command and exclude any renamed values from the `ParameterValue` list that you want to retain, as shown following:

  ```
  aws elasticache modify-cache-parameter-group --cache-parameter-group-name custom_param_group
  --parameter-name-values "ParameterName=rename-commands,  ParameterValue='flushall restrictedflushall'" --region region
  ```

  In this case, the `flushall` command is renamed to `restrictedflushall` and any other renamed commands revert to their original command names\.
**Note**  
When renaming commands, you are restricted to the following limitations:  
All renamed commands should be alphanumeric\.
The maximum length of new command names is 20 alphanumeric characters\.
When renaming commands, ensure that you update the parameter group associated with your cluster\.
To prevent a command's use entirely, use the keyword `blocked`, as shown following:  

    ```
    aws elasticache modify-cache-parameter-group --cache-parameter-group-name custom_param_group
    --parameter-name-values "ParameterName=rename-commands,  ParameterValue='flushall blocked'" --region region
    ```

  For more information on the parameter changes and a list of what commands are eligible for renaming, see [Redis 5\.0\.3 parameter changes](ParameterGroups.Redis.md#ParameterGroups.Redis.5-0-3)\.

## ElastiCache for Redis version 5\.0\.0 \(enhanced\)<a name="redis-version-5-0"></a>

Amazon ElastiCache for Redis introduces the next major version of the Redis engine supported by Amazon ElastiCache\. ElastiCache for Redis 5\.0\.0 brings support for the following improvements:
+ Redis Streams: This models a log data structure that allows producers to append new items in real time\. It also allows consumers to consume messages either in a blocking or nonblocking fashion\. Streams also allow consumer groups, which represent a group of clients to cooperatively consume different portions of the same stream of messages, similar to [Apache Kafka](https://kafka.apache.org/documentation/)\. For more information, see [Introduction to Redis Streams](https://redis.io/topics/streams-intro)\.
+ Support for a family of stream commands, such as `XADD`, `XRANGE` and `XREAD`\. For more information, see [Redis Streams Commands](https://redis.io/commands#stream)\.
+ A number of new and renamed parameters\. For more information, see [Redis 5\.0\.0 parameter changes](ParameterGroups.Redis.md#ParameterGroups.Redis.5.0)\.
+ A new Redis metric, `StreamBasedCmds`\.
+ Slightly faster snapshot time for Redis nodes\.

**Important**  
Amazon ElastiCache for Redis has back\-ported two critical bug fixes from [Redis open source version 5\.0\.1](https://raw.githubusercontent.com/antirez/redis/5.0/00-RELEASENOTES)\. They are listed following:  
RESTORE mismatch reply when certain keys have already expired\.
The `XCLAIM` command can potentially return a wrong entry or desynchronize the protocol\.
Both of these bug fixes are included in ElastiCache for Redis support for Redis engine version 5\.0\.0 and are consumed in future version updates\.

## ElastiCache for Redis version 4\.0\.10 \(enhanced\)<a name="redis-version-4-0-10"></a>

Amazon ElastiCache for Redis introduces the next major version of the Redis engine supported by Amazon ElastiCache\. ElastiCache for Redis 4\.0\.10 brings support the following improvements:
+ Both online cluster resizing and encryption in a single ElastiCache for Redis version\. For more information, see the following:
  + [Scaling clusters in Redis \(Cluster Mode Enabled\)](scaling-redis-cluster-mode-enabled.md)
  + [Online resharding and shard rebalancing for Redis \(cluster mode enabled\)](redis-cluster-resharding-online.md)
  + [Data security in Amazon ElastiCache](encryption.md)
+ A number of new parameters\. For more information, see [Redis 4\.0\.10 parameter changes](ParameterGroups.Redis.md#ParameterGroups.Redis.4-0-10)\.
+ Support for family of memory commands, such as `MEMORY`\. For more information, see [Redis Commands](https://redis.io/commands#) \(search on MEMO\)\.
+ Support for memory defragmentation while online thus allowing more efficient memory utilization and more memory available for your data\.
+ Support for asynchronous flushes and deletes\. ElastiCache for Redis supports commands like `UNLINK`, `FLUSHDB` and `FLUSHALL` to run in a different thread from the main thread\. Doing this helps improve performance and response times for your applications by freeing memory asynchronously\.
+ A new Redis metric, `ActiveDefragHits`\. For more information, see [Metrics for Redis](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CacheMetrics.Redis.html)\.

Redis \(cluster mode disabled\) users running Redis version 3\.2\.10 can use the console to upgrade their clusters via online upgrade\.


**Comparing ElastiCache for Redis cluster resizing and encryption support**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/supported-engine-versions.html)

## ElastiCache for Redis version 3\.2\.10 \(enhanced\)<a name="redis-version-3-2-10"></a>

Amazon ElastiCache for Redis introduces the next major version of the Redis engine supported by Amazon ElastiCache\. ElastiCache for Redis 3\.2\.10 introduces online cluster resizing to add or remove shards from the cluster while it continues to serve incoming I/O requests\. ElastiCache for Redis 3\.2\.10 users have all the functionality of earlier Redis versions except the ability to encrypt their data\. This ability is currently available only in version 3\.2\.6\. 


**Comparing ElastiCache for Redis versions 3\.2\.6 and 3\.2\.10**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/supported-engine-versions.html)

For more information, see the following:
+ [Online resharding and shard rebalancing for Redis \(cluster mode enabled\)](redis-cluster-resharding-online.md)
+ [Best practices: Online cluster resizing](best-practices-online-resharding.md)

## ElastiCache for Redis version 3\.2\.6 \(enhanced\)<a name="redis-version-3-2-6"></a>

Amazon ElastiCache for Redis introduces the next major version of the Redis engine supported by Amazon ElastiCache\. ElastiCache for Redis 3\.2\.6 users have all the functionality of earlier Redis versions plus the option to encrypt their data\. For more information, see the following:
+ [ElastiCache in\-transit encryption \(TLS\)](in-transit-encryption.md)
+ [At\-Rest Encryption in ElastiCache for Redis](at-rest-encryption.md)
+ [HIPAA eligibility](elasticache-compliance.md#elasticache-compliance-hipaa)

## ElastiCache for Redis version 3\.2\.4 \(enhanced\)<a name="redis-version-3-2-4"></a>

Amazon ElastiCache for Redis version 3\.2\.4 introduces the next major version of the Redis engine supported by Amazon ElastiCache\. ElastiCache for Redis 3\.2\.4 users have all the functionality of earlier Redis versions available to them plus the option to run in *cluster mode* or *non\-cluster mode*\. The following table summarizes \.


**Comparing Redis 3\.2\.4 non\-cluster mode and cluster mode**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/supported-engine-versions.html)

**Notes:**
+ **Partitioning** – the ability to split your data across 2 to 500 node groups \(shards\) with replication support for each node group\.
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

For information about Redis 3\.2\.4 parameters, see [Redis 3\.2\.4 parameter changes](ParameterGroups.Redis.md#ParameterGroups.Redis.3-2-4)\.

## ElastiCache for Redis version 2\.8\.24 \(enhanced\)<a name="redis-version-2-8-24"></a>

Redis improvements added since version 2\.8\.23 include bug fixes and logging of bad memory access addresses\. For more information, see [Redis 2\.8 release notes](https://raw.githubusercontent.com/antirez/redis/2.8/00-RELEASENOTES)\. 

## ElastiCache for Redis version 2\.8\.23 \(enhanced\)<a name="redis-version-2-8-23"></a>

Redis improvements added since version 2\.8\.22 include bug fixes\. For more information, see [Redis 2\.8 release notes](https://raw.githubusercontent.com/antirez/redis/2.8/00-RELEASENOTES)\. This release also includes support for the new parameter `close-on-slave-write` which, if enabled, disconnects clients who attempt to write to a read\-only replica\.

For more information on Redis 2\.8\.23 parameters, see [Redis 2\.8\.23 \(enhanced\) added parameters](ParameterGroups.Redis.md#ParameterGroups.Redis.2-8-23) in the ElastiCache User Guide\.

## ElastiCache for Redis version 2\.8\.22 \(enhanced\)<a name="redis-version-2-8-22"></a>

Redis improvements added since version 2\.8\.21 include the following:
+ Support for forkless backups and synchronizations, which allows you to allocate less memory for backup overhead and more for your application\. For more information, see [How synchronization and backup are implemented](Replication.Redis.Versions.md)\. The forkless process can impact both latency and throughput\. When there is high write throughput, when a replica re\-syncs, it can be unreachable for the entire time it is syncing\.
+ If there is a failover, replication groups now recover faster because replicas perform partial syncs with the primary rather than full syncs whenever possible\. Additionally, both the primary and replicas no longer use the disk during syncs, providing further speed gains\.
+ Support for two new CloudWatch metrics\. 
  + `ReplicationBytes` – The number of bytes a replication group's primary cluster is sending to the read replicas\.
  + `SaveInProgress` – A binary value that indicates whether or not there is a background save process running\.

   For more information, see [Monitoring use with CloudWatch Metrics](CacheMetrics.md)\.
+ A number of critical bug fixes in replication PSYNC behavior\. For more information, see [Redis 2\.8 release notes](https://raw.githubusercontent.com/antirez/redis/2.8/00-RELEASENOTES)\.
+ To maintain enhanced replication performance in Multi\-AZ replication groups and for increased cluster stability, non\-ElastiCache replicas are no longer supported\.
+ To improve data consistency between the primary cluster and replicas in a replication group, the replicas no longer evict keys independent of the primary cluster\.
+ Redis configuration variables `appendonly` and `appendfsync` are not supported on Redis version 2\.8\.22 and later\.
+ In low\-memory situations, clients with a large output buffer might be disconnected from a replica cluster\. If disconnected, the client needs to reconnect\. Such situations are most likely to occur for PUBSUB clients\.

## ElastiCache for Redis version 2\.8\.21<a name="redis-version-2-8-21"></a>

Redis improvements added since version 2\.8\.19 include a number of bug fixes\. For more information, see [Redis 2\.8 release notes](https://raw.githubusercontent.com/antirez/redis/2.8/00-RELEASENOTES)\.

## ElastiCache for Redis version 2\.8\.19<a name="redis-version-2-8-19"></a>

Redis improvements added since version 2\.8\.6 include the following:
+ Support for HyperLogLog\. For more information, see [Redis new data structure: HyperLogLog](http://antirez.com/news/75)\.
+ The sorted set data type now has support for lexicographic range queries with the new commands `ZRANGEBYLEX`, `ZLEXCOUNT`, and `ZREMRANGEBYLEX`\.
+ To prevent a primary node from sending stale data to replica nodes, the master SYNC fails if a background save \(`bgsave`\) child process is aborted\.
+ Support for the *HyperLogLogBasedCommands* CloudWatch metric\. For more information, see [Metrics for Redis](CacheMetrics.Redis.md)\.

## ElastiCache for Redis version 2\.8\.6<a name="redis-version-2-8-6"></a>

Redis improvements added since version 2\.6\.13 include the following:
+ Improved resiliency and fault tolerance for read replicas\.
+ Support for partial resynchronization\.
+ Support for user\-defined minimum number of read replicas that must be available at all times\.
+ Full support for pub/sub—notifying clients of events on the server\.
+ Automatic detection of a primary node failure and failover of your primary node to a secondary node\.

## ElastiCache for Redis version 2\.6\.13<a name="redis-version-2-6-13"></a>

Redis version 2\.6\.13 was the initial version of Redis supported by Amazon ElastiCache for Redis\. Multi\-AZ is not supported on Redis 2\.6\.13\.