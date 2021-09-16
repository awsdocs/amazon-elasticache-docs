# Metrics for Redis<a name="CacheMetrics.Redis"></a>

The `AWS/ElastiCache` namespace includes the following Redis metrics\.

With the exception of `ReplicationLag` and `EngineCPUUtilization`, these metrics are derived from the Redis info command\. Each metric is calculated at the cache node level\.

For complete documentation of the Redis info command, see [ http://redis\.io/commands/info](http://redis.io/commands/info)\. 

**See Also**
+ [Host\-Level Metrics](CacheMetrics.HostLevel.md)


| Metric  | Description  | Unit  | 
| --- | --- | --- | 
| ActiveDefragHits | The number of value reallocations per minute performed by the active defragmentation process\. This is derived from active\_defrag\_hits statistic at [Redis INFO](http://redis.io/commands/info)\. | Number | 
| AuthenticationFailures | The total number of failed attempts to authenticate to Redis using the AUTH command\. You can find more information about individual authentication failures using the [ACL LOG](https://redis.io/commands/acl-log) command\. We suggest setting an alarm on this to detect unauthorized access attempts\.  | Count | 
| BytesUsedForCache | The total number of bytes allocated by Redis for all purposes, including the dataset, buffers, and so on\. This is derived from used\_memory statistic at [Redis INFO](http://redis.io/commands/info)\. | Bytes | 
| CacheHits | The number of successful read\-only key lookups in the main dictionary\. This is derived from keyspace\_hits statistic at [Redis INFO](http://redis.io/commands/info)\. | Count | 
| CacheMisses | The number of unsuccessful read\-only key lookups in the main dictionary\. This is derived from keyspace\_misses statistic at [Redis INFO](http://redis.io/commands/info)\. | Count | 
| CommandAuthorizationFailures | The total number of failed attempts by users to run commands they don’t have permission to call\. You can find more information about individual authentication failures using the [ACL LOG](https://redis.io/commands/acl-log) command\. We suggest setting an alarm on this to detect unauthorized access attempts\.  | Count | 
| CacheHitRate | Indicates the usage efficiency of the Redis instance\. If the cache ratio is lower than about 0\.8, it means that a significant amount of keys are evicted, expired, or don't exist\. This is calculated using cache\_hits and cache\_misses statistics in the following way: cache\_hits /\(cache\_hits \+ cache\_misses\)\. | Percent | 
| CurrConnections | The number of client connections, excluding connections from read replicas\. ElastiCache uses two to four of the connections to monitor the cluster in each case\. This is derived from the connected\_clients statistic at [Redis INFO](http://redis.io/commands/info)\. | Count | 
| DatabaseMemoryUsagePercentage |  Percentage of the memory available for the cluster that is in use\. This is calculated using used\_memory/maxmemory from [Redis INFO](http://redis.io/commands/info)\.  | Percent | 
| DB0AverageTTL |  Exposes avg\_ttl of DBO from the keyspace statistic of [Redis INFO](http://redis.io/commands/info) command\.  | Milliseconds | 
| EngineCPUUtilization | Provides CPU utilization of the Redis engine thread\. Because Redis is single\-threaded, you can use this metric to analyze the load of the Redis process itself\. The `EngineCPUUtilization` metric provides a more precise visibility of the Redis process\. You can use it in conjunction with the `CPUUtilization` metric\. `CPUUtilization` exposes CPU utilization for the server instance as a whole, including other operating system and management processes\. For larger node types with four vCPUs or more, use the `EngineCPUUtilization` metric to monitor and set thresholds for scaling\.  On an ElastiCache host, background processes monitor the host to provide a managed database experience\. These background processes can take up a significant portion of the CPU workload\. This is not significant on larger hosts with more than two vCPUs\. But it can affect smaller hosts with 2vCPUs or fewer\. If you only monitor the `EngineCPUUtilization` metric, you will be unaware of situations where the host is overloaded with both high CPU usage from Redis and high CPU usage from the background monitoring processes\. Therefore, we recommend monitoring the `CPUUtilization` metric for hosts with two vCPUs or less\.   | Percent | 
| Evictions | The number of keys that have been evicted due to the maxmemory limit\. This is derived from the evicted\_keys statistic at [Redis INFO](http://redis.io/commands/info)\. | Count | 
| GlobalDatastoreReplicationLag | This is the lag between the secondary Region's primary node and the primary Region's primary node\. For cluster mode enabled Redis, the lag indicates the maximum delay among the shards\.  | Seconds | 
| IsPrimary | Indicates whether the node is the primary node of current shard/cluster\. The metric can be either 0 \(not primary\) or 1 \(primary\)\.  | Count | 
| KeyAuthorizationFailures | The total number of failed attempts by users to access keys they don’t have permission to access\. You can find more information about individual authentication failures using the [ACL LOG](https://redis.io/commands/acl-log) command\. We suggest setting an alarm on this to detect unauthorized access attempts\.  | Count | 
|  KeysTracked |  The number of keys being tracked by Redis key tracking as a percentage of tracking\-table\-max\-keys\. Key tracking is used to aid client\-side caching and notifies clients when keys are modified\.  | Count | 
| PrimaryLinkHealthStatus | This status has two values: 0 or 1\. The value 0 indicates that data in the ElastiCache primary node is not in sync with Redis on EC2\. The value of 1 indicates that the data is in sync\. To complete the migration, use the [CompleteMigration](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CompleteMigration.html) API operation\. | Boolean | 
| MemoryFragmentationRatio |  Indicates the efficiency in the allocation of memory of the Redis engine\. Certain thresholds signify different behaviors\. The recommended value is to have fragmentation above 1\.0\. This is calculated from the mem\_fragmentation\_ratio statistic of [Redis INFO](http://redis.io/commands/info)\.  | Number | 
| NewConnections | The total number of connections that have been accepted by the server during this period\. This is derived from the total\_connections\_received statistic at [Redis INFO](http://redis.io/commands/info)\. | Count | 
| Reclaimed | The total number of key expiration events\. This is derived from the expired\_keys statistic at [Redis INFO](http://redis.io/commands/info)\. | Count | 
| ReplicationBytes | For nodes in a replicated configuration, ReplicationBytes reports the number of bytes that the primary is sending to all of its replicas\. This metric is representative of the write load on the replication group\. This is derived from the master\_repl\_offset statistic at [Redis INFO](http://redis.io/commands/info)\.  | Bytes | 
| ReplicationLag | This metric is only applicable for a node running as a read replica\. It represents how far behind, in seconds, the replica is in applying changes from the primary node\. For Redis engine version 5\.0\.6 onwards, the lag can be measured in milliseconds\. | Seconds | 
| SaveInProgress | This binary metric returns 1 whenever a background save \(forked or forkless\) is in progress, and 0 otherwise\. A background save process is typically used during snapshots and syncs\. These operations can cause degraded performance\. Using the SaveInProgress metric, you can diagnose whether degraded performance was caused by a background save process\. This is derived from the rdb\_bgsave\_in\_progress statistic at [Redis INFO](http://redis.io/commands/info)\. | Count | 

**EngineCPUUtilization availability**  
AWS Regions listed following are available on all supported node types\.


| Region | Region name | 
| --- | --- | 
| us\-east\-2 | US East \(Ohio\) | 
| us\-east\-1 | US East \(N\. Virginia\) | 
| us\-west\-1 | US West \(N\. California\) | 
| us\-west\-2 | US West \(Oregon\) | 
| ap\-northeast\-1 | Asia Pacific \(Tokyo\) | 
| ap\-northeast\-2 | Asia Pacific \(Seoul\) | 
| ap\-northeast\-3 | Asia Pacific \(Osaka\) | 
| ap\-east\-1 | Asia Pacific \(Hong Kong\) | 
| ap\-south\-1 | Asia Pacific \(Mumbai\) | 
| ap\-southeast\-1 | Asia Pacific \(Singapore\) | 
| ap\-southeast\-2 | Asia Pacific \(Sydney\) | 
| ca\-central\-1 | Canada \(Central\) | 
| cn\-north\-1 | China \(Beijing\) | 
| cn\-northwest\-2 | China \(Ningxia\) | 
| me\-south\-1 | Middle East \(Bahrain\) | 
| eu\-central\-1 | Europe \(Frankfurt\) | 
| eu\-west\-1 | Europe \(Ireland\) | 
| eu\-west\-2 | Europe \(London\) | 
| eu\-west\-3 | EU \(Paris\) | 
| eu\-south\-1 | Europe \(Milan\) | 
| af\-south\-1 | Africa \(Cape Town\) | 
| eu\-north\-1 | Europe \(Stockholm\) | 
| sa\-east\-1 | South America \(São Paulo\) | 
| us\-gov\-west\-1 | AWS GovCloud \(US\-West\) | 
| us\-gov\-east\-1 | AWS GovCloud \(US\-East\) | 

The following are aggregations of certain kinds of commands, derived from info commandstats\. The commandstats section provides statistics based on the command type, including the number of calls, the total CPU time consumed by these commands, and the average CPU consumed per command execution\. For each command type, the following line is added: `cmdstat_XXX: calls=XXX,usec=XXX,usec_per_call=XXX`\.

The latency metrics listed following are calculated using commandstats statistic from [Redis INFO](http://redis.io/commands/info)\. They are calculated in the following way: `delta(usec)/delta(calls)`\. `delta` is calculated as the diff within one minute\. 

For a full list of available commands, see [redis commands](https://redis.io/commands) in the Redis documentation\. 


| Metric  | Description  | Unit  | 
| --- | --- | --- | 
| CurrItems | The number of items in the cache\. This is derived from the Redis keyspace statistic, summing all of the keys in the entire keyspace\. | Count | 
| EvalBasedCmds | The total number of commands for eval\-based commands\. This is derived from the Redis commandstats statistic by summing eval, evalsha\. | Count | 
| EvalBasedCmdsLatency | Latency of eval\-based commands\. | Microseconds | 
| GeoSpatialBasedCmds | The total number of commands for geospatial\-based commands\. This is derived from the Redis commandstats statistic\. It's derived by summing all of the geo type of commands: geoadd, geodist, geohash, geopos, georadius, and georadiusbymember\. | Count | 
| GeoSpatialBasedCmdsLatency | Latency of geospatial\-based commands\.  | Microseconds | 
| GetTypeCmds | The total number of read\-only type commands\. This is derived from the Redis commandstats statistic by summing all of the read\-only type commands \(get, hget, scard, lrange, and so on\.\) | Count | 
|  GetTypeCmdsLatency |  Latency of read commands\.  | Microseconds | 
| HashBasedCmds | The total number of commands that are hash\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more hashes \(hget, hkeys, hvals, hdel, and so on\)\. | Count | 
|  HashBasedCmdsLatency |  Latency of hash\-based commands\.  | Microseconds | 
| HyperLogLogBasedCmds | The total number of HyperLogLog\-based commands\. This is derived from the Redis commandstats statistic by summing all of the pf type of commands \(pfadd, pfcount, pfmerge, and so on\.\)\. | Count | 
|  HyperLogLogBasedCmdsLatency |  Latency of HyperLogLog\-based commands\.  | Microseconds | 
| KeyBasedCmds | The total number of commands that are key\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more keys across multiple data structures \(del, expire, rename, and so on\.\)\. | Count | 
|  KeyBasedCmdsLatency |  Latency of key\-based commands\.  | Microseconds | 
| ListBasedCmds | The total number of commands that are list\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more lists \(lindex, lrange, lpush, ltrim, and so on\)\. | Count | 
|  ListBasedCmdsLatency |  Latency of list\-based commands\.  | Microseconds | 
| PubSubBasedCmds | The total number of commands for pub/sub functionality\. This is derived from the Redis commandstatsstatistics by summing all of the commands used for pub/sub functionality: psubscribe, publish, pubsub, punsubscribe, subscribe, and unsubscribe\. | Count | 
| PubSubBasedCmdsLatency | Latency of pub/sub\-based commands\. | Microseconds | 
| SetBasedCmds | The total number of commands that are set\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more sets \(scard, sdiff, sadd, sunion, and so on\)\. | Count | 
|  SetBasedCmdsLatency |  Latency of set\-based commands\.  | Microseconds | 
| SetTypeCmds | The total number of write types of commands\. This is derived from the Redis commandstats statistic by summing all of the mutative types of commands that operate on data \(set, hset, sadd, lpop, and so on\.\) | Count | 
|  SetTypeCmdsLatency |  Latency of write commands\.  | Microseconds | 
| SortedSetBasedCmds | The total number of commands that are sorted set\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more sorted sets \(zcount, zrange, zrank, zadd, and so on\)\. | Count | 
|  SortedSetBasedCmdsLatency |  Latency of sorted\-based commands\.  | Microseconds | 
| StringBasedCmds | The total number of commands that are string\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more strings \(strlen, setex, setrange, and so on\)\. | Count | 
|  StringBasedCmdsLatency |  Latency of string\-based commands\.  | Microseconds | 
| StreamBasedCmds | The total number of commands that are stream\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more streams data types \(xrange, xlen, xadd, xdel, and so on\)\. | Count | 
|  StreamBasedCmdsLatency |  Latency of stream\-based commands\.  | Microseconds | 