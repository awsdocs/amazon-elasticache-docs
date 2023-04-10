# Which metrics should I monitor?<a name="CacheMetrics.WhichShouldIMonitor"></a>

The following CloudWatch metrics offer good insight into ElastiCache performance\. In most cases, we recommend that you set CloudWatch alarms for these metrics so that you can take corrective action before performance issues occur\.

**Topics**
+ [CPUUtilization](#metrics-cpu-utilization)
+ [SwapUsage](#metrics-swap-usage)
+ [Evictions](#metrics-evictions)
+ [CurrConnections](#metrics-curr-connections)

## CPUUtilization<a name="metrics-cpu-utilization"></a>

This is a host\-level metric reported as a percent\. For more information, see [Host\-Level Metrics](CacheMetrics.HostLevel.md)\.

Because Memcached is multi\-threaded, this metric can be as high as 90%\. If you exceed this threshold, scale your cache cluster up by using a larger cache node type, or scale out by adding more cache nodes\.

## SwapUsage<a name="metrics-swap-usage"></a>

This is a host\-level metric reported in bytes\. For more information, see [Host\-Level Metrics](CacheMetrics.HostLevel.md)\.

The `FreeableMemory` CloudWatch metric being close to 0 \(i\.e\., below 100MB\) or `SwapUsage` metric greater than the `FreeableMemory` metric indicates a node is under memory pressure\. If this happens, we recommend that you increase the *ConnectionOverhead parameter value*\.

## Evictions<a name="metrics-evictions"></a>

This is a cache engine metric\. We recommend that you determine your own alarm threshold for this metric based on your application needs\.

If you exceed your chosen threshold, scale your cluster up by using a larger node type, or scale out by adding more nodes\.

## CurrConnections<a name="metrics-curr-connections"></a>

This is a cache engine metric\. We recommend that you determine your own alarm threshold for this metric based on your application needs\.

An increasing number of *CurrConnections* might indicate a problem with your application; you will need to investigate the application behavior to address this issue\.