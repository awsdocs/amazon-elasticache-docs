# Supported Node Types<a name="CacheNodes.SupportedTypes"></a>

The following node types are supported by ElastiCache\. Generally speaking, the current generation types provide more memory and computational power at lower cost when compared to their equivalent previous generation counterparts\.

+ General purpose:

  + Current generation: 

    **T2 node types:** `cache.t2.micro`, `cache.t2.small`, `cache.t2.medium`

    **M3 node types:** `cache.m3.medium`, `cache.m3.large`, `cache.m3.xlarge`, `cache.m3.2xlarge`

    **M4 node types:** `cache.m4.large`, `cache.m4.xlarge`, `cache.m4.2xlarge`, `cache.m4.4xlarge`, `cache.m4.10xlarge`

  + Previous generation: \(not recommended\)

    **T1 node types:** `cache.t1.micro`

    **M1 node types:** `cache.m1.small`, `cache.m1.medium`, `cache.m1.large`, `cache.m1.xlarge`

+ Compute optimized:

  + Previous generation: \(not recommended\)

    **C1 node types:** `cache.c1.xlarge`

+ Memory optimized:

  + Current generation: 

    **R3 node types:** `cache.r3.large`, `cache.r3.xlarge`, `cache.r3.2xlarge`, `cache.r3.4xlarge`, `cache.r3.8xlarge`

    **R4 node types:** `cache.r4.large`, `cache.r4.xlarge`, `cache.r4.2xlarge`, `cache.r4.4xlarge`, `cache.r4.8xlarge`, `cache.r4.16xlarge`

  + Previous generation: \(not recommended\)

    **M2 node types:** `cache.m2.xlarge`, `cache.m2.2xlarge`, `cache.m2.4xlarge`

**Additional node type info**

+ All T2 instances are created in an Amazon Virtual Private Cloud \(Amazon VPC\)\.

+ Redis backup and restore is not supported for T2 instances\.

+ Redis append\-only files \(AOF\) are not supported for T1 or T2 instances\.

+ Redis Multi\-AZ with automatic failover is not supported on T1 instances\.

+ Redis Multi\-AZ with automatic failover is supported on T2 instances only when running Redis \(cluster mode enabled\) \- version 3\.2\.4 or later with the `default.redis3.2.cluster.on` parameter group or one derived from it\.

+ Redis configuration variables `appendonly` and `appendfsync` are not supported on Redis version 2\.8\.22 and later\.

**Supported engine versions**

Supported engine versions vary by region\. The latest engine versions are supported in all regions\. To find the available engine versions in your region see [Determine Available Engine Versions](SelectEngine.RegionVersions.md)\.

## Supported Node Types by Region<a name="CacheNodes.SupportedTypesByRegion"></a>


|  Region Name  |  Region  |   T2   |   M3   |   M4   |   R3   |   R4   | 
| --- | --- | --- | --- | --- | --- | --- | 
| US East \(Ohio\) | us\-east\-2 | Yes |  | Yes | Yes | Yes | 
| US East \(N\. Virginia\) | us\-east\-1 | Yes | Yes | Yes | Yes | Yes | 
| US West \(N\. California\) | us\-west\-1 | Yes | Yes | Yes | Yes | Yes | 
| US West \(Oregon\) | us\-west\-2 | Yes | Yes |  | Yes | Yes | 
| Canada \(Central\) | ca\-central\-1 | Yes |  | Yes |  | Yes | 
| Asia Pacific \(Mumbai\) | ap\-south\-1 | Yes |  | Yes | Yes | Yes | 
| Asia Pacific \(Seoul\) | ap\-northeast\-2 | Yes | Yes | Yes | Yes | Yes | 
| Asia Pacific \(Singapore\) | ap\-southeast\-1 | Yes | Yes | Yes | Yes | Yes | 
| Asia Pacific \(Sydney\) | ap\-southeast\-2 | Yes | Yes | Yes | Yes | Yes | 
| Asia Pacific \(Tokyo\) | ap\-northeast\-1 | Yes | Yes | Yes | Yes | Yes | 
| EU \(Frankfurt\) | eu\-central\-1 | Yes | Yes | Yes | Yes | Yes | 
| EU \(Ireland\) | eu\-west\-1 | Yes | Yes | Yes | Yes | Yes | 
| EU \(London\) | eu\-west\-2 | Yes |  | Yes |  | Yes | 
| EU \(Paris\) | eu\-west\-3 | Yes |  |  |  | Yes | 
| South America \(São Paulo\) | sa\-east\-1 | Yes | Yes | Yes | Yes | Yes | 
| China \(Beijing\) | cn\-north\-1 | Yes | Yes | Yes | Yes | Yes | 
| China \(Ningxia\) | cn\-northwest\-1 | Yes |  | Yes |  | Yes | 
| AWS GovCloud \(US\) | gov\-uswest\-1 | Yes | Yes |  | Yes | Yes | 

For a complete list of node types and specifications, see the following:

+ [Amazon ElastiCache Product Features and Details](https://aws.amazon.com/elasticache/details)

+ [Memcached Node\-Type Specific Parameters](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/ParameterGroups.Memcached.html#ParameterGroups.Memcached.NodeSpecific)

+ [Redis Node\-Type Specific Parameters](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/ParameterGroups.Redis.html#ParameterGroups.Redis.NodeSpecific)