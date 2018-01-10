# Choosing Your Node Size<a name="CacheNodes.SelectSize"></a>

This section helps you determine what node instance type you need for your scenarios\. Since the engines, Memcached and Redis, implement clusters differently, the engine you choose will make a difference in the node size you needed by your application\.


+ [Choosing Your Node Size for Memcached Clusters](#CacheNodes.SelectSize.Memcached)
+ [Choosing Your Node Size for Redis Clusters](#CacheNodes.SelectSize.Redis)

## Choosing Your Node Size for Memcached Clusters<a name="CacheNodes.SelectSize.Memcached"></a>

Memcached clusters contain one or more nodes\. Because of this, the memory needs of the cluster and the memory of a node are related, but not the same\. You can attain your needed cluster memory capacity by having a few large nodes or many smaller nodes\. Further, as your needs change, you can add or remove nodes from the cluster and thus pay only for what you need\.

The total memory capacity of your cluster is calculated by multiplying the number of nodes in the cluster by the RAM capacity of each node\. The capacity of each node is based on the node type\.

The number of nodes in the cluster is a key factor in the availability of your cluster running Memcached\. The failure of a single node can have an impact on the availability of your application and the load on your back\-end database while ElastiCache provisions a replacement for the failed node and it gets repopulated\. You can reduce this potential availability impact by spreading your memory and compute capacity over a larger number of nodes, each with smaller capacity, rather than using a fewer number of high capacity nodes\.

In a scenario where you want to have 40 GB of cache memory, you can set it up in any of the following configurations:

+ 13 `cache.t2.medium` nodes with 3\.22 GB of memory and 2 threads each = 41\.86 GB and 26 threads\.

   

+ 7 `cache.m3.large` nodes with 6\.05 GB of memory and 2 threads each = 42\.35 GB and 14 threads\.

  7 `cache.m4.large` nodes with 6\.42 GB of memory and 2 threads each = 44\.94 GB and 14 threads\.

   

+ 3 `cache.r3.large` nodes with 13\.50 GB of memory and 2 threads each = 40\.50 GB and 6 threads\.

  3 `cache.m4.xlarge` nodes with 14\.28 GB of memory and 4 threads each = 42\.84 GB and 12 threads\.


**Comparing node options**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/CacheNodes.SelectSize.html)

These options each provide similar memory capacity but different computational capacity and cost\. To compare the costs of your specific options, see [Amazon ElastiCache Pricing](https://aws.amazon.com/elasticache/pricing/)\.

For clusters running Memcached, some of the available memory on each node is used for connection overhead\. For more information, see [Memcached Connection Overhead](ParameterGroups.Memcached.md#ParameterGroups.Memcached.Overhead)

Using multiple nodes will require spreading the keys across them\. Each node has its own endpoint\. For easy endpoint management, you can use the ElastiCache the Auto Discovery feature, which enables client programs to automatically identify all of the nodes in a cluster\. For more information, see [Node Auto Discovery \(Memcached\)](AutoDiscovery.md)\.

If you're unsure about how much capacity you need, for testing we recommend starting with one `cache.m3.medium` node and monitoring the memory usage, CPU utilization, and cache hit rate with the ElastiCache metrics that are published to CloudWatch\. For more information on CloudWatch metrics for ElastiCache, see [Monitoring Use with CloudWatch Metrics](CacheMetrics.md)\. For production and larger workloads, the R3 nodes provide the best performance and RAM cost value\.

If your cluster does not have the desired hit rate, you can easily add more nodes, thereby increasing the total available memory in your cluster\.

If your cluster turns out to be bound by CPU but it has sufficient hit rate, try setting up a new cluster with a node type that provides more compute power\.

## Choosing Your Node Size for Redis Clusters<a name="CacheNodes.SelectSize.Redis"></a>

Answering the following questions will help you determine the minimum node type you need for your Redis implementation\.

+ How much total memory do you need for your data?

   

  You can get a general estimate by taking the size of the items you want to cache and multiplying it by the number of items you want to keep in the cache at the same time\. To get a reasonable estimation of the item size, serialize your cache items then count the characters, then divide this over the number of shards in your cluster\.

   

+ What version of Redis are you running?

   

  Redis versions prior to 2\.8\.22 require you to reserve more memory for failover, snapshot, synchronizing, and promoting a replica to primary operations\. This requirement occurs because you must have sufficient memory available for all writes that occur during the process\. 

  Redis version 2\.8\.22 and later use a forkless save process that requires less available memory than the earlier process\.

  For more information, see the following:

  + [How Synchronization and Backup are Implemented](Replication.Redis.Versions.md)

  + [Ensuring You Have Sufficient Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)

   

+ How write\-heavy is your application?

   

  Write heavy applications can require significantly more available memory, memory not used by data, when taking snapshots or failing over\. Whenever the `BGSAVE` process is performed–when taking a snapshot, when syncing a primary cluster with a replica in a cluster, when enabling the append\-only file \(AOF\) feature, or promoting a replica to primary \(if you have Multi\-AZ with auto failover enabled\)–you must have sufficient memory that is unused by data to accommodate all the writes that transpire during the `BGSAVE` process\. Worst case would be when all of your data is rewritten during the process, in which case you would need a node instance size with twice as much memory as needed for data alone\.

   

  For more detailed information, go to [Ensuring You Have Sufficient Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)\.

   

+ Will your implementation be a standalone Redis \(cluster mode disabled\) cluster or a Redis \(cluster mode enabled\) cluster with multiple shards?

   

**Redis \(cluster mode disabled\) cluster**  
If you're implementing a Redis \(cluster mode disabled\) cluster, your node type must be able to accommodate all your data plus the necessary overhead as described in the previous bullet\.

   

  For example, if you estimate that the total size of all your items to be 12 GB, you can use a `cache.m3.xlarge` node with 13\.3 GB of memory or a `cache.r3.large` node with 13\.5 GB of memory\. However, you may need more memory for `BGSAVE` operations\. If your application is write heavy, you should double the memory requirements to at least 24 GB, meaning you should use either a `cache.m3.2xlarge` with 27\.9 GB of memory or a `cache.r3.rge` with 28\.4 GB of memory\.

   

**Redis \(cluster mode enabled\) with multiple shards**  
If you're implementing a Redis \(cluster mode enabled\) cluster with multiple shards, then the node type must be able to accommodate `bytes-for-data-and-overhead / number-of-shards` bytes of data\.

   

  For example, if you estimate that the total size of all your items to be 12 GB and you have 2 shards, you can use a `cache.m3.large` node with 6\.05 GB of memory \(12 GB / 2\)\. However, you may need more memory for `BGSAVE` operations\. If your application is write heavy, you should double the memory requirements to at least 12 GB per shard, meaning you should use either a `cache.m3.xlarge` with 13\.3 GB of memory or a `cache.r3.large` with 13\.5 GB of memory\.

   

  Currently you cannot add shards to a Redis \(cluster mode enabled\) cluster\. Therefore, you may want to use a somewhat larger node type to accommodate anticipated growth\.

   

While your cluster is running, you can monitor the memory usage, processor utilization, cache hits, and cache misses metrics that are published to CloudWatch\. If your cluster does not have the desired hit rate or you notice that keys are being evicted too often, you can choose a different node size with larger CPU and memory specifications\.

When monitoring CPU usage, remember that Redis is single\-threaded, so you need to multiply the reported CPU usage by the number of CPU cores to get that actual usage\. For example, a four core CPU reporting a 20% usage rate is actually the one core Redis is using running at 80%\.