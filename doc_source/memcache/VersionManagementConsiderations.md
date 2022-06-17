# Major version behavior and compatibility differences<a name="VersionManagementConsiderations"></a>

Redis versions are identified with a semantic version which comprise a MAJOR, MINOR, and PATCH component\. For example, in Redis 4\.0\.10, the major version is 4, the minor version 0, and the patch version is 10\. These values are generally incremented based off the following conventions:
+ MAJOR versions are for API incompatible changes
+ MINOR versions are for new functionality added in a backwards\-compatible way
+ PATCH versions are for backwards\-compatible bug fixes and non\-functional changes

We recommend always staying on the latest patch version within a given MAJOR\.MINOR version in order to have the latest performance and stability improvements\. Beginning with Redis 6\.0, ElastiCache for Redis will offer a single version for each Redis OSS minor release, rather than offering multiple patch versions\. ElastiCache for Redis will automatically manage the patch version of your running cache clusters, ensuring improved performance and enhanced security\.

We also recommend periodically upgrading to the latest major version, since most major improvements are not back ported to older versions\. When doing an upgrade that spans major or minor versions, please consider the following list which includes behavior and backwards incompatible changes released with Redis over time\. 

## Redis 2\.8 behavior and backwards incompatible changes<a name="VersionManagementConsiderations-redis28"></a>

For a full list of changes, see [Redis 2\.8 release notes](https://raw.githubusercontent.com/redis/redis/2.8/00-RELEASENOTES)\. 
+ Starting in Redis 2\.8\.22, Redis AOF is no longer supported in ElastiCache for Redis\. We recommend using MemoryDB when data needs to be persisted durably\.
+ Starting in Redis 2\.8\.22, ElastiCache for Redis no longer supports attaching replicas to primaries hosted within ElastiCache\. While upgrading, external replicas will be disconnected and they will be unable to reconnect\. We recommend using client\-side caching, made available in Redis 6\.0 as an alternative to external replicas\.
+ The `TTL` and `PTTL` commands now return \-2 if the key does not exist and \-1 if it exists but has no associated expire\. Redis 2\.6 and previous versions used to return \-1 for both the conditions\.
+ `SORT` with `ALPHA` now sorts according to local collation locale if no `STORE` option is used\.

## Redis 3\.2 behavior and backwards incompatible changes<a name="VersionManagementConsiderations-redis32"></a>

For a full list of changes, see [Redis 3\.2 release notes](https://raw.githubusercontent.com/redis/redis/3.2/00-RELEASENOTES)\. 
+ There are no compatibility changes to call out for this version\.

## Redis 4\.0 behavior and backwards incompatible changes<a name="VersionManagementConsiderations-redis40"></a>

For a full list of changes, see [Redis 4\.0 release notes](https://raw.githubusercontent.com/redis/redis/4.0/00-RELEASENOTES)\. 
+ Slow log now logs an additional two arguments, the client name and address\. This change should be backwards compatible unless you are explicitly relying on each slow log entry containing 3 values\.
+ The `CLUSTER NODES` command now returns a slight different format, which is not backwards compatible\. We recommend that clients don’t use this command for learning about the nodes present in a cluster, and instead they should use `CLUSTER SLOTS`\.

## Redis 5\.0 behavior and backwards incompatible changes<a name="VersionManagementConsiderations-redis50"></a>

For a full list of changes, see [Redis 5\.0 release notes](https://raw.githubusercontent.com/redis/redis/5.0/00-RELEASENOTES)\. 
+ Scripts are by replicated by effects instead of re\-executing the script on the replica\. This generally improves performance but may increase the amount of data replicated between primaries and replicas\. There is an option to revert back to the previous behavior that is only available in ElastiCache for Redis 5\.0\.
+ If you are upgrading from Redis 4\.0, some commands in LUA scripts will return arguments in a different order than they did in earlier versions\. In Redis 4\.0, Redis would order some responses lexographically in order to make the responses deterministic, this ordering is not applied when scripts are replicated by effects\.
+ Starting in Redis 5\.0\.3, ElastiCache for Redis will offload some IO work to background cores on instance types with more than 4 VCPUs\. This may change the performance characteristics Redis and change the values of some metrics\. For more information, see [ Which metrics should I monitor?  Lists best practices for which CloudWatch metrics to use to monitor your ElastiCache performance\.   The following CloudWatch metrics offer good insight into ElastiCache performance\. In most cases, we recommend that you set CloudWatch alarms for these metrics so that you can take corrective action before performance issues occur\. Metrics to Monitor  CPUUtilization  This is a host\-level metric reported as a percent\. For more information, see [Host\-Level Metrics](CacheMetrics.HostLevel.md)\. Because Memcached is multi\-threaded, this metric can be as high as 90%\. If you exceed this threshold, scale your cache cluster up by using a larger cache node type, or scale out by adding more cache nodes\.   SwapUsage  This is a host\-level metric reported in bytes\. For more information, see [Host\-Level Metrics](CacheMetrics.HostLevel.md)\. This metric should not exceed 50 MB\. If it does, we recommend that you increase the *ConnectionOverhead parameter value*\.   Evictions  This is a cache engine metric\. We recommend that you determine your own alarm threshold for this metric based on your application needs\. If you exceed your chosen threshold, scale your cluster up by using a larger node type, or scale out by adding more nodes\.   CurrConnections  This is a cache engine metric\. We recommend that you determine your own alarm threshold for this metric based on your application needs\. An increasing number of *CurrConnections* might indicate a problem with your application; you will need to investigate the application behavior to address this issue\.  ](CacheMetrics.WhichShouldIMonitor.md) to understand if you need to change which metrics you watch\.

## Redis 6\.0 behavior and backwards incompatible changes<a name="VersionManagementConsiderations-redis60"></a>

For a full list of changes, see [Redis 6\.0 release notes](https://raw.githubusercontent.com/redis/redis/6.0/00-RELEASENOTES)\. 
+ The maximum number of allowed databases has been decreased from 1\.2 million to 10 thousand\. The default value is 16, and we discourage using values much larger than this as we’ve found performance and memory concerns\.
+ Set `AutoMinorVersionUpgrade` parameter to yes, and ElastiCache for Redis will manage the minor version upgrade through self\-service updates\. This will be handled through standard customer\-notification channels via a self\-service update campaign\. For more information, see [Self\-service updates in ElastiCache](mazonElastiCache/latest/red-ug/Self-Service-Updates.html)\.

## Redis 6\.2 behavior and backwards incompatible changes<a name="VersionManagementConsiderations-redis62"></a>

For a full list of changes, see [Redis 6\.2 release notes](https://raw.githubusercontent.com/redis/redis/6.2/00-RELEASENOTES)\. 
+ The ACL flags of the `TIME`, `ECHO`, `ROLE`, and `LASTSAVE` commands were changed\. This may cause commands that were previously allowed to be rejected and vice versa\. 
**Note**  
None of these commands modify or give access to data\.
+ When upgrading from Redis 6\.0, the ordering of key/value pairs returned from a map response to a lua script are changed\. If your scripts use `redis.setresp()` or return a map \(new in Redis 6\.0\), consider the implications that the script may break on upgrades\.