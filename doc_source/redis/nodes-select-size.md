# Choosing your node size<a name="nodes-select-size"></a>

The node size you select for your cluster impacts costs, performance, and fault tolerance\. 

## Choosing your node size<a name="CacheNodes.SelectSize"></a>

Answering the following questions can help you determine the minimum node type you need for your Redis implementation:
+ Do you expect throughput\-bound workloads with multiple client connections?

  If this is the case and you are running Redis version 5\.0\.3 or higher, you can get better throughput and latency using [Enhanced IO](https://aws.amazon.com/blogs/database/boosting-application-performance-and-reducing-costs-with-amazon-elasticache-for-redis/), where available CPUs are used for offloading the client connections, on behalf of the Redis engine\. If you are running Redis version 7\.0\.4 or higher, on top of Enhanced IO, you will get additional acceleration with enhanced I/O multiplexing, where each dedicated network IO thread pipelines commands from multiple clients into the Redis engine, taking advantage of Redis' ability to efficiently process commands in batches\. You can refer to the [Better Throughput with enhanced I/O multiplexing](https://aws.amazon.com/blogs/database/enhanced-io-multiplexing-for-amazon-elasticache-for-redis) blog post and [Supported node types](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/CacheNodes.SupportedTypes.html) for additional details\.
+ Do you have workloads that access a small percentage of their data regularly?

  If this is the case and you are running on Redis engine version 6\.2 or later, you can leverage data tiering by choosing the r6gd node type\. With data tiering, least\-recently used data is stored in SSD\. When it is retrieved there is a small latency cost, which is balanced by cost savings\. For more information, see [Data tiering](data-tiering.md)\.

  For more information, see [Supported node types](CacheNodes.SupportedTypes.md)\.
+ How much total memory do you need for your data?

  To get a general estimate, take the size of the items that you want to cache\. Multiply this size by the number of items that you want to keep in the cache at the same time\. To get a reasonable estimation of the item size, first serialize your cache items, then count the characters\. Then divide this over the number of shards in your cluster\.

  For more information, see [Supported node types](CacheNodes.SupportedTypes.md)\.
+ What version of Redis are you running?

  Redis versions before 2\.8\.22 require you to reserve more memory for failover, snapshot, synchronizing, and promoting a replica to primary operations\. This requirement occurs because you must have sufficient memory available for all writes that occur during the process\. 

  Redis version 2\.8\.22 and later use a forkless save process that requires less available memory than the earlier process\.

  For more information, see the following:
  + [How synchronization and backup are implemented](Replication.Redis.Versions.md)
  + [Ensuring that you have enough memory to create a Redis snapshot](BestPractices.BGSAVE.md)
+ How write\-heavy is your application?

  Write heavy applications can require significantly more available memory, memory not used by data, when taking snapshots or failing over\. Whenever the `BGSAVE` process is performed, you must have sufficient memory that is unused by data to accommodate all the writes that transpire during the `BGSAVE` process\. Examples are when taking a snapshot, when syncing a primary cluster with a replica in a cluster, and when enabling the append\-only file \(AOF\) feature\. Another is when promoting a replica to primary \(if you have Multi\-AZ enabled\)\. The worst case is when all of your data is rewritten during the process\. In this case, you need a node instance size with twice as much memory as needed for data alone\.

  For more detailed information, see [Ensuring that you have enough memory to create a Redis snapshot](BestPractices.BGSAVE.md)\.
+ Will your implementation be a standalone Redis \(cluster mode disabled\) cluster or a Redis \(cluster mode enabled\) cluster with multiple shards?

**Redis \(cluster mode disabled\) cluster**  
If you're implementing a Redis \(cluster mode disabled\) cluster, your node type must be able to accommodate all your data plus the necessary overhead as described in the previous bullet\.

  For example, suppose that you estimate that the total size of all your items is 12 GB\. In this case, you can use a `cache.m3.xlarge` node with 13\.3 GB of memory or a `cache.r3.large` node with 13\.5 GB of memory\. However, you might need more memory for `BGSAVE` operations\. If your application is write\-heavy, double the memory requirements to at least 24 GB\. Thus, use either a `cache.m3.2xlarge` with 27\.9 GB of memory or a `cache.r3.xlarge` with 30\.5 GB of memory\.

**Redis \(cluster mode enabled\) with multiple shards**  
If you're implementing a Redis \(cluster mode enabled\) cluster with multiple shards, then the node type must be able to accommodate `bytes-for-data-and-overhead / number-of-shards` bytes of data\.

  For example, suppose that you estimate that the total size of all your items to be 12 GB and you have two shards\. In this case, you can use a `cache.m3.large` node with 6\.05 GB of memory \(12 GB / 2\)\. However, you might need more memory for `BGSAVE` operations\. If your application is write\-heavy, double the memory requirements to at least 12 GB per shard\. Thus, use either a `cache.m3.xlarge` with 13\.3 GB of memory or a `cache.r3.large` with 13\.5 GB of memory\.
+ Are you using Local Zones?

[Local Zones](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Local_zones.html) enable you to place resources such as an ElastiCache cluster in multiple locations close to your users\. But when you choose your node size, be aware that the available node sizes are limited to the following at this time, regardless of capacity requirements:
  + Current generation: 

    **M5 node types:** `cache.m5.large`, `cache.m5.xlarge`, `cache.m5.2xlarge`, `cache.m5.4xlarge`, `cache.m5.12xlarge`, `cache.m5.24xlarge` 

    **R5 node types:** `cache.r5.large`, `cache.r5.xlarge`, `cache.r5.2xlarge`, `cache.r5.4xlarge`, `cache.r5.12xlarge`, `cache.r5.24xlarge`

    **T3 node types:** `cache.t3.micro`, `cache.t3.small`, `cache.t3.medium`

While your cluster is running, you can monitor the memory usage, processor utilization, cache hits, and cache misses metrics that are published to CloudWatch\. You might notice that your cluster doesn't have the hit rate that you want or that keys are being evicted too often\. In these cases, you can choose a different node size with larger CPU and memory specifications\.

When monitoring CPU usage, remember the Redis is single\-threaded\. Thus, multiply the reported CPU usage by the number of CPU cores to get that actual usage\. For example, a four\-core CPU reporting a 20\-percent usage rate is actually the one core Redis is running at 80 percent utilization\.