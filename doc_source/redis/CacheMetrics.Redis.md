# Metrics for Redis<a name="CacheMetrics.Redis"></a>

The `AWS/ElastiCache` namespace includes the following Redis metrics\.

With the exception of `ReplicationLag` and `EngineCPUUtilization`, these metrics are derived from the Redis info command\. Each metric is calculated at the cache node level\.

For complete documentation of the Redis info command, see [ http://redis\.io/commands/info](http://redis.io/commands/info)\. 

**See Also**
+ [Host\-Level Metrics](CacheMetrics.HostLevel.md)

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/CacheMetrics.Redis.html)

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
| ap\-southeast\-3 | Asia Pacific \(Jakarta\) | 
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

The latency metrics listed following are calculated using commandstats statistic from [Redis INFO](http://redis.io/commands/info)\. They are calculated in the following way: `delta(usec)/delta(calls)`\. `delta` is calculated as the diff within one minute\. Latency is defined as CPU time taken by ElastiCache to process the command\. Note that for clusters using data tiering, the time taken to fetch items from SSD is not included in these measurements\.

For a full list of available commands, see [redis commands](https://redis.io/commands) in the Redis documentation\. 


| Metric  | Description  | Unit  | 
| --- | --- | --- | 
| ClusterBasedCmds | The total number of commands that are cluster\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon a cluster \(cluster slot, cluster info, and so on\)\.  | Count | 
| ClusterBasedCmdsLatency | Latency of cluster\-based commands\.  | Microseconds | 
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
| JsonBasedCmds | The total number of commands that are JSON\-based\. This is derived from the Redis commandstats statistic by summing all of the commands that act upon one or more JSON document objects\.  | Count | 
| JsonBasedCmdsLatency | Exposes the aggregate latency \(server side CPU time\) calculated as Delta\[Usec\]/Delta\[Calls\] of all commands that act upon one or more JSON document objects\.  | Microseconds | 
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