# Supported Node Types<a name="CacheNodes.SupportedTypes"></a>

ElastiCache supports the following node types\. Generally speaking, the current generation types provide more memory and computational power at lower cost when compared to their equivalent previous generation counterparts\.
+ General purpose:
  + Current generation: 

    **M5 node types:** `cache.m5.large`, `cache.m5.xlarge`, `cache.m5.2xlarge`, `cache.m5.4xlarge`, `cache.m5.12xlarge`, `cache.m5.24xlarge` 

    **M4 node types:** `cache.m4.large`, `cache.m4.xlarge`, `cache.m4.2xlarge`, `cache.m4.4xlarge`, `cache.m4.10xlarge`

    **T3 node types:** `cache.t3.micro`, `cache.t3.small`, `cache.t3.medium`

    **T2 node types:** `cache.t2.micro`, `cache.t2.small`, `cache.t2.medium`
  + Previous generation: \(not recommended\)

    **T1 node types:** `cache.t1.micro`

    **M1 node types:** `cache.m1.small`, `cache.m1.medium`, `cache.m1.large`, `cache.m1.xlarge`

    **M3 node types:** `cache.m3.medium`, `cache.m3.large`, `cache.m3.xlarge`, `cache.m3.2xlarge`
+ Compute optimized:
  + Previous generation: \(not recommended\)

    **C1 node types:** `cache.c1.xlarge`
+ Memory optimized:
  + Current generation: 

    **R5 node types:** `cache.r5.large`, `cache.r5.xlarge`, `cache.r5.2xlarge`, `cache.r5.4xlarge`, `cache.r5.12xlarge`, `cache.r5.24xlarge`

    **R4 node types:** `cache.r4.large`, `cache.r4.xlarge`, `cache.r4.2xlarge`, `cache.r4.4xlarge`, `cache.r4.8xlarge`, `cache.r4.16xlarge`
  + Previous generation: \(not recommended\)

    **M2 node types:** `cache.m2.xlarge`, `cache.m2.2xlarge`, `cache.m2.4xlarge`

    **R3 node types:** `cache.r3.large`, `cache.r3.xlarge`, `cache.r3.2xlarge`, `cache.r3.4xlarge`, `cache.r3.8xlarge`

You can launch general\-purpose burstable T3\-Standard and T2\-Standard cache nodes in Amazon ElastiCache\. These nodes provide a baseline level of CPU performance with the ability to burst CPU usage at any time until the accrued credits are exhausted\. A *CPU credit* provides the performance of a full CPU core for one minute\.

Amazon ElastiCache's T3 and T2 nodes are configured as standard and suited for workloads with an average CPU utilization that is consistently below the baseline performance of the instance\. To burst above the baseline, the node spends credits that it has accrued in its CPU credit balance\. If the node is running low on accrued credits, performance is gradually lowered to the baseline performance level\. This gradual lowering ensures the node doesn't experience a sharp performance drop\-off when its accrued CPU credit balance is depleted\. For more information, see [CPU Credits and Baseline Performance for Burstable Performance Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/burstable-credits-baseline-concepts.html) in the *Amazon EC2 User Guide*\.**

The following table lists the burstable performance node types, the rate at which CPU credits are earned per hour\. It also shows the maximum number of earned CPU credits that a node can accrue and the number of vCPUs per node\. In addition, it gives the baseline performance level as a percentage of a full core performance \(using a single vCPU\)\.


| Node Type | CPU Credits Earned Per Hour |  Maximum Earned Credits That Can Be Accrued\* |  vCPUs  |  Baseline Performance Per vCPU  |  Memory \(GiB\)  |  Network Performance  | 
| --- | --- | --- | --- | --- | --- | --- | 
| t3\.micro | 12 | 288 | 2 | 10% | 0\.5 | Up to 5 Gigabit | 
| t3\.small | 24 | 576 | 2 | 20% | 1\.37 | Up to 5 Gigabit | 
| t3\.medium | 24 | 576 | 2 | 20% | 3\.09 | Up to 5 Gigabit | 
| t2\.micro | 6 | 144 | 1 | 10% | 0\.5 | Low to moderate | 
| t2\.small | 12 | 288 | 1 | 20% | 1\.55 | Low to moderate | 
| t2\.medium | 24 | 576 | 2 | 20% | 3\.22 | Low to moderate | 

\* The number of credits that can be accrued is equivalent to the number of credits that can be earned in a 24\-hour period\.

\*\* The baseline performance in the table is per vCPU\. Some node sizes that have more than one vCPU\. For these, calculate the baseline CPU utilization for the node by multiplying the vCPU percentage by the number of vCPUs\.

The following CPU credit metrics are available for T3 burstable performance instances:

**Note**  
These metrics are not available for T2 burstable performance instances\.
+ `CPUCreditUsage`
+ `CPUCreditBalance`

For more information on these metrics, see [CPU Credit Metrics](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html#cpu-credit-metrics)\.

In addition, be aware of these details:
+ All current generation node types are created in a virtual private cloud \(VPC\) based on Amazon VPC by default\.
+ Redis append\-only files \(AOF\) aren't supported for T1 or T2 instances\.
+ Redis Multi\-AZ isn't supported on T1 instances\.
+ Redis configuration variables `appendonly` and `appendfsync` aren't supported on Redis version 2\.8\.22 and later\.

**Note**  
Supported engine versions vary by AWS Region\. The latest engine versions are supported in all AWS Regions\. To find the available engine versions in your AWS Region, see [Supported ElastiCache for Redis Versions](supported-engine-versions.md)\.

## Supported Node Types by AWS Region<a name="CacheNodes.SupportedTypesByRegion"></a>

The following table lists supported node types for each AWS Region\.


| AWS Region Name | AWS Region |  T3  |  T2  |  M4  |  M5  |  R4  |  R5  | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| US East \(Ohio\) | us\-east\-2 | Yes | Yes | Yes | Yes | Yes | Yes | 
| US East \(N\. Virginia\) | us\-east\-1 | Yes | Yes | Yes | Yes | Yes | Yes | 
| US West \(N\. California\) | us\-west\-1 | Yes | Yes | Yes | Yes | Yes | Yes | 
| US West \(Oregon\) | us\-west\-2 | Yes | Yes | Yes  | Yes | Yes | Yes | 
| Canada \(Central\) | ca\-central\-1 | Yes | Yes | Yes | Yes | Yes | Yes | 
| Asia Pacific \(Mumbai\) | ap\-south\-1 | Yes | Yes | Yes | Yes | Yes | Yes | 
| Asia Pacific \(Tokyo\) | ap\-northeast\-1 | Yes | Yes | Yes | Yes | Yes | Yes | 
| Asia Pacific \(Seoul\) | ap\-northeast\-2 | Yes | Yes | Yes | Yes | Yes | Yes | 
| Asia Pacific \(Osaka\-Local\) \* | ap\-northeast\-3 | Yes | Yes | Yes | Yes | Yes |  | 
| Asia Pacific \(Singapore\) | ap\-southeast\-1 | Yes | Yes | Yes | Yes | Yes | Yes  | 
| Asia Pacific \(Sydney\) | ap\-southeast\-2 | Yes | Yes | Yes | Yes | Yes | Yes | 
| Asia Pacific \(Hong Kong\) | ap\-east\-1 | Yes |  |  | Yes |  | Yes | 
| Europe \(Stockholm\) | eu\-north\-1 | Yes |  |  | Yes | No | Yes | 
| Europe \(Frankfurt\) | eu\-central\-1 | Yes | Yes | Yes | Yes | Yes | Yes | 
| Europe \(Ireland\) | eu\-west\-1 | Yes | Yes | Yes | Yes | Yes | Yes | 
| Europe \(London\) | eu\-west\-2 | Yes | Yes | Yes | Yes | Yes | Yes | 
| EU \(Paris\) | eu\-west\-3 | Yes | Yes |   | Yes | Yes | Yes | 
| South America \(São Paulo\) | sa\-east\-1 | Yes | Yes | Yes | Yes | Yes | Yes | 
| China \(Beijing\) | cn\-north\-1 | Yes | Yes | Yes |   | Yes | Yes | 
| China \(Ningxia\) | cn\-northwest\-1 | Yes | Yes | Yes |   | Yes | Yes | 
| Middle East \(Bahrain\) | me\-south\-1 | Yes  |   |   | Yes |   | Yes | 
| AWS GovCloud \(US\-West\) | us\-gov\-west\-1 | Yes | Yes |  | Yes | Yes | Yes | 
|  \* The Asia Pacific \(Osaka\-Local\) Region is a local region that is available to select AWS customers who request access\. If you want to use the Asia Pacific \(Osaka\-Local\) Region, speak with your sales representative\. The Asia Pacific \(Osaka\-Local\) Region supports a single Availability Zone\. | 

For a complete list of node types and specifications, see the following:
+ [Amazon ElastiCache Product Features and Details](https://aws.amazon.com/elasticache/details)
+ [Redis Specific Parameters](ParameterGroups.Redis.md)