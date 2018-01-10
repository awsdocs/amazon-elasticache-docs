# Which Metrics Should I Monitor?<a name="CacheMetrics.WhichShouldIMonitor"></a>

The following CloudWatch metrics offer good insight into ElastiCache performance\. In most cases, we recommend that you set CloudWatch alarms for these metrics so that you can take corrective action before performance issues occur\.


+ [CPUUtilization](#metrics-cpu-utilization)
+ [SwapUsage](#metrics-swap-usage)
+ [Evictions](#metrics-evictions)
+ [CurrConnections](#metrics-curr-connections)

## CPUUtilization<a name="metrics-cpu-utilization"></a>

This is a host\-level metric reported as a percent\. For more information, see [Host\-Level Metrics](CacheMetrics.HostLevel.md)\.

+ *Memcached: *Since Memcached is multi\-threaded, this metric can be as high as 90%\. If you exceed this threshold, scale your cache cluster up by using a larger cache node type, or scale out by adding more cache nodes\.

+ *Redis: *Since Redis is single\-threaded, the threshold is calculated as \(90 / number of processor cores\)\. For example, suppose you are using a *cache\.m1\.xlarge* node, which has four cores\. In this case, the threshold for *CPUUtilization* would be \(90 / 4\), or 22\.5%\.

  You will need to determine your own threshold, based on the number of cores in the cache node that you are using\. If you exceed this threshold, and your main workload is from read requests, scale your cache cluster out by adding read replicas\. If the main workload is from write requests, depending on your cluster configuration, we recommend that you:

  + **Redis \(cluster mode disabled\) clusters:** scale up by using a larger cache instance type\.

  + **Redis \(cluster mode enabled\) clusters:** add more shards to distribute the write workload across more primary nodes\.

## SwapUsage<a name="metrics-swap-usage"></a>

This is a host\-level metric reported in bytes\. For more information, see [Host\-Level Metrics](CacheMetrics.HostLevel.md)\.

+ *Memcached: *This metric should not exceed 50 MB\. If it does, we recommend that you increase the *ConnectionOverhead parameter value*\.

+ *Redis: *At this time, we have no recommendation for this parameter; you do not need to set a CloudWatch alarm for it\.

## Evictions<a name="metrics-evictions"></a>

This is a cache engine metric, published for both Memcached and Redis cache clusters\. We recommend that you determine your own alarm threshold for this metric based on your application needs\.

+ *Memcached: *If you exceed your chosen threshold, scale your cluster up by using a larger node type, or scale out by adding more nodes\.

+ *Redis: *If you exceed your chosen threshold, scale your cluster up by using a larger node type\.

## CurrConnections<a name="metrics-curr-connections"></a>

This is a cache engine metric, published for both Memcached and Redis cache clusters\. We recommend that you determine your own alarm threshold for this metric based on your application needs\.

Whether you are running Memcached or Redis, an increasing number of *CurrConnections* might indicate a problem with your application; you will need to investigate the application behavior to address this issue\.