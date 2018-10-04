# Host\-Level Metrics<a name="CacheMetrics.HostLevel"></a>

The `AWS/ElastiCache` namespace includes the following host\-level metrics for individual cache nodes\.

**See Also**
+ [Metrics for Redis](CacheMetrics.Redis.md)


| Metric | Description | Unit | 
| --- | --- | --- | 
| CPUUtilization |  The percentage of CPU utilization for the entire host\. Because Redis is single\-threaded, we recommend you monitor EngineCPUUtilization metric if available\. |  Percent  | 
| FreeableMemory  |  The amount of free memory available on the host\. This is derived from the RAM, buffers and cache that the OS reports as freeable\. |  Bytes  | 
| NetworkBytesIn |  The number of bytes the host has read from the network\.  |  Bytes  | 
| NetworkBytesOut |  The number of bytes the host has written to the network\.  |  Bytes  | 
| SwapUsage |  The amount of swap used on the host\.  |  Bytes  | 