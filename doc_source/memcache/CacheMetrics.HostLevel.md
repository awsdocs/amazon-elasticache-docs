# Host\-Level Metrics<a name="CacheMetrics.HostLevel"></a>

The `AWS/ElastiCache` namespace includes the following host\-level metrics for individual cache nodes\.

**See Also**
+ [Metrics for Memcached](CacheMetrics.Memcached.md)


| Metric | Description | Unit | 
| --- | --- | --- | 
| CPUUtilization |  The percentage of CPU utilization for the entire host\.  |  Percent  | 
| CPUCreditBalance | The number of earned CPU credits that an instance has accrued since it was launched or started\. For T2 Standard, the CPUCreditBalance also includes the number of launch credits that have been accrued\. Credits are accrued in the credit balance after they are earned, and removed from the credit balance when they are spent\. The credit balance has a maximum limit, determined by the instance size\. After the limit is reached, any new credits that are earned are discarded\. For T2 Standard, launch credits do not count towards the limit\. The credits in the CPUCreditBalance are available for the instance to spend to burst beyond its baseline CPU utilization\. CPU credit metrics are available at a five\-minute frequency only\. | Credits \(vCPU\-minutes\)  | 
| CPUCreditUsage | The number of CPU credits spent by the instance for CPU utilization\. One CPU credit equals one vCPU running at 100% utilization for one minute or an equivalent combination of vCPUs, utilization, and time \(for example, one vCPU running at 50% utilization for two minutes or two vCPUs running at 25% utilization for two minutes\)\. CPU credit metrics are available at a five\-minute frequency only\. If you specify a period greater than five minutes, use the Sum statistic instead of the Average statistic\.  | Credits \(vCPU\-minutes\)  | 
| FreeableMemory  |  The amount of free memory available on the host\. This is derived from the RAM, buffers, and cache that the OS reports as freeable\. |  Bytes  | 
| NetworkBytesIn |  The number of bytes the host has read from the network\.  |  Bytes  | 
| NetworkBytesOut | The number of bytes sent out on all network interfaces by the instance\.  |  Bytes  | 
| NetworkPacketsIn | The number of packets received on all network interfaces by the instance\. This metric identifies the volume of incoming traffic in terms of the number of packets on a single instance\.  | Count  | 
| NetworkPacketsOut |  The number of packets sent out on all network interfaces by the instance\. This metric identifies the volume of outgoing traffic in terms of the number of packets on a single instance\. | Count  | 
| NetworkBandwidthInAllowanceExceeded | The number of packets shaped because the inbound aggregate bandwidth exceeded the maximum for the instance\. | Count  | 
| NetworkConntrackAllowanceExceeded | The number of packets shaped because connection tracking exceeded the maximum for the instance and new connections could not be established\. This can result in packet loss for traffic to or from the instance\. | Count  | 
| NetworkLinkLocalAllowanceExceeded | The number of packets shaped because the PPS of the traffic to local proxy services exceeded the maximum for the network interface\. This impacts traffic to the DNS service, the Instance Metadata Service, and the Amazon Time Sync Service\. | Count  | 
| NetworkBandwidthOutAllowanceExceeded | The number of packets shaped because the outbound aggregate bandwidth exceeded the maximum for the instance\. | Count  | 
| Network Packets Per Second Allowance Exceeded | The number of packets shaped because the bidirectional packets per second exceeded the maximum for the instance\. | Count  | 
| SwapUsage |  The amount of swap used on the host\.  |  Bytes  | 