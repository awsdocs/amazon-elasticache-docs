# Supported node types<a name="CacheNodes.SupportedTypes"></a>

For information on which node size to use, see [Choosing your Memcached node size](nodes-select-size.md#CacheNodes.SelectSize)\. 

ElastiCache supports the following node types\. Generally speaking, the current generation types provide more memory and computational power at lower cost when compared to their equivalent previous generation counterparts\.

For more information on performance details for each node type, see [Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)\.
+ General purpose:
  + Current generation: 

    **M6g node types** \(available only for Memcached engine version 1\.5\.16 onward\)\.

     `cache.m6g.large`, `cache.m6g.xlarge`, `cache.m6g.2xlarge`, `cache.m6g.4xlarge`, `cache.m6g.8xlarge`, `cache.m6g.12xlarge`, `cache.m6g.16xlarge` 
**Note**  
For region availability, see [Supported node types by AWS Region](#CacheNodes.SupportedTypesByRegion)\.

    **M5 node types:** `cache.m5.large`, `cache.m5.xlarge`, `cache.m5.2xlarge`, `cache.m5.4xlarge`, `cache.m5.12xlarge`, `cache.m5.24xlarge` 

    **M4 node types:** `cache.m4.large`, `cache.m4.xlarge`, `cache.m4.2xlarge`, `cache.m4.4xlarge`, `cache.m4.10xlarge`

    **T4g node types** \(available only for Memcached engine version 1\.5\.16 onward\)\.

     `cache.t4g.micro`, `cache.t4g.small`, `cache.t4g.medium` 

    **T3 node types:** `cache.t3.micro`, `cache.t3.small`, `cache.t3.medium`

    **T2 node types:** `cache.t2.micro`, `cache.t2.small`, `cache.t2.medium`
  + Previous generation: \(not recommended\. Existing clusters are still supported but creation of new clusters is not supported for these types\.\)

    **T1 node types:** `cache.t1.micro`

    **M1 node types:** `cache.m1.small`, `cache.m1.medium`, `cache.m1.large`, `cache.m1.xlarge`

    **M3 node types:** `cache.m3.medium`, `cache.m3.large`, `cache.m3.xlarge`, `cache.m3.2xlarge`
+ Compute optimized:
  + Previous generation: \(not recommended\)

    **C1 node types:** `cache.c1.xlarge`
+ Memory optimized:
  + Current generation: 

    \(**R6g node types** are available only for Memcached engine version 1\.5\.16 onward\.\)

    **R6g node types:** `cache.r6g.large`, `cache.r6g.xlarge`, `cache.r6g.2xlarge`, `cache.r6g.4xlarge`, `cache.r6g.8xlarge`, `cache.r6g.12xlarge`, `cache.r6g.16xlarge` 
**Note**  
For region availability, see [Supported node types by AWS Region](#CacheNodes.SupportedTypesByRegion)\.

    **R5 node types:** `cache.r5.large`, `cache.r5.xlarge`, `cache.r5.2xlarge`, `cache.r5.4xlarge`, `cache.r5.12xlarge`, `cache.r5.24xlarge`

    **R4 node types:** `cache.r4.large`, `cache.r4.xlarge`, `cache.r4.2xlarge`, `cache.r4.4xlarge`, `cache.r4.8xlarge`, `cache.r4.16xlarge`
  + Previous generation: \(not recommended\)

    **M2 node types:** `cache.m2.xlarge`, `cache.m2.2xlarge`, `cache.m2.4xlarge`

    **R3 node types:** `cache.r3.large`, `cache.r3.xlarge`, `cache.r3.2xlarge`, `cache.r3.4xlarge`, `cache.r3.8xlarge`

## Supported node types by AWS Region<a name="CacheNodes.SupportedTypesByRegion"></a>

Supported node types may vary between AWS Regions\. For more details, see [Amazon ElastiCache pricing](https://aws.amazon.com/elasticache/pricing/)\.

## Related Information<a name="CacheNodes.RelatedInfo"></a>
+ [Amazon ElastiCache Product Features and Details](https://aws.amazon.com/elasticache/details)
+ [Memcached Node\-Type Specific Parameters](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/ParameterGroups.Memcached.html)