# Host\-level metrics<a name="CacheMetrics.HostLevel"></a>

The `AWS/ElastiCache` namespace includes the following host\-level metrics for individual cache nodes\.

**See also**
+ [Metrics for Memcached](CacheMetrics.Memcached.md)


| Metric | Description | Unit | 
| --- | --- | --- | 
| CPUUtilization |  The percentage of CPU utilization\.  |  Percent  | 
| FreeableMemory  |  The amount of free memory available on the host\.  |  Bytes  | 
| NetworkBytesIn |  The number of bytes the host has read from the network\.  |  Bytes  | 
| NetworkBytesOut |  The number of bytes the host has written to the network\.  |  Bytes  | 
| NetworkPacketsIn | The number of packets received on all network interfaces by the instance\. This metric identifies the volume of incoming traffic in terms of the number of packets on a single instance\.  | Count  | 
| NetworkPacketsOut |  The number of packets sent out on all network interfaces by the instance\. This metric identifies the volume of outgoing traffic in terms of the number of packets on a single instance\.  | Count  | 
| SwapUsage |  The amount of swap used on the host\.  | Bytes  | 