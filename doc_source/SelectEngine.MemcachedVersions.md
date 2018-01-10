# ElastiCache for Memcached Versions<a name="SelectEngine.MemcachedVersions"></a>

ElastiCache supports the following Memcached versions and upgrading to newer versions\. When upgrading to a newer version, pay careful attention to the conditions which if not met will cause your upgrade to fail\.


+ [Upgrading to a Newer Version](#SelectEngine.MemcachedVersions.Upgrading)
+ [Memcached Version 1\.4\.34](#SelectEngine.MemcachedVersions.1-4-34)
+ [Memcached Version 1\.4\.33](#SelectEngine.MemcachedVersions.1-4-33)
+ [Memcached Version 1\.4\.24](#SelectEngine.MemcachedVersions.1-4-24)
+ [Memcached Version 1\.4\.14](#SelectEngine.MemcachedVersions.1-4-14)
+ [Memcached Version 1\.4\.5](#SelectEngine.MemcachedVersions.1-4-5)

## Upgrading to a Newer Version<a name="SelectEngine.MemcachedVersions.Upgrading"></a>

To upgrade to a newer Memcached version, modify your cache cluster specifying the new engine version you want to use\. Upgrading to a newer Memcached version is a destructive process – you lose your data and start with a cold cache\. For more information, see [Modifying an ElastiCache Cluster](Clusters.Modify.md)\.

You should be aware of the following requirements when upgrading from an older version of Memcached to Memcached version 1\.4\.33 or newer\. `CreateCacheCluster` and `ModifyCacheCluster` fails under the following conditions:

+ If `slab_chunk_max > max_item_size`\.

+ If `max_item_size modulo slab_chunk_max != 0`\.

+ If `max_item_size > ((max_cache_memory - memcached_connections_overhead) / 4)`\.

  The value `(max_cache_memory - memcached_connections_overhead)` is the node's memory useable for data\. For more information, see [Memcached Connection Overhead](ParameterGroups.Memcached.md#ParameterGroups.Memcached.Overhead)\.

## Memcached Version 1\.4\.34<a name="SelectEngine.MemcachedVersions.1-4-34"></a>

ElastiCache for Memcached version 1\.4\.34 adds no new features to version 1\.4\.33\. Version 1\.4\.34 is a bug fix release that is larger than the usual such release\.

For more information, see [Memcached 1\.4\.34 Release Notes](https://github.com/memcached/memcached/wiki/ReleaseNotes1434) at Memcached on GitHub\.

## Memcached Version 1\.4\.33<a name="SelectEngine.MemcachedVersions.1-4-33"></a>

Memcached improvements added since version 1\.4\.24 include the following:

+ Ability to dump all of the metadata for a particular slab class, a list of slab classes, or all slab classes\. For more information, see [Memcached 1\.4\.31 Release Notes](https://github.com/memcached/memcached/wiki/ReleaseNotes1431)\.

+ Improved support for large items over the 1 megabyte default\. For more information, see [Memcached 1\.4\.29 Release Notes](https://github.com/memcached/memcached/wiki/ReleaseNotes1429)\.

+ Ability to specify how long a client can be idle before being asked to close\.

  Ability to dynamically increase the amount of memory available to Memcached without having to restart the cluster\. For more information, see [Memcached 1\.4\.27 Release Notes](https://github.com/memcached/memcached/wiki/ReleaseNotes1427)\.

+ Logging of `fetchers`, `mutations`, and `evictions` are now supported\. For more information, see [Memcached 1\.4\.26 Release Notes](https://github.com/memcached/memcached/wiki/ReleaseNotes1426)\.

+ Freed memory can be reclaimed back into a global pool and reassigned to new slab classes\. For more information, see [Memcached 1\.4\.25 Release Notes](https://github.com/memcached/memcached/wiki/ReleaseNotes1425)\.

+ Several bug fixes\.

+ Some new commands and parameters\. For a list, see [Memcached 1\.4\.33 Added Parameters](ParameterGroups.Memcached.md#ParameterGroups.Memcached.1-4-33)\.

## Memcached Version 1\.4\.24<a name="SelectEngine.MemcachedVersions.1-4-24"></a>

Memcached improvements added since version 1\.4\.14 include the following:

+ Least recently used \(LRU\) management using a background process\.

+ Added the option of using *jenkins* or *murmur3* as your hash algorithm\.

+ Some new commands and parameters\. For a list, see [Memcached 1\.4\.24 Added Parameters](ParameterGroups.Memcached.md#ParameterGroups.Memcached.1-4-24)\.

+ Several bug fixes\.

## Memcached Version 1\.4\.14<a name="SelectEngine.MemcachedVersions.1-4-14"></a>

Memcached improvements added since version 1\.4\.5 include the following:

+ Enhanced slab rebalancing capability\.

+ Performance and scalability improvement\.

+ Introduced the *touch* command to update the expiration time of an existing item without fetching it\.

+ Auto discovery—the ability for client programs to automatically determine all of the cache nodes in a cluster, and to initiate and maintain connections to all of these nodes\.

## Memcached Version 1\.4\.5<a name="SelectEngine.MemcachedVersions.1-4-5"></a>

Memcached version 1\.4\.5 was the initial engine and version supported by Amazon ElastiCache\.