# ElastiCache Clusters<a name="Clusters"></a>

A *cluster* is a collection of one or more cache nodes, all of which run an instance of supported cache engine software, Memcached or Redis\. When you create a cluster, you specify the engine that all of the nodes will use\.

The following diagram illustrates a typical Memcached and a typical Redis cluster\. Memcached clusters contain from 1 to 20 nodes across which you can horizontally partition your data\. Redis clusters can contain a single node or up to 6 nodes inside a shard \(API/CLI: node group\), Redis \(cluster mode disabled\) clusters always have a single shard\. Redis \(cluster mode enabled\) clusters can have up to 15 shards, with your data partitioned across the shards\. When you have multiple nodes in a shard, one of the nodes is a read/write primary node\. All other nodes in the shard are read\-only replicas\.

![\[Image: Typical Memcached and Redis Clusters\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCacheClusters.png)

*Typical Memcached and Redis Clusters*

Most ElastiCache operations are performed at the cluster level\. You can set up a cluster with a specific number of nodes and a parameter group that controls the properties for each node\. All nodes within a cluster are designed to be of the same node type and have the same parameter and security group settings\. 

Every cluster must have a cluster identifier\. The cluster identifier is a customer\-supplied name for the cluster\. This identifier specifies a particular cluster when interacting with the ElastiCache API and AWS CLI commands\. The cluster identifier must be unique for that customer in an AWS Region\.

ElastiCache supports multiple versions of each engine\. Unless you have specific reasons, we recommend always using the your engine's latest version\.

## Memcached Versions<a name="Clusters.MemcachedVersions"></a>

+ [Memcached Version 1\.4\.34](SelectEngine.MemcachedVersions.md#SelectEngine.MemcachedVersions.1-4-34)

+ [Memcached Version 1\.4\.33](SelectEngine.MemcachedVersions.md#SelectEngine.MemcachedVersions.1-4-33)

+ [Memcached Version 1\.4\.24](SelectEngine.MemcachedVersions.md#SelectEngine.MemcachedVersions.1-4-24)

+ [Memcached Version 1\.4\.14 ](SelectEngine.MemcachedVersions.md#SelectEngine.MemcachedVersions.1-4-14)

+ [Memcached Version 1\.4\.5](SelectEngine.MemcachedVersions.md#SelectEngine.MemcachedVersions.1-4-5)

## Redis Versions<a name="Clusters.RedisVersions"></a>

+ [ElastiCache for Redis Version 3\.2\.10 \(Enhanced\)](SelectEngine.RedisVersions.md#SelectEngine.RedisVersions.3-2-10)

+ [ElastiCache for Redis Version 3\.2\.6 \(Enhanced\)](SelectEngine.RedisVersions.md#SelectEngine.RedisVersions.3-2-6)

+ [ElastiCache for Redis Version 3\.2\.4 \(Enhanced\)](SelectEngine.RedisVersions.md#SelectEngine.RedisVersions.3-2-4)

+ [ElastiCache for Redis Version 2\.8\.23 \(Enhanced\)](SelectEngine.RedisVersions.md#SelectEngine.RedisVersions.2-8-23)

+ [ElastiCache for Redis Version 2\.8\.22 \(Enhanced\)](SelectEngine.RedisVersions.md#SelectEngine.RedisVersions.2-8-22)

+ [ElastiCache for Redis Version 2\.8\.19](SelectEngine.RedisVersions.md#SelectEngine.RedisVersions.2-8-19)

+ [ElastiCache for Redis Version 2\.8\.6](SelectEngine.RedisVersions.md#SelectEngine.RedisVersions.2-8-6)

+ [ElastiCache for Redis Version 2\.6\.13](SelectEngine.RedisVersions.md#SelectEngine.RedisVersions.2-6-13)

## Other ElastiCache Cluster Operations<a name="Clusters.OtherOperations"></a>

Additional operations involving clusters: 

+ [Finding Your ElastiCache Endpoints](Endpoints.md)

+ [Accessing ElastiCache Resources from Outside AWS](Access.Outside.md)