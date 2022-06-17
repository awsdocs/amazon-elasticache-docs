# Configuring engine parameters using parameter groups<a name="ParameterGroups"></a>

Amazon ElastiCache uses parameters to control the runtime properties of your nodes and clusters\. Generally, newer engine versions include additional parameters to support the newer functionality\. For tables of parameters, see [Memcached specific parameters](ParameterGroups.Memcached.md)\.

As you would expect, some parameter values, such as `maxmemory`, are determined by the engine and node type\. For a table of these parameter values by node type, see [Memcached node\-type specific parameters](ParameterGroups.Memcached.md#ParameterGroups.Memcached.NodeSpecific)\.

**Note**  
For a list of Memcached\-specific parameters, see [Memcached Specific Parameters](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/ParameterGroups.Memcached.html)\.

**Topics**
+ [Parameter management](ParameterGroups.Management.md)
+ [Cache parameter group tiers](ParameterGroups.Tiers.md)
+ [Creating a parameter group](ParameterGroups.Creating.md)
+ [Listing parameter groups by name](ParameterGroups.ListingGroups.md)
+ [Listing a parameter group's values](ParameterGroups.ListingValues.md)
+ [Modifying a parameter group](ParameterGroups.Modifying.md)
+ [Deleting a parameter group](ParameterGroups.Deleting.md)
+ [Memcached specific parameters](ParameterGroups.Memcached.md)