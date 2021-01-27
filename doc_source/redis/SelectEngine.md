# Comparing Memcached and Redis<a name="SelectEngine"></a>

Amazon ElastiCache supports the Memcached and Redis cache engines\. Each engine provides some advantages\. Use the information in this topic to help you choose the engine and version that best meets your requirements\.

**Important**  
After you create a cache cluster or replication group, you can upgrade to a newer engine version, but you cannot downgrade to an older engine version\. If you want to use an older engine version, you must delete the existing cache cluster or replication group and create it again with the earlier engine version\.

On the surface, the engines look similar\. Each of them is an in\-memory key\-value store\. However, in practice there are significant differences\. 

**Choose Memcached if the following apply for you:**
+ You need the simplest model possible\.
+ You need to run large nodes with multiple cores or threads\.
+ You need the ability to scale out and in, adding and removing nodes as demand on your system increases and decreases\.
+ You need to cache objects\.

**Choose Redis with a version of ElastiCache for Redis if the following apply for you:**
+ **ElastiCache for Redis version 6\.x \(Enhanced\)**

  You want to authenticate users with role\-based access control\.

  For more information, see [Redis Version 6\.x \(Enhanced\)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/supported-engine-versions.html#redis-version-6.x)\.
+ **ElastiCache for Redis version 5\.0\.0 \(Enhanced\)**

  You want to use [ Redis streams](https://redis.io/topics/streams-intro), a log data structure that allows producers to append new items in real time and also allows consumers to consume messages either in a blocking or non\-blocking fashion\.

  For more information, see [Redis Version 5\.0\.0 \(Enhanced\)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/supported-engine-versions.html#redis-version-5-0)\.
+ **ElastiCache for Redis version 4\.0\.10 \(Enhanced\)**

  Supports both encryption and dynamically adding or removing shards from your Redis \(cluster mode enabled\) cluster\.

  For more information, see [Redis Version 4\.0\.10 \(Enhanced\)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/supported-engine-versions.html#redis-version-4-0-10)\.
+ **ElastiCache for Redis version 3\.2\.10 \(Enhanced\)**

  Supports the ability to dynamically add or remove shards from your Redis \(cluster mode enabled\) cluster\.
**Important**  
Currently ElastiCache for Redis 3\.2\.10 doesn't support encryption\.

  For more information, see the following:
  + [Redis Version 3\.2\.10 \(Enhanced\)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/supported-engine-versions.html#redis-version-3-2-10)
  + Online resharding best practices for Redis, For more information, see the following:
    + [Best Practices: Online Resharding](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/best-practices-online-resharding.html)
    + [Online Resharding and Shard Rebalancing for Redis \(Cluster Mode Enabled\)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/redis-cluster-resharding-online.html)
  + For more information on scaling Redis clusters, see [Scaling](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Scaling.html)\.

    
+ **ElastiCache for Redis version 3\.2\.6 \(Enhanced\)**

  If you need the functionality of earlier Redis versions plus the following features, choose ElastiCache for Redis 3\.2\.6:
  + In\-transit encryption\. For more information, see [Amazon ElastiCache for Redis In\-Transit Encryption](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/in-transit-encryption.html)\.
  + At\-rest encryption\. For more information, see [Amazon ElastiCache for Redis At\-Rest Encryption](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/at-rest-encryption.html)\.
  + HIPAA eligibility certification\. For more information, see [HIPAA Eligibility for Amazon ElastiCache for Redis](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/elasticache-compliance-hipaa.html)\.

    
+ **ElastiCache for Redis \(Cluster mode enabled\) version 3\.2\.4**

  If you need the functionality of Redis 2\.8\.x plus the following features, choose Redis 3\.2\.4 \(clustered mode\):
  + You need to partition your data across two to 250 node groups \(clustered mode only\)\.
  + You need geospatial indexing \(clustered mode or non\-clustered mode\)\.
  + You don't need to support multiple databases\.
**Important**  
Redis \(cluster mode enabled\) has the following limitations:  
No scale\-up to larger node types
No changing the number of replicas in a node group \(partition\)
+ **ElastiCache for Redis \(non\-clustered mode\) 2\.8x and 3\.2\.4 \(Enhanced\)**

  If the following apply for you, choose Redis 2\.8\.x or Redis 3\.2\.4 \(non\-clustered mode\):
  + You need complex data types, such as strings, hashes, lists, sets, sorted sets, and bitmaps\.
  + You need to sort or rank in\-memory datasets\.
  + You need persistence of your key store\.
  + You need to replicate your data from the primary to one or more read replicas for read intensive applications\.
  + You need automatic failover if your primary node fails\.
  + You need publish and subscribe \(pub/sub\) capabilities—to inform clients about events on the server\.
  + You need backup and restore capabilities\.
  + You need to support multiple databases\.


**Comparison summary of Memcached, Redis \(cluster mode disabled\), and Redis \(cluster mode enabled\)**  

|  |  Memcached  |  Redis \(cluster mode disabled\)  |  Redis \(cluster mode enabled\)  | 
| --- |--- |--- |--- |
| Engine versions | 1\.5\.x  | 2\.8\.x and later | 3\.2\.x and later | 
| Data types | Simple  | 2\.8\.x \- Complex \* | 3\.2\.x and later \- Complex  | 
| Complex  | 
| Data partitioning | Yes | No | Yes | 
| Cluster is modifiable | Yes | Yes | 3\.2\.10 and later \- Limited | 
| Online resharding | No | No | 3\.2\.10 and later | 
| Encryption | No | 3\.2\.6, 4\.0\.10 and later | 3\.2\.6, 4\.0\.10 and later | 
| Compliance certifications | 
| --- |
| Compliance Certification     FedRAMP     HIPAA     PCI DSS |   No No No |   3\.2\.6, 4\.0\.10 and later 3\.2\.6, 4\.0\.10 and later 3\.2\.6, 4\.0\.10 and later |   3\.2\.6, 4\.0\.10 and later 3\.2\.6, 4\.0\.10 and later 3\.2\.6, 4\.0\.10 and later | 
| Multi\-threaded | Yes | No | No | 
| Node type upgrade | No | Yes | Yes | 
| Engine upgrading | Yes | Yes | Yes | 
| High availability \(replication\) | No | Yes | Yes | 
| Automatic failover | No | Optional | Required | 
| Pub/Sub capabilities | No | Yes | Yes | 
| Sorted sets | No | Yes | Yes | 
| Backup and restore | No | Yes | Yes | 
| Geospatial indexing | No | 2\.8\.x \- No | Yes | 
| 3\.2\.x and later \- Yes | 
| Notes: | 
|  string, objects \(like databases\) | 
| \* string, sets, sorted sets, lists, hashes, bitmaps, hyperloglog | 
|  string, sets, sorted sets, lists, hashes, bitmaps, hyperloglog, geospatial indexes | 

After you choose the engine for your cluster, we recommend that you use the most recent version of that engine\. For more information, see [Supported ElastiCache for Memcached Versions](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/CacheNodes.SupportedTypes.html) or [Supported ElastiCache for Redis Versions](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/CacheNodes.SupportedTypes.html)\.