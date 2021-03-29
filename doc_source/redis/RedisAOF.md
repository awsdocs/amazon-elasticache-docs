# Append only files \(AOF\) in ElastiCache for Redis<a name="RedisAOF"></a>

By default, the data in a Redis node on ElastiCache resides only in memory and isn't persistent\. If a node is rebooted, or if the underlying physical server experiences a hardware failure, the data in the cache is lost\.

If you require data durability, you can enable the Redis append\-only file feature \(AOF\)\. When this feature is enabled, the node writes all of the commands that change cache data to an append\-only file\. When a node is rebooted and the cache engine starts, the AOF is "replayed\." The result is a warm Redis cache with all of the data intact\.

AOF is disabled by default\. To enable AOF for a cluster running Redis, you must create a parameter group with the `appendonly` parameter set to yes\. You then assign that parameter group to your cluster\. You can also modify the `appendfsync` parameter to control how often Redis writes to the AOF file\.

**Important**  
Append\-only files \(AOF\) aren't supported for cache\.t1\.micro and cache\.t2\.\* nodes\. For nodes of these types, the `appendonly` parameter value is ignored\.  
For Multi\-AZ replication groups, AOF isn't enabled\.  
AOF isn't supported on Redis versions 2\.8\.22 and later\.

**Warning**  
AOF can't protect against all failure scenarios\. For example, if a node fails due to a hardware fault in an underlying physical server, ElastiCache provisions a new node on a different server\. In this case, the AOF file is no longer available and can't be used to recover the data\. Thus, Redis restarts with a cold cache\.  
For greater reliability and faster recovery, we recommend that you create one or more read replicas in different Availability Zones for your cluster\. Enable Multi\-AZ on your replication group instead of using AOF\. AOF isn't enabled for Multi\-AZ replication groups\.  
For more information on mitigating failures, see [Mitigating Failures when Running Redis](FaultTolerance.md#FaultTolerance.Redis)\.

For more information, see the following:
+ [Redis\-specific parameters](ParameterGroups.Redis.md)
+ [Minimizing downtime in ElastiCache for Redis with Multi\-AZ](AutoFailover.md)
+ [Mitigating Failures](FaultTolerance.md)