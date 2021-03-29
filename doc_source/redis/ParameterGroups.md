# Configuring engine parameters using parameter groups<a name="ParameterGroups"></a>

Amazon ElastiCache uses parameters to control the runtime properties of your nodes and clusters\. Generally, newer engine versions include additional parameters to support the newer functionality\. For tables of parameters, see [Redis\-specific parameters](ParameterGroups.Redis.md)\.

As you would expect, some parameter values, such as `max_cache_memory`, are determined by the engine and node type\. For a table of these parameter values by node type, see [Redis node\-type specific parameters](ParameterGroups.Redis.md#ParameterGroups.Redis.NodeSpecific)\.

**Note**  
For a list of Memcached\-specific parameters, see [Memcached Specific Parameters](https://docs.aws.amazon.com/en_us/AmazonElastiCache/latest/mem-ug/ParameterGroups.Memcached.html)\.

**Topics**
+ [Parameter management](ParameterGroups.Management.md)
+ [Cache parameter group tiers](ParameterGroups.Tiers.md)
+ [Creating a parameter group](ParameterGroups.Creating.md)
+ [Listing parameter groups by name](ParameterGroups.ListingGroups.md)
+ [Listing a parameter group's values](ParameterGroups.ListingValues.md)
+ [Modifying a parameter group](ParameterGroups.Modifying.md)
+ [Deleting a parameter group](ParameterGroups.Deleting.md)
+ [Redis\-specific parameters](ParameterGroups.Redis.md)