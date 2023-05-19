# Supported node types<a name="CacheNodes.SupportedTypes"></a>

ElastiCache supports the following node types\. Generally speaking, the current generation types provide more memory and computational power at lower cost when compared to their equivalent previous generation counterparts\.

For more information on performance details for each node type, see [Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)\.

For information on which node size to use, see [Choosing your node size](nodes-select-size.md#CacheNodes.SelectSize)\. 

## Current Generation<a name="CacheNodes.CurrentGen"></a>

For more information on Previous Generation, please refer to [Previous Generation Nodes](https://aws.amazon.com/elasticache/previous-generation/)\.

**Note**  
Instance types with burstable network performance use a network I/O credit mechanism to burst beyond their baseline bandwidth on a best\-effort basis\.

**General**


| Instance type | Minimum supported Redis version | Enhanced I/O \(Redis 5\.0\.6\+\) | TLS Offloading \(Redis 6\.2\.5\+\) | Enhanced I/O Multiplexing \(Redis 7\.0\.4\+\) | Baseline bandwidth \(Gbps\) | Burst bandwidth \(Gbps\) | 
| --- | --- | --- | --- | --- | --- | --- | 
| cache\.m6g\.large | 5\.0\.6 | N | N | N | 0\.75 | 10\.0 | 
| cache\.m6g\.xlarge | 5\.0\.6 | Y | Y | Y | 1\.25 | 10\.0 | 
| cache\.m6g\.2xlarge | 5\.0\.6 | Y | Y | Y | 2\.5 | 10\.0 | 
| cache\.m6g\.4xlarge | 5\.0\.6 | Y | Y | Y | 5\.0 | 10\.0 | 
| cache\.m6g\.8xlarge | 5\.0\.6 | Y | Y | Y | 12 | N/A | 
| cache\.m6g\.12xlarge | 5\.0\.6 | Y | Y | Y | 20 | N/A | 
| cache\.m6g\.16xlarge | 5\.0\.6 | Y | Y | Y | 25 | N/A | 
| cache\.m5\.large | 3\.2\.4 | N | N | N | 0\.75 | 10\.0 | 
| cache\.m5\.xlarge | 3\.2\.4 | Y | N | N | 1\.25 | 10\.0 | 
| cache\.m5\.2xlarge | 3\.2\.4 | Y | Y | Y | 2\.5 | 10\.0 | 
| cache\.m5\.4xlarge | 3\.2\.4 | Y | Y | Y | 5\.0 | 10\.0 | 
| cache\.m5\.12xlarge | 3\.2\.4 | Y | Y | Y | 12 | N/A | 
| cache\.m5\.24xlarge | 3\.2\.4 | Y | Y | Y | 25 | N/A | 
| cache\.m4\.large | 3\.2\.4 | N | N | N | 0\.45 | 1\.2 | 
| cache\.m4\.xlarge | 3\.2\.4 | Y | N | N | 0\.75 | 2\.8 | 
| cache\.m4\.2xlarge | 3\.2\.4 | Y | Y | Y | 1\.0 | 10\.0 | 
| cache\.m4\.4xlarge | 3\.2\.4 | Y | Y | Y | 2\.0 | 10\.0 | 
| cache\.m4\.10xlarge | 3\.2\.4 | Y | Y | Y | 5\.0 | 10\.0 | 
| cache\.t4g\.micro | 3\.2\.4 | N | N | N | 0\.064 | 5\.0 | 
| cache\.t4g\.small | 5\.0\.6 | N | N | N | 0\.128 | 5\.0 | 
| cache\.t4g\.medium | 5\.0\.6 | N | N | N | 0\.256 | 5\.0 | 
| cache\.t3\.micro | 3\.2\.4 | N | N | N | 0\.064 | 5\.0 | 
| cache\.t3\.small | 3\.2\.4 | N | N | N | 0\.128 | 5\.0 | 
| cache\.t3\.medium | 3\.2\.4 | N | N | N | 0\.256 | 5\.0 | 
| cache\.t2\.micro | 3\.2\.4 | N | N | N | 0\.064 | 1\.024 | 
| cache\.t2\.small | 3\.2\.4 | N | N | N | 0\.128 | 1\.024 | 
| cache\.t2\.medium | 3\.2\.4 | N | N | N | 0\.256 | 1\.024 | 

**Memory optimized**


| Instance type | Minimum supported Redis version | Enhanced I/O \(Redis 5\.0\.6\+\) | TLS Offloading \(Redis 6\.2\.5\+\) | Enhanced I/O Multiplexing \(Redis 7\.0\.4\+\) | Baseline bandwidth \(Gbps\) | Burst bandwidth \(Gbps\) | 
| --- | --- | --- | --- | --- | --- | --- | 
| cache\.r6g\.large | 5\.0\.6 | N | N | N | 0\.75 | 10\.0 | 
| cache\.r6g\.xlarge | 5\.0\.6 | Y | Y | Y | 1\.25 | 10\.0 | 
| cache\.r6g\.2xlarge | 5\.0\.6 | Y | Y | Y | 2\.5 | 10\.0 | 
| cache\.r6g\.4xlarge | 5\.0\.6 | Y | Y | Y | 5\.0 | 10\.0 | 
| cache\.r6g\.8xlarge | 5\.0\.6 | Y | Y | Y | 12 | N/A | 
| cache\.r6g\.12xlarge | 5\.0\.6 | Y | Y | Y | 20 | N/A | 
| cache\.r6g\.16xlarge | 5\.0\.6 | Y | Y | Y | 25 | N/A | 
| cache\.r5\.large | 3\.2\.4 | N | N | N | 0\.75 | 10\.0 | 
| cache\.r5\.xlarge | 3\.2\.4 | Y | N | N | 1\.25 | 10\.0 | 
| cache\.r5\.2xlarge | 3\.2\.4 | Y | Y | Y | 2\.5 | 10\.0 | 
| cache\.r5\.4xlarge | 3\.2\.4 | Y | Y | Y | 5\.0 | 10\.0 | 
| cache\.r5\.12xlarge | 3\.2\.4 | Y | Y | Y | 12 | N/A | 
| cache\.r5\.24xlarge | 3\.2\.4 | Y | Y | Y | 25 | N/A | 
| cache\.r4\.large | 3\.2\.4 | N | N | N | 0\.75 | 10\.0 | 
| cache\.r4\.xlarge | 3\.2\.4 | Y | N | N | 1\.25 | 10\.0 | 
| cache\.r4\.2xlarge | 3\.2\.4 | Y | Y | Y | 2\.5 | 10\.0 | 
| cache\.r4\.4xlarge | 3\.2\.4 | Y | Y | Y | 5\.0 | 10\.0 | 
| cache\.r4\.8xlarge | 3\.2\.4 | Y | Y | Y | 12 | N/A | 
| cache\.r4\.16xlarge | 3\.2\.4 | Y | Y | Y | 25 | N/A | 

**Memory optimized with data tiering**


| Instance type | Minimum supported Redis version | Enhanced I/O \(Redis 5\.0\.6\+\) | TLS Offloading \(Redis 6\.2\.5\+\) | Enhanced I/O Multiplexing \(Redis 7\.0\.4\+\) | Baseline bandwidth \(Gbps\) | Burst bandwidth \(Gbps\) | 
| --- | --- | --- | --- | --- | --- | --- | 
| cache\.r6gd\.xlarge | 6\.2\.0 | Y | N | N | 1\.25 | 10 | 
| cache\.r6gd\.2xlarge | 6\.2\.0 | Y | Y | Y | 2\.5 | 10 | 
| cache\.r6gd\.4xlarge | 6\.2\.0 | Y | Y | Y | 5\.0 | 10 | 
| cache\.r6gd\.8xlarge | 6\.2\.0 | Y | Y | Y | N/A | N/A | 
| cache\.r6gd\.12xlarge | 6\.2\.0 | Y | Y | Y | N/A | N/A | 
| cache\.r6gd\.16xlarge | 6\.2\.0 | Y | Y | Y | N/A | N/A | 

## Supported node types by AWS Region<a name="CacheNodes.SupportedTypesByRegion"></a>

Supported node types may vary between AWS Regions\. For more details, see [Amazon ElastiCache pricing](https://aws.amazon.com/elasticache/pricing/)\.

## Burstable Performance Instances<a name="CacheNodes.Burstable"></a>

You can launch general\-purpose burstable T4g, T3\-Standard and T2\-Standard cache nodes in Amazon ElastiCache\. These nodes provide a baseline level of CPU performance with the ability to burst CPU usage at any time until the accrued credits are exhausted\. A *CPU credit* provides the performance of a full CPU core for one minute\.

Amazon ElastiCache's T4g, T3 and T2 nodes are configured as standard and suited for workloads with an average CPU utilization that is consistently below the baseline performance of the instance\. To burst above the baseline, the node spends credits that it has accrued in its CPU credit balance\. If the node is running low on accrued credits, performance is gradually lowered to the baseline performance level\. This gradual lowering ensures the node doesn't experience a sharp performance drop\-off when its accrued CPU credit balance is depleted\. For more information, see [CPU Credits and Baseline Performance for Burstable Performance Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/burstable-credits-baseline-concepts.html) in the *Amazon EC2 User Guide*\.**

The following table lists the burstable performance node types, the rate at which CPU credits are earned per hour\. It also shows the maximum number of earned CPU credits that a node can accrue and the number of vCPUs per node\. In addition, it gives the baseline performance level as a percentage of a full core performance \(using a single vCPU\)\.


| Node type | CPU credits earned per hour |  Maximum earned credits that can be accrued\* |  vCPUs  |  Baseline performance per vCPU  |  Memory \(GiB\)  |  Network performance  | 
| --- | --- | --- | --- | --- | --- | --- | 
| t4g\.micro | 12 | 288 | 2 | 10% | 0\.5 | Up to 5 Gigabit | 
| t4g\.small | 24 | 576 | 2 | 20% | 1\.37 | Up to 5 Gigabit | 
| t4g\.medium | 24 | 576 | 2 | 20% | 3\.09 | Up to 5 Gigabit | 
| t3\.micro | 12 | 288 | 2 | 10% | 0\.5 | Up to 5 Gigabit | 
| t3\.small | 24 | 576 | 2 | 20% | 1\.37 | Up to 5 Gigabit | 
| t3\.medium | 24 | 576 | 2 | 20% | 3\.09 | Up to 5 Gigabit | 
| t2\.micro | 6 | 144 | 1 | 10% | 0\.5 | Low to moderate | 
| t2\.small | 12 | 288 | 1 | 20% | 1\.55 | Low to moderate | 
| t2\.medium | 24 | 576 | 2 | 20% | 3\.22 | Low to moderate | 

\* The number of credits that can be accrued is equivalent to the number of credits that can be earned in a 24\-hour period\.

\*\* The baseline performance in the table is per vCPU\. Some node sizes that have more than one vCPU\. For these, calculate the baseline CPU utilization for the node by multiplying the vCPU percentage by the number of vCPUs\.

The following CPU credit metrics are available for T3 and T4g burstable performance instances:

**Note**  
These metrics are not available for T2 burstable performance instances\.
+ `CPUCreditUsage`
+ `CPUCreditBalance`

For more information on these metrics, see [CPU Credit Metrics](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html#cpu-credit-metrics)\.

In addition, be aware of these details:
+ All current generation node types are created in a virtual private cloud \(VPC\) based on Amazon VPC by default\.
+ Redis append\-only files \(AOF\) aren't supported for T2 instances\. Redis configuration variables `appendonly` and `appendfsync` aren't supported on Redis version 2\.8\.22 and later\.

## Related Information<a name="CacheNodes.RelatedInfo"></a>
+ [Amazon ElastiCache Product Features and Details](https://aws.amazon.com/elasticache/details)
+ [Redis\-specific parameters](ParameterGroups.Redis.md)
+ [In Transit Encryption \(TLS\)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/in-transit-encryption.html)