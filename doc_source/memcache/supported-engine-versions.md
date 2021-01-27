# Supported ElastiCache for Memcached Versions<a name="supported-engine-versions"></a>

ElastiCache supports the following Memcached versions and upgrading to newer versions\. When upgrading to a newer version, pay careful attention to the conditions that if not met cause your upgrade to fail\.

**Topics**
+ [Memcached Version 1\.6\.6](#memcached-version-1-6-6)
+ [Memcached Version 1\.5\.16](#memcached-version-1-5-16)
+ [Memcached Version 1\.5\.10](#memcached-version-1-5-10)
+ [Memcached Version 1\.4\.34](#memcached-version-1-4-34)
+ [Memcached Version 1\.4\.33](#memcached-version-1-4-33)
+ [Memcached Version 1\.4\.24](#memcached-version-1-4-24)
+ [Memcached Version 1\.4\.14](#memcached-version-1-4-14)
+ [Memcached Version 1\.4\.5](#memcached-version-1-4-5)

## Memcached Version 1\.6\.6<a name="memcached-version-1-6-6"></a>

ElastiCache for Memcached adds support for Memcached version 1\.6\.6\. It includes no new features, but does include bug fixes and cumulative updates from [Memcached 1\.5\.16](https://github.com/memcached/memcached/wiki/ReleaseNotes1.5.16)\. ElastiCache for Memcached does not include support for [Extstore](https://memcached.org/extstore)\.

For more information, see [ReleaseNotes166](https://github.com/memcached/memcached/wiki/ReleaseNotes166) at Memcached on GitHub\.

## Memcached Version 1\.5\.16<a name="memcached-version-1-5-16"></a>

ElastiCache for Memcached adds support for Memcached version 1\.5\.16\. It includes no new features, but does include bug fixes and cumulative updates from [Memcached 1\.5\.14](https://github.com/memcached/memcached/wiki/ReleaseNotes1514) and [Memcached 1\.5\.15](https://github.com/memcached/memcached/wiki/ReleaseNotes1515)\.

For more information, see [Memcached 1\.5\.16 Release Notes](https://github.com/memcached/memcached/wiki/ReleaseNotes1516) at Memcached on GitHub\.

## Memcached Version 1\.5\.10<a name="memcached-version-1-5-10"></a>

ElastiCache for Memcached version 1\.5\.10 supports the following Memcached features:
+ Automated slab rebalancing\.
+ Faster hash table lookups with `murmur3` algorithm\.
+ Segmented LRU algorithm\.
+ LRU crawler to background\-reclaim memory\.
+ `--enable-seccomp`: A compile\-time option\.

It also introduces the `no_modern` and `inline_ascii_resp` parameters\. For more information, see [Memcached 1\.5\.10 Parameter Changes](ParameterGroups.Memcached.md#ParameterGroups.Memcached.1-5-10)\.

Memcached improvements added since ElastiCache for Memcached version 1\.4\.34 include the following:
+ Cumulative fixes, such as ASCII multigets, CVE\-2017\-9951 and limit crawls for `metadumper`\. 
+ Better connection management by closing connections at the connection limit\. 
+ Improved item\-size management for item size above 1MB\. 
+ Better performance and memory\-overhead improvements by reducing memory requirements per\-item by a few bytes\.

For more information, see [Memcached 1\.5\.10 Release Notes](https://github.com/memcached/memcached/wiki/ReleaseNotes1510) at Memcached on GitHub\.

## Memcached Version 1\.4\.34<a name="memcached-version-1-4-34"></a>

ElastiCache for Memcached version 1\.4\.34 adds no new features to version 1\.4\.33\. Version 1\.4\.34 is a bug fix release that is larger than the usual such release\.

For more information, see [Memcached 1\.4\.34 Release Notes](https://github.com/memcached/memcached/wiki/ReleaseNotes1434) at Memcached on GitHub\.

## Memcached Version 1\.4\.33<a name="memcached-version-1-4-33"></a>

Memcached improvements added since version 1\.4\.24 include the following:
+ Ability to dump all of the metadata for a particular slab class, a list of slab classes, or all slab classes\. For more information, see [Memcached 1\.4\.31 Release Notes](https://github.com/memcached/memcached/wiki/ReleaseNotes1431)\.
+ Improved support for large items over the 1 megabyte default\. For more information, see [Memcached 1\.4\.29 Release Notes](https://github.com/memcached/memcached/wiki/ReleaseNotes1429)\.
+ Ability to specify how long a client can be idle before being asked to close\.

  Ability to dynamically increase the amount of memory available to Memcached without having to restart the cluster\. For more information, see [Memcached 1\.4\.27 Release Notes](https://github.com/memcached/memcached/wiki/ReleaseNotes1427)\.
+ Logging of `fetchers`, `mutations`, and `evictions` are now supported\. For more information, see [Memcached 1\.4\.26 Release Notes](https://github.com/memcached/memcached/wiki/ReleaseNotes1426)\.
+ Freed memory can be reclaimed back into a global pool and reassigned to new slab classes\. For more information, see [Memcached 1\.4\.25 Release Notes](https://github.com/memcached/memcached/wiki/ReleaseNotes1425)\.
+ Several bug fixes\.
+ Some new commands and parameters\. For a list, see [Memcached 1\.4\.33 Added Parameters](ParameterGroups.Memcached.md#ParameterGroups.Memcached.1-4-33)\.

## Memcached Version 1\.4\.24<a name="memcached-version-1-4-24"></a>

Memcached improvements added since version 1\.4\.14 include the following:
+ Least recently used \(LRU\) management using a background process\.
+ Added the option of using *jenkins* or *murmur3* as your hash algorithm\.
+ Some new commands and parameters\. For a list, see [Memcached 1\.4\.24 Added Parameters](ParameterGroups.Memcached.md#ParameterGroups.Memcached.1-4-24)\.
+ Several bug fixes\.

## Memcached Version 1\.4\.14<a name="memcached-version-1-4-14"></a>

Memcached improvements added since version 1\.4\.5 include the following:
+ Enhanced slab rebalancing capability\.
+ Performance and scalability improvement\.
+ Introduced the *touch* command to update the expiration time of an existing item without fetching it\.
+ Auto discovery—the ability for client programs to automatically determine all of the cache nodes in a cluster, and to initiate and maintain connections to all of these nodes\.

## Memcached Version 1\.4\.5<a name="memcached-version-1-4-5"></a>

Memcached version 1\.4\.5 was the initial engine and version supported by Amazon ElastiCache for Memcached\.