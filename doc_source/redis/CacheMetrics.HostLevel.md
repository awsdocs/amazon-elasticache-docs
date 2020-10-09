# Host\-Level Metrics<a name="CacheMetrics.HostLevel"></a>

The `AWS/ElastiCache` namespace includes the following host\-level metrics for individual cache nodes\.

**See Also**
+ [Metrics for Redis](CacheMetrics.Redis.md)


| Metric | Description | Unit | 
| --- | --- | --- | 
| CPUUtilization |  The percentage of CPU utilization for the entire host\. Because Redis is single\-threaded, and we recommend you monitor EngineCPUUtilization metric for nodes with 4 or more vCPUs\. |  Percent  | 
| FreeableMemory  |  The amount of free memory available on the host\. This is derived from the RAM, buffers, and cache that the OS reports as freeable\. |  Bytes  | 
| NetworkBytesIn |  The number of bytes the host has read from the network\.  |  Bytes  | 
| NetworkBytesOut | The number of bytes sent out on all network interfaces by the instance\.  |  Bytes  | 
| NetworkPacketsIn | The number of packets received on all network interfaces by the instance\. This metric identifies the volume of incoming traffic in terms of the number of packets on a single instance\.  | Count  | 
| NetworkPacketsOut |  The number of packets sent out on all network interfaces by the instance\. This metric identifies the volume of outgoing traffic in terms of the number of packets on a single instance\. | Count  | 
| SwapUsage |  The amount of swap used on the host\.  |  Bytes  | 