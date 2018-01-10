# Choosing an Engine: Memcached, Redis \(cluster mode disabled\), or Redis \(cluster mode enabled\)<a name="SelectEngine.Uses"></a>

On the surface, the engines look similar\. Each of them is an in\-memory key/value store\. However, in practice there are significant differences\.

**Choose Memcached if the following apply to your situation:**

+ You need the simplest model possible\.

+ You need to run large nodes with multiple cores or threads\.

+ You need the ability to scale out/in, adding and removing nodes as demand on your system increases and decreases\.

+ You need to cache objects, such as a database\.

**Choose a version of ElastiCache for Redis if the following apply to your situation:**

+ 

**Choose ElastiCache for Redis 3\.2\.10 if you require the ability to dynamically add or remove shards from your Redis \(cluster mode enabled\) cluster:**
**Important**  
Currently ElastiCache for Redis 3\.2\.10 does not support encryption\.

  For more information, see:

  + [Best Practices: Online Resharding](best-practices-online-resharding.md)

  + At\-rest encryption\. For more information, see [Online Resharding and Shard Rebalancing for ElastiCache for Redis—Redis \(cluster mode enabled\)](scaling-redis-cluster-mode-enabled.md#redis-cluster-resharding-online)

+ 

**Choose ElastiCache for Redis 3\.2\.6 if you require all the functionality of the earlier Redis versions plus:**

  + In\-transit encryption\. For more information, see [Amazon ElastiCache for Redis In\-Transit Encryption](in-transit-encryption.md)

  + At\-rest encryption\. For more information, see [Amazon ElastiCache for Redis At\-Rest Encryption](at-rest-encryption.md)

  + HIPAA compliance certification\. For more information, see [HIPAA Compliance for Amazon ElastiCache for Redis](elasticache-compliance-hipaa.md)

     

+ 

**Choose Redis 3\.2\.4 \(clustered mode\) if you require all the functionality of Redis 2\.8\.x with the following differences:**

  + You need to partition your data across 2 to 15 node groups \(cluster mode only\)\.

  + You need geospatial indexing \(clustered mode or non\-clustered mode\)\.

  + You do not need to support multiple databases\.

  + 
**Important**  
Redis \(cluster mode enabled\) cluster mode has the following limitations:  
No scale up to larger node types\.
No changing the number of replicas in a node group \(partition\)\.

+ 

**Choose Redis 2\.8\.x or Redis 3\.2\.4 \(non\-clustered mode\) if the following apply to your situation:**

  + You need complex data types, such as strings, hashes, lists, sets, sorted sets, and bitmaps\.

  + You need to sort or rank in\-memory data\-sets\.

  + You need persistence of your key store\.

  + You need to replicate your data from the primary to one or more read replicas for read intensive applications\.

  + You need automatic failover if your primary node fails\.

  + You need publish and subscribe \(pub/sub\) capabilities—to inform clients about events on the server\.

  + You need backup and restore capabilities\.

  + You need to support multiple databases\.


**Comparison summary of Memcached, Redis \(cluster mode disabled\), and Redis \(cluster mode enabled\)**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/SelectEngine.Uses.html)

After you choose the engine for your cluster, we recommend that you use the most recent version of that engine\. For more information, see [ElastiCache for Memcached Versions](SelectEngine.MemcachedVersions.md) or [ElastiCache for Redis Versions](SelectEngine.RedisVersions.md)\.