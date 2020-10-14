# Which Metrics Should I Monitor?<a name="CacheMetrics.WhichShouldIMonitor"></a>

The following CloudWatch metrics offer good insight into ElastiCache performance\. In most cases, we recommend that you set CloudWatch alarms for these metrics so that you can take corrective action before performance issues occur\.

**Topics**
+ [CPUUtilization](#metrics-cpu-utilization)
+ [EngineCPUUtilization](#metrics-engine-cpu-utilization)
+ [SwapUsage](#metrics-swap-usage)
+ [Evictions](#metrics-evictions)
+ [CurrConnections](#metrics-curr-connections)

## CPUUtilization<a name="metrics-cpu-utilization"></a>

This is a host\-level metric reported as a percentage\. For more information, see [Host\-Level Metrics](CacheMetrics.HostLevel.md)\.

 For smaller node types with 2vCPUs or less, use the `CPUUtilization ` metric to monitor your workload\.

Generally speaking, we suggest you set your threshold at 90% of your available CPU\. Because Redis is single\-threaded, the actual threshold value should be calculated as a fraction of the node's total capacity\. For example, suppose you are using a node type that has two cores\. In this case, the threshold for CPUUtilization would be 90/2, or 45%\. To find the number of cores \(vCPUs\) your node type has, see [Amazon ElastiCache Pricing](https://aws.amazon.com/elasticache/pricing/?p=ps)\.

You will need to determine your own threshold, based on the number of cores in the cache node that you are using\. If you exceed this threshold, and your main workload is from read requests, scale your cache cluster out by adding read replicas\. If the main workload is from write requests, depending on your cluster configuration, we recommend that you:
+ **Redis \(cluster mode disabled\) clusters:** scale up by using a larger cache instance type\.
+ **Redis \(cluster mode enabled\) clusters:** add more shards to distribute the write workload across more primary nodes\.

**Tip**  
Instead of using the Host\-Level metric `CPUUtilization`, Redis users might be able to use the Redis metric `EngineCPUUtilization`, which reports the percentage of usage on the Redis engine core\. To see if this metric is available on your nodes and for more information, see [Metrics for Redis](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/CacheMetrics.Redis.html)\.

## EngineCPUUtilization<a name="metrics-engine-cpu-utilization"></a>

For larger node types with 4vCPUs or more, you may want to use the `EngineCPUUtilization` metric, which reports the percentage of usage on the Redis engine core\. To see if this metric is available on your nodes and for more information, see [Metrics for Redis](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/CacheMetrics.Redis.html)\.

## SwapUsage<a name="metrics-swap-usage"></a>

This is a host\-level metric reported in bytes\. For more information, see [Host\-Level Metrics](CacheMetrics.HostLevel.md)\.

This metric should not exceed 50 MB\. If it does, see the following topics:
+ [Ensuring That You Have Enough Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)
+ [Managing Reserved Memory](redis-memory-management.md)

## Evictions<a name="metrics-evictions"></a>

This is a cache engine metric\. We recommend that you determine your own alarm threshold for this metric based on your application needs\.

## CurrConnections<a name="metrics-curr-connections"></a>

This is a cache engine metric\. We recommend that you determine your own alarm threshold for this metric based on your application needs\.

An increasing number of *CurrConnections* might indicate a problem with your application; you will need to investigate the application behavior to address this issue\.