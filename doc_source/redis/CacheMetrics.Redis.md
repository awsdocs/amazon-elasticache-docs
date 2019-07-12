# Metrics for Redis<a name="CacheMetrics.Redis"></a>

The `AWS/ElastiCache` namespace includes the following Redis metrics\.

With the exception of `ReplicationLag` and `EngineCPUUtilization`, these metrics are derived from the Redis info command\. Each metric is calculated at the cache node level\.

For complete documentation of the Redis info command, see [ http://redis\.io/commands/info](http://redis.io/commands/info)\. 

**See Also**
+ [Host\-Level Metrics](CacheMetrics.HostLevel.md)


| Metric  | Description  | Unit  | 
| --- | --- | --- | 
| ActiveDefragHits | The number of value reallocations per minute performed by the active defragmentation process\. This is derived from active\_defrag\_hits statistic at [Redis INFO](http://redis.io/commands/info)\. | Number | 
| BytesUsedForCache | The total number of bytes allocated by Redis for all purposes, including the dataset, buffers, etc\. This is derived from used\_memory statistic at [Redis INFO](http://redis.io/commands/info)\. | Bytes | 
| CacheHits | The number of successful read\-only key lookups in the main dictionary\. This is derived from keyspace\_hits statistic at [Redis INFO](http://redis.io/commands/info)\. | Count | 
| CacheMisses | The number of unsuccessful read\-only key lookups in the main dictionary\. This is derived from keyspace\_misses at [Redis INFO](http://redis.io/commands/info)\.  | Count | 
| CurrConnections | The number of client connections, excluding connections from read replicas\. ElastiCache uses two to three of the connections to monitor the cluster in each case\. This is derived from the connected\_clients statistic at [Redis INFO](http://redis.io/commands/info)\. | Count | 
| EngineCPUUtilization | Provides CPU utilization of the Redis engine thread\. Since Redis is single\-threaded, you can use this metric to analyze the load of the Redis process itself\. The `EngineCPUUtilization` metric provides a more precise visibility of the Redis process and can be used in conjunction with `CPUUtilization` metric, which exposes CPU utilization for the server instance as a whole, including other operating system and management processes\. For larger node types with 4vCPUs or more, use the `EngineCPUUtilization` metric to monitor and set thresholds for scaling\. For smaller node types with 2vCPUs or less, use the `CPUUtilization` metric\.   | Percent | 
| Evictions | The number of keys that have been evicted due to the maxmemory limit\. This is derived from the evicted\_keys statistic at [Redis INFO](http://redis.io/commands/info)\. | Count | 
| NewConnections | The total number of connections that have been accepted by the server during this period\. This is derived from the total\_connections\_received statistic at [Redis INFO](http://redis.io/commands/info)\. | Count | 
| Reclaimed | The total number of key expiration events\. This is derived from the expired\_keys statistic at [Redis INFO](http://redis.io/commands/info)\. | Count | 
| ReplicationBytes | For nodes in a replicated configuration, ReplicationBytes reports the number of bytes that the primary is sending to all of its replicas\. This metric is representative of the write load on the replication group\. This is derived from the master\_repl\_offset statistic at [Redis INFO](http://redis.io/commands/info)\.  | Bytes | 
| ReplicationLag | This metric is only applicable for a node running as a read replica\. It represents how far behind, in seconds, the replica is in applying changes from the primary node\. | Seconds | 
| SaveInProgress | This binary metric returns 1 whenever a background save \(forked or forkless\) is in progress, and 0 otherwise\. A background save process is typically used during snapshots and syncs\. These operations can cause degraded performance\. Using the  SaveInProgress metric, you can diagnose whether or not degraded performance was caused by a background save process\. This is derived from the rdb\_bgsave\_in\_progress statistic at [Redis INFO](http://redis.io/commands/info)\. | Count | 

**EngineCPUUtilization availability**  
Nodes in a region created or replaced after the date and time specified in the following table will include the `EngineCPUUtilization` metric\.


| Region | Region name |  EngineCPUUtilization availability  | 
| --- | --- | --- | 
| us\-east\-2 | US East \(Ohio\) | February 16, 2017 | 17:21 \(UTC\) | 
| us\-east\-1 | US East \(N\. Virginia\) | February 8, 2017 | 21:20 \(UTC\) | 
| us\-west\-1 | US West \(N\. California\) | February 14, 2017 | 22:23 \(UTC\) | 
| us\-west\-2 | US West \(Oregon\) | Febrary 20, 2017 | 19:20 \(UTC\) | 
| ap\-northeast\-1 | Asia Pacific \(Tokyo\) | February 14, 2017 | 19:58 \(UTC\) | 
| ap\-northeast\-2 | Asia Pacific \(Seoul\) | Available on all nodes\. | 
| ap\-northeast\-3 | Asia Pacific \(Osaka\-Local\) | Available on all nodes\. | 
| ap\-south\-1 | Asia Pacific \(Mumbai\) | February 7, 2017 | 02:51 \(UTC\) | 
| ap\-southeast\-1 | Asia Pacific \(Singapore\) | February 13, 2017 | 23:40 \(UTC\) | 
| ap\-southeast\-2 | Asia Pacific \(Sydney\) | February 14, 2017 | 03:33 \(UTC\) | 
| ca\-central\-1 | Canada \(Central\) | Available on all nodes\. | 
| cn\-north\-1 | China \(Beijing\) | February 16, 2017 | 22:39 \(UTC\) | 
| cn\-northwest\-2 | China \(Ningxia\) | Available on all nodes\. | 
| eu\-central\-1 | EU \(Frankfurt\) | February 15, 2017 | 00:46 \(UTC\) | 
| eu\-west\-1 | EU \(Ireland\) | February 7, 2017 | 21:30 \(UTC\) | 
| eu\-west\-2 | EU \(London\) | February 16, 2017 | 18:58 \(UTC\) | 
| eu\-west\-3 | EU \(Paris\) | Available on all nodes\. | 
| sa\-east\-1 | South America \(São Paulo\) | February 7, 2017 | 04:35 \(UTC\) | 
| us\-gov\-west\-1 | AWS GovCloud \(US\-West\) | February 16, 2017 | 20:11 \(UTC\) | 

These are aggregations of certain kinds of commands, derived from info commandstats\. For a full list of available commands, see [redis commands](https://redis.io/commands)\.


| Metric  | Description  | Unit  | 
| --- | --- | --- | 
| CurrItems | The number of items in the cache\. This is derived from the Redis keyspace statistic, summing all of the keys in the entire keyspace\. | Count | 
| GetTypeCmds | The total number of read\-only type commands\. This is derived from the Redis commandstats statistic by summing all of the read\-only type commands \(get, hget, scard, lrange, etc\.\) | Count | 
| HashBasedCmds | The total number of commands that are hash\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more hashes \(hget, hkeys, hvals, hdel, etc\)\. | Count | 
| HyperLogLogBasedCmds | The total number of HyperLogLog\-based commands\. This is derived from the Redis commandstats statistic by summing all of the pf type of commands \(pfadd, pfcount, pfmerge, etc\.\)\. | Count | 
| KeyBasedCmds | The total number of commands that are key\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more keys across multiple data structures \(del, expire, rename, etc\.\)\. | Count | 
| ListBasedCmds | The total number of commands that are list\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more lists \(lindex, lrange, lpush, ltrim, etc\)\. | Count | 
| SetBasedCmds | The total number of commands that are set\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more sets \(scard,  sdiff,  sadd, sunion, etc\)\. | Count | 
| SetTypeCmds | The total number of write types of commands\. This is derived from the Redis commandstats statistic by summing all of the mutative types of commands that operate on data \(set, hset, sadd, lpop, etc\.\) | Count | 
| SortedSetBasedCmds | The total number of commands that are sorted set\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more sorted sets \(zcount, zrange, zrank, zadd, etc\)\. | Count | 
| StringBasedCmds | The total number of commands that are string\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more strings \(strlen, setex, setrange, etc\)\. | Count | 
| StreamBasedCmds | The total number of commands that are stream\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more streams data types \(xrange, xlen, xadd, xdel, etc\)\. | Count | 