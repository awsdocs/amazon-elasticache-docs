# Cache Parameter Group Tiers<a name="ParameterGroups.Tiers"></a>

Amazon ElastiCache has three tiers of cache parameter groups as illustrated here\.

![\[Image: Amazon ElastiCache parameter group tiers\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-ParameterGroups-Tiers.png)

*Amazon ElastiCache parameter group tiers*

**Global Default**

The top\-level root parameter group for all Amazon ElastiCache customers in the region\.

The global default cache parameter group:

+ Is reserved for ElastiCache and not available to the customer\.

**Customer Default**

A copy of the Global Default cache parameter group which is created for the customer's use\.

The Customer Default cache parameter group:

+ Is created and owned by ElastiCache\.

+ Is available to the customer for use as a cache parameter group for any clusters running an engine version supported by this cache parameter group\. For example, `default.redis2.8` supports Redis engine versions 2\.8\.*x*\.

+ Cannot be edited by the customer\.

**Customer Owned**

A copy of the Customer Default cache parameter group\. A Customer Owned cache parameter group is created whenever the customer creates a cache parameter group\.

The Customer Owned cache parameter group:

+ Is created and owned by the customer\.

+ Can be assigned to any of the customer's compatible clusters\. For example, a cache parameter group created with the family `redis2.8` is compatible with any cluster running Redis 2\.8\.*x*\.

+ Can be modified by the customer to create a custom cache parameter group\. 

   Not all parameter values can be modified\. For more information, see either [Memcached Specific Parameters](ParameterGroups.Memcached.md) or [Redis Specific Parameters](ParameterGroups.Redis.md)\.