# Upgrading engine versions<a name="VersionManagement"></a>

You can control if and when the protocol\-compliant software powering your cache cluster is upgraded to new versions that are supported by ElastiCache\. This level of control enables you to maintain compatibility with specific versions, test new versions with your application before deploying in production, and perform version upgrades on your own terms and timelines\.

Because version upgrades might involve some compatibility risk, they don't occur automatically\. You must initiate them\. 

To upgrade to a newer Memcached version, modify your cache cluster specifying the new engine version you want to use\. Upgrading to a newer Memcached version is a destructive process – you lose your data and start with a cold cache\. For more information, see [Modifying an ElastiCache cluster](Clusters.Modify.md)\.

You should be aware of the following requirements when upgrading from an older version of Memcached to Memcached version 1\.4\.33 or newer\. `CreateCacheCluster` and `ModifyCacheCluster` fails under the following conditions:
+ If `slab_chunk_max > max_item_size`\.
+ If `max_item_size modulo slab_chunk_max != 0`\.
+ If `max_item_size > ((max_cache_memory - memcached_connections_overhead) / 4)`\.

  The value `(max_cache_memory - memcached_connections_overhead)` is the node's memory useable for data\. For more information, see [Memcached connection overhead](ParameterGroups.Memcached.md#ParameterGroups.Memcached.Overhead)\.

## Upgrade considerations<a name="VersionManagement-upgrade-considerations"></a>

Consider the following when choosing to upgrade:
+ Engine version management is designed so that you can have as much control as possible over how patching occurs\. However, ElastiCache reserves the right to patch your cluster on your behalf in the unlikely event of a critical security vulnerability in the system or cache software\.
+ Beginning with Redis 6\.0, ElastiCache for Redis will offer a single version for each Redis OSS minor release, rather than offering multiple patch versions\.
+ Because the Memcached engine does not support persistence, Memcached engine version upgrades are always a disruptive process that clears all cache data in the cluster\.

## How to upgrade engine versions<a name="VersionManagement.HowTo"></a>

To start version upgrades to your cluster, you modify it and specify a newer engine version\. You can do this by using the ElastiCache console, the AWS CLI, or the ElastiCache API:
+ To use the AWS Management Console, see – [Using the AWS Management Console](Clusters.Modify.md#Clusters.Modify.CON)\.
+ To use the AWS CLI, see [Using the AWS CLI](Clusters.Modify.md#Clusters.Modify.CLI)\.
+ To use the ElastiCache API, see [Using the ElastiCache API](Clusters.Modify.md#Clusters.Modify.API)\.