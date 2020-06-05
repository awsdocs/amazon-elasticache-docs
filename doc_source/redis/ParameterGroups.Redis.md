# Redis Specific Parameters<a name="ParameterGroups.Redis"></a>

If you do not specify a parameter group for your Redis cluster, then a default parameter group appropriate to your engine version will be used\. You can't change the values of any parameters in the default parameter group\. However, you can create a custom parameter group and assign it to your cluster at any time as long as the values of conditionally modifiable parameters are the same in both parameter groups\. For more information, see [Creating a Parameter Group](ParameterGroups.Creating.md)\.

**Topics**
+ [Redis 5\.0\.3 Parameter Changes](#ParameterGroups.Redis.5-0-3)
+ [Redis 5\.0\.0 Parameter Changes](#ParameterGroups.Redis.5.0)
+ [Redis 4\.0\.10 Parameter Changes](#ParameterGroups.Redis.4-0-10)
+ [Redis 3\.2\.10 Parameter Changes](#ParameterGroups.Redis.3-2-10)
+ [Redis 3\.2\.6 Parameter Changes](#ParameterGroups.Redis.3-2-6)
+ [Redis 3\.2\.4 Parameter Changes](#ParameterGroups.Redis.3-2-4)
+ [Redis 2\.8\.24 \(Enhanced\) Added Parameters](#ParameterGroups.Redis.2-8-24)
+ [Redis 2\.8\.23 \(Enhanced\) Added Parameters](#ParameterGroups.Redis.2-8-23)
+ [Redis 2\.8\.22 \(Enhanced\) Added Parameters](#ParameterGroups.Redis.2-8-22)
+ [Redis 2\.8\.21 Added Parameters](#ParameterGroups.Redis.2-8-21)
+ [Redis 2\.8\.19 Added Parameters](#ParameterGroups.Redis.2-8-19)
+ [Redis 2\.8\.6 Added Parameters](#ParameterGroups.Redis.2-8-6)
+ [Redis 2\.6\.13 Parameters](#ParameterGroups.Redis.2-6-13)
+ [Redis Node\-Type Specific Parameters](#ParameterGroups.Redis.NodeSpecific)

**Note**  
Because the newer Redis versions provide a better and more stable user experience, Redis versions 2\.6\.13, 2\.8\.6, and 2\.8\.19 are deprecated when using the ElastiCache console\. We recommend against using these Redis versions\. If you need to use one of them, work with the AWS CLI or ElastiCache API\.  
For more information, see the following topics:  


|  | AWS CLI | ElastiCache API | 
| --- | --- | --- | 
| **Create Cluster** | [Creating a Cluster \(AWS CLI\)](Clusters.Create.CLI.md) You can't use this action to create a replication group with cluster mode enabled\. | [Creating a Cluster \(ElastiCache API\)](Clusters.Create.API.md)  You can't use this action to create a replication group with cluster mode enabled\.  | 
| **Modify Cluster** | [Using the AWS CLI](Clusters.Modify.md#Clusters.Modify.CLI)  You can't use this action to create a replication group with cluster mode enabled\.  | [Using the ElastiCache API](Clusters.Modify.md#Clusters.Modify.API)  You can't use this action to create a replication group with cluster mode enabled\. | 
| **Create Replication Group** | [Creating a Redis \(Cluster Mode Disabled\) Replication Group from Scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.CLI) [Creating a Redis \(Cluster Mode Enabled\) Replication Group from Scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.CLI)  | [Creating a Redis \(cluster mode disabled\) Replication Group from Scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.API) [Creating a Replication Group in Redis \(Cluster Mode Enabled\) from Scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.API) | 
| **Modify Replication Group** | [Using the AWS CLI](Replication.Modify.md#Replication.Modify.CLI)  | [Using the ElastiCache API](Replication.Modify.md#Replication.Modify.API)  | 

## Redis 5\.0\.3 Parameter Changes<a name="ParameterGroups.Redis.5-0-3"></a>

**Parameter group family:** redis5\.0

Redis 5\.0 default parameter groups
+ `default.redis5.0` – Use this parameter group, or one derived from it, for Redis \(cluster mode disabled\) clusters and replication groups\.
+ `default.redis5.0.cluster.on` – Use this parameter group, or one derived from it, for Redis \(cluster mode enabled\) clusters and replication groups\.


**Parameters added in Redis 5\.0\.3**  

|  Name  |  Details |  Description  | 
| --- | --- | --- | 
| rename\-commands |  Default: none Type: string Modifiable: Yes Changes take effect: Immediately across all nodes in the cluster | A space\-separated list of renamed Redis commands\. The following is a restricted list of commands available for renaming:  `APPEND AUTH BITCOUNT BITFIELD BITOP BITPOS BLPOP BRPOP BRPOPLPUSH BZPOPMIN BZPOPMAX CLIENT CLUSTER COMMAND DBSIZE DECR DECRBY DEL DISCARD DUMP ECHO EVAL EVALSHA EXEC EXISTS EXPIRE EXPIREAT FLUSHALL FLUSHDB GEOADD GEOHASH GEOPOS GEODIST GEORADIUS GEORADIUSBYMEMBER GET GETBIT GETRANGE GETSET HDEL HEXISTS HGET HGETALL HINCRBY HINCRBYFLOAT HKEYS HLEN HMGET HMSET HSET HSETNX HSTRLEN HVALS INCR INCRBY INCRBYFLOAT INFO KEYS LASTSAVE LINDEX LINSERT LLEN LPOP LPUSH LPUSHX LRANGE LREM LSET LTRIM MEMORY MGET MONITOR MOVE MSET MSETNX MULTI OBJECT PERSIST PEXPIRE PEXPIREAT PFADD PFCOUNT PFMERGE PING PSETEX PSUBSCRIBE PUBSUB PTTL PUBLISH PUNSUBSCRIBE RANDOMKEY READONLY READWRITE RENAME RENAMENX RESTORE ROLE RPOP RPOPLPUSH RPUSH RPUSHX SADD SCARD SCRIPT SDIFF SDIFFSTORE SELECT SET SETBIT SETEX SETNX SETRANGE SINTER SINTERSTORE SISMEMBER SLOWLOG SMEMBERS SMOVE SORT SPOP SRANDMEMBER SREM STRLEN SUBSCRIBE SUNION SUNIONSTORE SWAPDB TIME TOUCH TTL TYPE UNSUBSCRIBE UNLINK UNWATCH WAIT WATCH ZADD ZCARD ZCOUNT ZINCRBY ZINTERSTORE ZLEXCOUNT ZPOPMAX ZPOPMIN ZRANGE ZRANGEBYLEX ZREVRANGEBYLEX ZRANGEBYSCORE ZRANK ZREM ZREMRANGEBYLEX ZREMRANGEBYRANK ZREMRANGEBYSCORE ZREVRANGE ZREVRANGEBYSCORE ZREVRANK ZSCORE ZUNIONSTORE SCAN SSCAN HSCAN ZSCAN XINFO XADD XTRIM XDEL XRANGE XREVRANGE XLEN XREAD XGROUP XREADGROUP XACK XCLAIM XPENDING GEORADIUS_RO GEORADIUSBYMEMBER_RO LOLWUT XSETID SUBSTR`  | 

For more information, see [ElastiCache for Redis Version 5\.0\.3 \(Enhanced\)](supported-engine-versions.md#redis-version-5-0.3)\. 

## Redis 5\.0\.0 Parameter Changes<a name="ParameterGroups.Redis.5.0"></a>

**Parameter group family:** redis5\.0

Redis 5\.0 default parameter groups
+ `default.redis5.0` – Use this parameter group, or one derived from it, for Redis \(cluster mode disabled\) clusters and replication groups\.
+ `default.redis5.0.cluster.on` – Use this parameter group, or one derived from it, for Redis \(cluster mode enabled\) clusters and replication groups\.


**Parameters added in Redis 5\.0**  

|  Name  |  Details |  Description  | 
| --- | --- | --- | 
| stream\-node\-max\-bytes |  Permitted values: 0\+ Default: 4096 Type: integer Modifiable: Yes Changes take effect: Immediately | The stream data structure is a radix tree of nodes that encode multiple items inside\. Use this configuration to specify the maximum size of a single node in radix tree in Bytes\. If set to 0, the size of the tree node is unlimited\.  | 
| stream\-node\-max\-entries |  Permitted values: 0\+ Default: 100 Type: integer Modifiable: Yes Changes take effect: Immediately | The stream data structure is a radix tree of nodes that encode multiple items inside\. Use this configuration to specify the maximum number of items a single node can contain before switching to a new node when appending new stream entries\. If set to 0, the number of items in the tree node is unlimited  | 
| active\-defrag\-max\-scan\-fields |  Permitted values: 1 to 1000000 Default: 1000 Type: integer Modifiable: Yes Changes take effect: Immediately | Maximum number of set/hash/zset/list fields that will be processed from the main dictionary scan  | 
| lua\-replicate\-commands |  Permitted values: yes/no Default: yes Type: boolean Modifiable: Yes Changes take effect: Immediately | Always enable Lua effect replication or not in Lua scripts  | 
| replica\-ignore\-maxmemory |  Default: yes Type: boolean Modifiable: No  | Determines if replica ignores maxmemory setting by not evicting items independent from the master  | 

Redis has renamed several parameters in engine version 5\.0 in response to community feedback\. For more information, see [What's New in Redis 5?](https://aws.amazon.com/redis/Whats_New_Redis5/)\. The following table lists the new names and how they map to previous versions\.


**Parameters renamed in Redis 5\.0**  

|  Name  |  Details |  Description  | 
| --- | --- | --- | 
| replica\-lazy\-flush |  Default: no Type: boolean Modifiable: No Former name: slave\-lazy\-flush  | Performs an asynchronous flushDB during replica sync\. | 
| client\-output\-buffer\-limit\-replica\-hard\-limit | Default: For values see [Redis Node\-Type Specific Parameters](#ParameterGroups.Redis.NodeSpecific) Type: integer Modifiable: No Former name: client\-output\-buffer\-limit\-slave\-hard\-limit | For Redis read replicas: If a client's output buffer reaches the specified number of bytes, the client will be disconnected\. | 
| client\-output\-buffer\-limit\-replica\-soft\-limit | Default: For values see [Redis Node\-Type Specific Parameters](#ParameterGroups.Redis.NodeSpecific) Type: integer Modifiable: No Former name: client\-output\-buffer\-limit\-slave\-soft\-limit | For Redis read replicas: If a client's output buffer reaches the specified number of bytes, the client will be disconnected, but only if this condition persists for client\-output\-buffer\-limit\-replica\-soft\-seconds\. | 
| client\-output\-buffer\-limit\-replica\-soft\-seconds | Default: 60 Type: integer Modifiable: No Former name: client\-output\-buffer\-limit\-slave\-soft\-seconds  | For Redis read replicas: If a client's output buffer remains at client\-output\-buffer\-limit\-replica\-soft\-limit bytes for longer than this number of seconds, the client will be disconnected\. | 
| replica\-allow\-chaining | Default: no Type: string Modifiable: No Former name: slave\-allow\-chaining | Determines whether a read replica in Redis can have read replicas of its own\. | 
| min\-replicas\-to\-write | Default: 0 Type: integer Modifiable: Yes Former name: min\-slaves\-to\-write Changes Take Effect: Immediately | The minimum number of read replicas which must be available in order for the primary node to accept writes from clients\. If the number of available replicas falls below this number, then the primary node will no longer accept write requests\. If either this parameter or min\-replicas\-max\-lag is 0, then the primary node will always accept writes requests, even if no replicas are available\. | 
| min\-replicas\-max\-lag  | Default: 10 Type: integer Modifiable: Yes Former name: min\-slaves\-max\-lag Changes Take Effect: Immediately | The number of seconds within which the primary node must receive a ping request from a read replica\. If this amount of time passes and the primary does not receive a ping, then the replica is no longer considered available\. If the number of available replicas drops below min\-replicas\-to\-write, then the primary will stop accepting writes at that point\. If either this parameter or min\-replicas\-to\-write is 0, then the primary node will always accept write requests, even if no replicas are available\. | 
| close\-on\-replica\-write  | Default: yes Type: boolean Modifiable: Yes Former name: close\-on\-slave\-write Changes Take Effect: Immediately | If enabled, clients who attempt to write to a read\-only replica will be disconnected\. | 


**Parameters removed in Redis 5\.0**  

|  Name  |  Details |  Description  | 
| --- | --- | --- | 
| repl\-timeout |  Default: 60 Modifiable: No  | Parameter is not available in this version\. | 

## Redis 4\.0\.10 Parameter Changes<a name="ParameterGroups.Redis.4-0-10"></a>

**Parameter group family:** redis4\.0

Redis 4\.0\.x default parameter groups
+ `default.redis4.0` – Use this parameter group, or one derived from it, for Redis \(cluster mode disabled\) clusters and replication groups\.
+ `default.redis4.0.cluster.on` – Use this parameter group, or one derived from it, for Redis \(cluster mode enabled\) clusters and replication groups\.


**Parameters changed in Redis 4\.0\.10**  

|  Name  |  Details |  Description  | 
| --- | --- | --- | 
| maxmemory\-policy |  Permitted values: `allkeys-lru`, `volatile-lru`, **allkeys\-lfu**, **volatile\-lfu**, `allkeys-random`, `volatile-random`, `volatile-ttl`, `noeviction` Default: volatile\-lru Type: string Modifiable: Yes Changes take place: immediately | maxmemory\-policy was added in version 2\.6\.13\. In version 4\.0\.10 two new permitted values are added: allkeys\-lfu, which will evict any key using approximated LFU, and volatile\-lfu, which will evict using approximated LFU among the keys with an expire set\.  | 


**Parameters Added in Redis 4\.0\.10**  

|  Name  |  Details |  Description  | 
| --- | --- | --- | 
| Async deletion parameters | 
| lazyfree\-lazy\-eviction |  Permitted values: yes/no Default: no Type: boolean Modifiable: Yes Changes take place: immediately | Performs an asynchronous delete on evictions\. | 
| lazyfree\-lazy\-expire |  Permitted values: yes/no Default: no Type: boolean Modifiable: Yes Changes take place: immediately | Performs an asynchronous delete on expired keys\. | 
| lazyfree\-lazy\-server\-del |  Permitted values: yes/no Default: no Type: boolean Modifiable: Yes Changes take place: immediately | Performs an asynchronous delete for commands which update values\. | 
| slave\-lazy\-flush |  Permitted values: N/A Default: no Type: boolean Modifiable: No Changes take place: N/A | Performs an asynchronous flushDB during slave sync\. | 
| LFU parameters | 
| lfu\-log\-factor |  Permitted values: any integer > 0 Default: 10 Type: integer Modifiable: Yes Changes take place: immediately | Set the log factor, which determines the number of key hits to saturate the key counter\. | 
| lfu\-decay\-time |  Permitted values: any integer Default: 1 Type: integer Modifiable: Yes Changes take place: immediately | The amount of time in minutes to decrement the key counter\. | 
| Active defragmentation parameters | 
| activedefrag |  Permitted values: yes/no Default: no Type: boolean Modifiable: Yes Changes take place: immediately | Enabled active defragmentation\. | 
| active\-defrag\-ignore\-bytes |  Permitted values: 10485760\-104857600 Default: 104857600 Type: integer Modifiable: Yes Changes take place: immediately | Minimum amount of fragmentation waste to start active defrag\. | 
| active\-defrag\-threshold\-lower |  Permitted values: 1\-100 Default: 10 Type: integer Modifiable: Yes Changes take place: immediately | Minimum percentage of fragmentation to start active defrag\. | 
| active\-defrag\-threshold\-upper |  Permitted values: 1\-100 Default: 100 Type: integer Modifiable: Yes Changes take place: immediately | Maximum percentage of fragmentation at which we use maximum effort\. | 
| active\-defrag\-cycle\-min |  Permitted values: 1\-75 Default: 25 Type: integer Modifiable: Yes Changes take place: immediately | Minimal effort for defrag in CPU percentage\. | 
| active\-defrag\-cycle\-max |  Permitted values: 1\-75 Default: 75 Type: integer Modifiable: Yes Changes take place: immediately | Maximal effort for defrag in CPU percentage\. | 
| Client output buffer parameters | 
| client\-query\-buffer\-limit |  Permitted values: 1048576\-1073741824 Default: 1073741824 Type: integer Modifiable: Yes Changes take place: immediately | Max size of a single client query buffer\. | 
| proto\-max\-bulk\-len |  Permitted values: 1048576\-536870912 Default: 536870912 Type: integer Modifiable: Yes Changes take place: immediately | Max size of a single element request\. | 

## Redis 3\.2\.10 Parameter Changes<a name="ParameterGroups.Redis.3-2-10"></a>

**Parameter group family: **redis3\.2

ElastiCache for Redis 3\.2\.10 there are no additional parameters supported\.

## Redis 3\.2\.6 Parameter Changes<a name="ParameterGroups.Redis.3-2-6"></a>

**Parameter group family: **redis3\.2

For Redis 3\.2\.6 there are no additional parameters supported\.

## Redis 3\.2\.4 Parameter Changes<a name="ParameterGroups.Redis.3-2-4"></a>

**Parameter group family:** redis3\.2

Beginning with Redis 3\.2\.4 there are two default parameter groups\.
+ `default.redis3.2` – When running Redis 3\.2\.4, specify this parameter group or one derived from it, if you want to create a Redis \(cluster mode disabled\) replication group and still use the additional features of Redis 3\.2\.4\.
+ `default.redis3.2.cluster.on` – Specify this parameter group or one derived from it, when you want to create a Redis \(cluster mode enabled\) replication group\.

**Topics**
+ [New Parameters for Redis 3\.2\.4](#ParameterGroups.Redis.3-2-4.New)
+ [Parameters Changed in Redis 3\.2\.4 \(Enhanced\)](#ParameterGroups.Redis.3-2-4.Changed)

### New Parameters for Redis 3\.2\.4<a name="ParameterGroups.Redis.3-2-4.New"></a>

**Parameter group family:** redis3\.2

For Redis 3\.2\.4 the following additional parameters are supported\.


****  

|  Name  |  Details |  Description  | 
| --- | --- | --- | 
| list\-max\-ziplist\-size | Default: \-2 Type: integer Modifiable: No  | Lists are encoded in a special way to save space\. The number of entries allowed per internal list node can be specified as a fixed maximum size or a maximum number of elements\. For a fixed maximum size, use \-5 through \-1, meaning: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/ParameterGroups.Redis.html) | 
| list\-compress\-depth | Default: 0 Type: integer Modifiable: Yes Changes Take Effect: Immediately | Lists may also be compressed\. Compress depth is the number of quicklist ziplist nodes from each side of the list to exclude from compression\. The head and tail of the list are always uncompressed for fast push and pop operations\. Settings are: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/ParameterGroups.Redis.html) | 
| cluster\-enabled |  Default: no/yes \* Type: string Modifiable: Yes | Indicates whether this is a Redis \(cluster mode enabled\) replication group in cluster mode \(yes\) or a Redis \(cluster mode enabled\) replication group in non\-cluster mode \(no\)\. Redis \(cluster mode enabled\) replication groups in cluster mode can partition their data across up to 90 node groups\. \* Redis 3\.2\.*x* has two default parameter groups\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/ParameterGroups.Redis.html)\. | 
| cluster\-require\-full\-coverage | Default: no Type: boolean Modifiable: yes Changes Take Effect: Immediately |  When set to `yes`, Redis \(cluster mode enabled\) nodes in cluster mode stop accepting queries if they detect there is at least one hash slot uncovered \(no available node is serving it\)\. This way if the cluster is partially down, the cluster becomes unavailable\. It automatically becomes available again as soon as all the slots are covered again\. However, sometimes you want the subset of the cluster which is working to continue to accept queries for the part of the key space that is still covered\. To do so, just set the `cluster-require-full-coverage` option to `no`\. | 
| hll\-sparse\-max\-bytes | Default: 3000 Type: integer Modifiable: Yes Changes Take Effect: Immediately | HyperLogLog sparse representation bytes limit\. The limit includes the 16 byte header\. When a HyperLogLog using the sparse representation crosses this limit, it is converted into the dense representation\. A value greater than 16000 is not recommended, because at that point the dense representation is more memory efficient\. We recommend a value of about 3000 to have the benefits of the space\-efficient encoding without slowing down PFADD too much, which is O\(N\) with the sparse encoding\. The value can be raised to \~10000 when CPU is not a concern, but space is, and the data set is composed of many HyperLogLogs with cardinality in the 0 \- 15000 range\. | 
| reserved\-memory\-percent | Default: 25 Type: integer Modifiable: Yes Changes Take Effect: Immediately |  The percent of a node's memory reserved for nondata use\. By default, the Redis data footprint grows until it consumes all of the node's memory\. If this occurs, then node performance will likely suffer due to excessive memory paging\. By reserving memory, you can set aside some of the available memory for non\-Redis purposes to help reduce the amount of paging\. This parameter is specific to ElastiCache, and is not part of the standard Redis distribution\. For more information, see `reserved-memory` and [Managing Reserved Memory](redis-memory-management.md)\. | 

### Parameters Changed in Redis 3\.2\.4 \(Enhanced\)<a name="ParameterGroups.Redis.3-2-4.Changed"></a>

**Parameter group family:** redis3\.2

For Redis 3\.2\.4 the following parameters were changed\.


****  

|  Name  |  Details |  Change  | 
| --- | --- | --- | 
| activerehashing | Modifiable: Yes | Modifiable was No\. | 
| databases | Modifiable: Yes if the parameter group is not associated with any cache clusters\. Otherwise, no\. | Modifiable was No\. | 
| appendonly | Default: off Modifiable: No | If you want to upgrade from an earlier Redis version, you must first turn `appendonly` off\. | 
| appendfsync | Default: off Modifiable: No | If you want to upgrade from an earlier Redis version, you must first turn `appendfsync` off\. | 
| repl\-timeout | Default: 60 Modifiable: No | Is now unmodifiable with a default of 60\. | 
| tcp\-keepalive | Default: 300 | Default was 0\. | 
| list\-max\-ziplist\-entries |  | Parameter is no longer available\. | 
| list\-max\-ziplist\-value |  | Parameter is no longer available\. | 

## Redis 2\.8\.24 \(Enhanced\) Added Parameters<a name="ParameterGroups.Redis.2-8-24"></a>

**Parameter group family:** redis2\.8

For Redis 2\.8\.24 there are no additional parameters supported\.

## Redis 2\.8\.23 \(Enhanced\) Added Parameters<a name="ParameterGroups.Redis.2-8-23"></a>

**Parameter group family:** redis2\.8

For Redis 2\.8\.23 the following additional parameter is supported\.


****  

|  Name  |  Details |  Description  | 
| --- | --- | --- | 
| close\-on\-slave\-write  | Default: yes Type: string \(yes/no\) Modifiable: Yes Changes Take Effect: Immediately | If enabled, clients who attempt to write to a read\-only replica will be disconnected\. | 

### How close\-on\-slave\-write works<a name="w38aac18c46c57c25b9"></a>

The `close-on-slave-write` parameter is introduced by Amazon ElastiCache to give you more control over how your cluster responds when a primary node and a read replica node swap roles due to promoting a read replica to primary\.

![\[Image: close-on-replica-write, everything working fine\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-close-on-slave-write-01.png)

If the read\-replica cluster is promoted to primary for any reason other than a Multi\-AZ enabled replication group failing over, the client will continue trying to write to endpoint A\. Because endpoint A is now the endpoint for a read\-replica, these writes will fail\. This is the behavior for Redis before ElastiCache introducing `close-on-replica-write` and the behavior if you disable `close-on-replica-write`\.

![\[Image: close-on-slave-write, writes failing\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-close-on-slave-write-02.png)

With `close-on-replica-write` enabled, any time a client attempts to write to a read\-replica, the client connection to the cluster is closed\. Your application logic should detect the disconnection, check the DNS table, and reconnect to the primary endpoint, which now would be endpoint B\.

![\[Image: close-on-slave-write, writing to new primary cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-close-on-slave-write-03.png)

### When You Might Disable close\-on\-replica\-write<a name="w38aac18c46c57c25c11"></a>

If disabling `close-on-replica-write` results in writes to the failing cluster, why disable `close-on-replica-write`?

As previously mentioned, with `close-on-replica-write` enabled, any time a client attempts to write to a read\-replica the client connection to the cluster is closed\. Establishing a new connection to the node takes time\. Thus, disconnecting and reconnecting as a result of a write request to the replica also affects the latency of read requests that are served through the same connection\. This effect remains in place until a new connection is established\. If your application is especially read\-heavy or very latency\-sensitive, you might keep your clients connected to avoid degrading read performance\. 

## Redis 2\.8\.22 \(Enhanced\) Added Parameters<a name="ParameterGroups.Redis.2-8-22"></a>

**Parameter group family:** redis2\.8

For Redis 2\.8\.22 there are no additional parameters supported\.

**Important**  
Beginning with Redis version 2\.8\.22, `repl-backlog-size` applies to the primary cluster as well as to replica clusters\.
Beginning with Redis version 2\.8\.22, the `repl-timeout` parameter is not supported\. If it is changed, ElastiCache will overwrite with the default \(60s\), as we do with `appendonly`\.

The following parameters are no longer supported\.
+ *appendonly*
+ *appendfsync*
+ *repl\-timeout*

## Redis 2\.8\.21 Added Parameters<a name="ParameterGroups.Redis.2-8-21"></a>

**Parameter group family:** redis2\.8

For Redis 2\.8\.21, there are no additional parameters supported\.

## Redis 2\.8\.19 Added Parameters<a name="ParameterGroups.Redis.2-8-19"></a>

**Parameter group family:** redis2\.8

For Redis 2\.8\.19 there are no additional parameters supported\.

## Redis 2\.8\.6 Added Parameters<a name="ParameterGroups.Redis.2-8-6"></a>

**Parameter group family:** redis2\.8

For Redis 2\.8\.6 the following additional parameters are supported\.


****  

|  Name  |  Details  |  Description  | 
| --- | --- | --- | 
| min\-slaves\-max\-lag  | Default: 10 Type: integer Modifiable: Yes Changes Take Effect: Immediately | The number of seconds within which the primary node must receive a ping request from a read replica\. If this amount of time passes and the primary does not receive a ping, then the replica is no longer considered available\. If the number of available replicas drops below min\-slaves\-to\-write, then the primary will stop accepting writes at that point\. If either this parameter or min\-slaves\-to\-write is 0, then the primary node will always accept writes requests, even if no replicas are available\. | 
| min\-slaves\-to\-write | Default: 0 Type: integer Modifiable: Yes Changes Take Effect: Immediately | The minimum number of read replicas which must be available in order for the primary node to accept writes from clients\. If the number of available replicas falls below this number, then the primary node will no longer accept write requests\. If either this parameter or min\-slaves\-max\-lag is 0, then the primary node will always accept writes requests, even if no replicas are available\. | 
| notify\-keyspace\-events | Default: \(an empty string\) Type: string Modifiable: Yes Changes Take Effect: Immediately | The types of keyspace events that Redis can notify clients of\. Each event type is represented by a single letter: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/ParameterGroups.Redis.html) You can have any combination of these event types\. For example, *AKE* means that Redis can publish notifications of all event types\. Do not use any characters other than those listed above; attempts to do so will result in error messages\. By default, this parameter is set to an empty string, meaning that keyspace event notification is disabled\. | 
| repl\-backlog\-size | Default: 1048576 Type: integer Modifiable: Yes Changes Take Effect: Immediately | The size, in bytes, of the primary node backlog buffer\. The backlog is used for recording updates to data at the primary node\. When a read replica connects to the primary, it attempts to perform a partial sync \(`psync`\), where it applies data from the backlog to catch up with the primary node\. If the `psync` fails, then a full sync is required\. The minimum value for this parameter is 16384\. Beginning with Redis 2\.8\.22, this parameter applies to the primary cluster as well as the read replicas\. | 
| repl\-backlog\-ttl | Default: 3600 Type: integer Modifiable: Yes Changes Take Effect: Immediately | The number of seconds that the primary node will retain the backlog buffer\. Starting from the time the last replica node disconnected, the data in the backlog will remain intact until `repl-backlog-ttl` expires\. If the replica has not connected to the primary within this time, then the primary will release the backlog buffer\. When the replica eventually reconnects, it will have to perform a full sync with the primary\. If this parameter is set to 0, then the backlog buffer will never be released\. | 
| repl\-timeout | Default: 60 Type: integer Modifiable: Yes Changes Take Effect: Immediately | Represents the timeout period, in seconds, for: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/ParameterGroups.Redis.html) | 

## Redis 2\.6\.13 Parameters<a name="ParameterGroups.Redis.2-6-13"></a>

**Parameter group family:** redis2\.6

Redis 2\.6\.13 was the first version of Redis supported by ElastiCache\. The following table shows the Redis 2\.6\.13 parameters that ElastiCache supports\.


****  

|  Name  |  Details  |  Description  | 
| --- | --- | --- | 
| activerehashing | Default: yes Type: string \(yes/no\) Modifiable: Yes Changes take place: At Creation | Determines whether to enable Redis' active rehashing feature\. The main hash table is rehashed ten times per second; each rehash operation consumes 1 millisecond of CPU time\. This value is set when you create the parameter group\. When assigning a new parameter group to a cluster, this value must be the same in both the old and new parameter groups\. | 
| appendonly | Default: no Type: string Modifiable: Yes Changes Take Effect: Immediately | Enables or disables Redis' append only file feature \(AOF\)\. AOF captures any Redis commands that change data in the cache, and is used to recover from certain node failures\.  The default value is *no*, meaning AOF is turned off\. Set this parameter to *yes* to enable AOF\. For more information, see [Mitigating Failures](FaultTolerance.md)\. Append Only Files \(AOF\) is not supported for cache\.t1\.micro and cache\.t2\.\* nodes\. For nodes of this type, the `appendonly` parameter value is ignored\.   For Multi\-AZ replication groups, AOF is not allowed\. | 
| appendfsync | Default: everysec Type: string Modifiable: Yes Changes Take Effect: Immediately | When appendonly is set to yes, controls how often the AOF output buffer is written to disk: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/ParameterGroups.Redis.html)  | 
| client\-output\-buffer\-limit\-normal\-hard\-limit | Default: 0 Type: integer Modifiable: Yes Changes Take Effect: Immediately | If a client's output buffer reaches the specified number of bytes, the client will be disconnected\. The default is zero \(no hard limit\)\. | 
| client\-output\-buffer\-limit\-normal\-soft\-limit | Default: 0 Type: integer Modifiable: Yes Changes Take Effect: Immediately | If a client's output buffer reaches the specified number of bytes, the client will be disconnected, but only if this condition persists for client\-output\-buffer\-limit\-normal\-soft\-seconds\. The default is zero \(no soft limit\)\. | 
| client\-output\-buffer\-limit\-normal\-soft\-seconds | Default: 0 Type: integer Modifiable: Yes Changes Take Effect: Immediately | If a client's output buffer remains at client\-output\-buffer\-limit\-normal\-soft\-limit bytes for longer than this number of seconds, the client will be disconnected\. The default is zero \(no time limit\)\. | 
| client\-output\-buffer\-limit\-pubsub\-hard\-limit | Default: 33554432 Type: integer Modifiable: Yes Changes Take Effect: Immediately | For Redis publish/subscribe clients: If a client's output buffer reaches the specified number of bytes, the client will be disconnected\. | 
| client\-output\-buffer\-limit\-pubsub\-soft\-limit | Default: 8388608 Type: integer Modifiable: Yes Changes Take Effect: Immediately | For Redis publish/subscribe clients: If a client's output buffer reaches the specified number of bytes, the client will be disconnected, but only if this condition persists for client\-output\-buffer\-limit\-pubsub\-soft\-seconds\. | 
| client\-output\-buffer\-limit\-pubsub\-soft\-seconds | Default: 60 Type: integer Modifiable: Yes Changes Take Effect: Immediately | For Redis publish/subscribe clients: If a client's output buffer remains at client\-output\-buffer\-limit\-pubsub\-soft\-limit bytes for longer than this number of seconds, the client will be disconnected\. | 
| client\-output\-buffer\-limit\-slave\-hard\-limit | Default: For values see [Redis Node\-Type Specific Parameters](#ParameterGroups.Redis.NodeSpecific) Type: integer Modifiable: No | For Redis read replicas: If a client's output buffer reaches the specified number of bytes, the client will be disconnected\. | 
| client\-output\-buffer\-limit\-slave\-soft\-limit | Default: For values see [Redis Node\-Type Specific Parameters](#ParameterGroups.Redis.NodeSpecific) Type: integer Modifiable: No | For Redis read replicas: If a client's output buffer reaches the specified number of bytes, the client will be disconnected, but only if this condition persists for client\-output\-buffer\-limit\-slave\-soft\-seconds\. | 
| client\-output\-buffer\-limit\-slave\-soft\-seconds | Default: 60 Type: integer Modifiable: No | For Redis read replicas: If a client's output buffer remains at client\-output\-buffer\-limit\-slave\-soft\-limit bytes for longer than this number of seconds, the client will be disconnected\. | 
| databases | Default: 16 Type: integer Modifiable: No Changes take place: At Creation | The number of logical partitions the databases is split into\. We recommend keeping this value low\. This value is set when you create the parameter group\. When assigning a new parameter group to a cluster, this value must be the same in both the old and new parameter groups\. | 
| hash\-max\-ziplist\-entries | Default: 512 Type: integer Modifiable: Yes Changes Take Effect: Immediately | Determines the amount of memory used for hashes\. Hashes with fewer than the specified number of entries are stored using a special encoding that saves space\. | 
| hash\-max\-ziplist\-value | Default: 64 Type: integer Modifiable: Yes Changes Take Effect: Immediately | Determines the amount of memory used for hashes\. Hashes with entries that are smaller than the specified number of bytes are stored using a special encoding that saves space\. | 
| list\-max\-ziplist\-entries | Default: 512 Type: integer Modifiable: Yes Changes Take Effect: Immediately | Determines the amount of memory used for lists\. Lists with fewer than the specified number of entries are stored using a special encoding that saves space\. | 
| list\-max\-ziplist\-value | Default: 64 Type: integer Modifiable: Yes Changes Take Effect: Immediately | Determines the amount of memory used for lists\. Lists with entries that are smaller than the specified number of bytes are stored using a special encoding that saves space\. | 
| lua\-time\-limit | Default: 5000 Type: integer Modifiable: No | The maximum execution time for a Lua script, in milliseconds, before ElastiCache takes action to stop the script\. If `lua-time-limit` is exceeded, all Redis commands will return an error of the form *\_\_\_\_\-BUSY*\. Since this state can cause interference with many essential Redis operations, ElastiCache will first issue a *SCRIPT KILL* command\. If this is unsuccessful, ElastiCache will forcibly restart Redis\. | 
| maxclients | Default: 65000 Type: integer Modifiable: No | The maximum number of clients that can be connected at one time\. | 
| maxmemory\-policy | Default: volatile\-lru Type: string Modifiable: Yes Changes Take Effect: Immediately | The eviction policy for keys when maximum memory usage is reached\. Valid values are: `volatile-lru \| allkeys-lru \| volatile-random \| allkeys-random \| volatile-ttl \| noeviction` For more information, see [Using Redis as an LRU cache](https://redis.io/topics/lru-cache)\. | 
| maxmemory\-samples | Default: 3 Type: integer Modifiable: Yes Changes Take Effect: Immediately | For least\-recently\-used \(LRU\) and time\-to\-live \(TTL\) calculations, this parameter represents the sample size of keys to check\. By default, Redis chooses 3 keys and uses the one that was used least recently\. | 
| reserved\-memory | Default: 0 Type: integer Modifiable: Yes Changes Take Effect: Immediately |  The total memory, in bytes, reserved for non\-data usage\. By default, the Redis node will grow until it consumes the node's `maxmemory` \(see [Redis Node\-Type Specific Parameters](#ParameterGroups.Redis.NodeSpecific)\)\. If this occurs, then node performance will likely suffer due to excessive memory paging\. By reserving memory you can set aside some of the available memory for non\-Redis purposes to help reduce the amount of paging\. This parameter is specific to ElastiCache, and is not part of the standard Redis distribution\. For more information, see `reserved-memory-percent` and [Managing Reserved Memory](redis-memory-management.md)\. | 
| set\-max\-intset\-entries | Default: 512 Type: integer Modifiable: Yes Changes Take Effect: Immediately | Determines the amount of memory used for certain kinds of sets \(strings that are integers in radix 10 in the range of 64 bit signed integers\)\. Such sets with fewer than the specified number of entries are stored using a special encoding that saves space\. | 
| slave\-allow\-chaining | Default: no Type: string Modifiable: No | Determines whether a read replica in Redis can have read replicas of its own\. | 
| slowlog\-log\-slower\-than | Default: 10000 Type: integer Modifiable: Yes Changes Take Effect: Immediately | The maximum execution time, in microseconds, for commands to be logged by the Redis Slow Log feature\. | 
| slowlog\-max\-len | Default: 128 Type: integer Modifiable: Yes Changes Take Effect: Immediately | The maximum length of the Redis Slow Log\. | 
| tcp\-keepalive | Default: 0 Type: integer Modifiable: Yes Changes Take Effect: Immediately | If this is set to a nonzero value \(N\), node clients are polled every N seconds to ensure that they are still connected\. With the default setting of 0, no such polling occurs\. Some aspects of this parameter changed in Redis version 3\.2\.4\. See [Parameters Changed in Redis 3\.2\.4 \(Enhanced\)](#ParameterGroups.Redis.3-2-4.Changed)\. | 
| timeout | Default: 0 Type: integer Modifiable: Yes Changes Take Effect: Immediately | The number of seconds a node waits before timing out\. Values are: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/ParameterGroups.Redis.html) | 
| zset\-max\-ziplist\-entries | Default: 128 Type: integer Modifiable: Yes Changes Take Effect: Immediately | Determines the amount of memory used for sorted sets\. Sorted sets with fewer than the specified number of elements are stored using a special encoding that saves space\. | 
| zset\-max\-ziplist\-value | Default: 64 Type: integer Modifiable: Yes Changes Take Effect: Immediately | Determines the amount of memory used for sorted sets\. Sorted sets with entries that are smaller than the specified number of bytes are stored using a special encoding that saves space\. | 

**Note**  
If you do not specify a parameter group for your Redis 2\.6\.13 cluster, then a default parameter group \(`default.redis2.6`\) will be used\. You cannot change the values of any parameters in the default parameter group; however, you can always create a custom parameter group and assign it to your cluster at any time\.

## Redis Node\-Type Specific Parameters<a name="ParameterGroups.Redis.NodeSpecific"></a>

Although most parameters have a single value, some parameters have different values depending on the node type used\. The following table shows the default values for the `maxmemory`, `client-output-buffer-limit-slave-hard-limit`, and `client-output-buffer-limit-slave-soft-limit` parameters for each node type\. The value of `maxmemory` is the maximum number of bytes available to you for use, data and other uses, on the node\.

**Note**  
The `maxmemory` parameter cannot be modified\.


|  Node Type  | maxmemory  | client\-output\-buffer\-limit\-slave\-hard\-limit | client\-output\-buffer\-limit\-slave\-soft\-limit | 
| --- | --- | --- | --- | 
| cache\.t1\.micro | 142606336 | 14260633 | 14260633 | 
| cache\.t2\.micro | 581959680 | 58195968 | 58195968 | 
| cache\.t2\.small | 1665138688 | 166513868 | 166513868 | 
| cache\.t2\.medium | 3461349376 | 346134937 | 346134937 | 
| cache\.t3\.micro | 536870912 | 53687091 | 53687091 | 
| cache\.t3\.small | 1471026299 | 147102629 | 147102629 | 
| cache\.t3\.medium | 3317862236 | 331786223 | 331786223 | 
| cache\.m1\.small | 943718400 | 94371840 | 94371840 | 
| cache\.m1\.medium | 3093299200 | 309329920 | 309329920 | 
| cache\.m1\.large | 7025459200 | 702545920 | 702545920 | 
| cache\.m1\.xlarge | 14889779200 | 1488977920 | 1488977920 | 
| cache\.m2\.xlarge | 17091788800 | 1709178880 | 1709178880 | 
| cache\.m2\.2xlarge | 35022438400 | 3502243840 | 3502243840 | 
| cache\.m2\.4xlarge | 70883737600 | 7088373760 | 7088373760 | 
| cache\.m3\.medium | 2988441600 | 309329920 | 309329920 | 
| cache\.m3\.large | 6501171200 | 650117120 | 650117120 | 
| cache\.m3\.xlarge | 14260633600 | 1426063360 | 1426063360 | 
| cache\.m3\.2xlarge | 29989273600 | 2998927360 | 2998927360 | 
| cache\.m4\.large | 6892593152 | 689259315 | 689259315 | 
| cache\.m4\.xlarge | 15328501760 | 1532850176 | 1532850176 | 
| cache\.m4\.2xlarge | 31889126359 | 3188912636 | 3188912636 | 
| cache\.m4\.4xlarge | 65257290629 | 6525729063 | 6525729063 | 
| cache\.m4\.10xlarge | 166047614239 | 16604761424 | 16604761424 | 
| cache\.m5\.large | 6854542746 | 685454275  | 685454275 | 
| cache\.m5\.xlarge | 13891921715 | 1389192172 | 1389192172 | 
| cache\.m5\.2xlarge | 27966669210 | 2796666921 | 2796666921 | 
| cache\.m5\.4xlarge | 56116178125 | 5611617812 | 5611617812 | 
| cache\.m5\.12xlarge | 168715971994 | 16871597199 | 16871597199 | 
| cache\.m5\.24xlarge | 337500562842 | 33750056284 | 33750056284 | 
| cache\.c1\.xlarge | 6501171200 | 650117120 | 650117120 | 
| cache\.r3\.large | 14470348800 | 1468006400 | 1468006400 | 
| cache\.r3\.xlarge | 30513561600 | 3040870400 | 3040870400 | 
| cache\.r3\.2xlarge | 62495129600 | 6081740800 | 6081740800 | 
| cache\.r3\.4xlarge | 126458265600 | 12268339200 | 12268339200 | 
| cache\.r3\.8xlarge | 254384537600 | 24536678400 | 24536678400 | 
| cache\.r4\.large | 13201781556 | 1320178155 | 1320178155 | 
| cache\.r4\.xlarge | 26898228839 | 2689822883 | 2689822883 | 
| cache\.r4\.2xlarge | 54197537997 | 5419753799 | 5419753799 | 
| cache\.r4\.4xlarge | 108858546586 | 10885854658 | 10885854658 | 
| cache\.r4\.8xlarge | 218255432090 | 21825543209 | 21825543209 | 
| cache\.r4\.16xlarge | 437021573120 | 43702157312 | 43702157312 | 
| cache\.r5\.large | 10527885773 | 10527885773 | 10527885773 | 
| cache\.r5\.xlarge | 28261849702 | 2826184970 | 2826184970 | 
| cache\.r5\.2xlarge | 56711183565 | 5671118356 | 5671118356 | 
| cache\.r5\.4xlarge | 113609865216 | 11360986522 | 11360986522 | 
| cache\.r5\.12xlarge | 341206346547 | 34120634655 | 34120634655 | 
| cache\.r5\.24xlarge | 682485973811 | 68248597381 | 68248597381 | 

**Note**  
All current generation instance types are created in an Amazon Virtual Private Cloud VPC by default\.  
T1 instances do not support Multi\-AZ\.  
T1 and T2 instances do not support Redis AOF\.  
Redis configuration variables `appendonly` and `appendfsync` are not supported on Redis version 2\.8\.22 and later\.