# Related services<a name="related-services-choose-between-memorydb-and-redis"></a>

[Amazon MemoryDB for Redis](https://docs.aws.amazon.com/memorydb/latest/devguide/what-is-memorydb-for-redis.html) 

When deciding whether to use ElastiCache for Redis or Amazon MemoryDB for Redis consider the following comparisons:
+ ElastiCache for Redis is a service that is commonly used to cache data from other databases and data stores using Redis\. You should consider ElastiCache for Redis for caching workloads where you want to accelerate data access with your existing primary database or data store \(microsecond read and write performance\)\. You should also consider ElastiCache for Redis for use cases where you want to use the Redis data structures and APIs to access data stored in a primary database or data store\.
+ Amazon MemoryDB for Redis is a durable, in\-memory database for workloads that require an ultra\-fast, primary database\. You should consider using MemoryDB if your workload requires a durable database that provides ultra\-fast performance \(microsecond read and single\-digit millisecond write latency\)\. MemoryDB may also be a good fit for your use case if you want to build an application using Redis data structures and APIs with a primary, durable database\. Finally, you should consider using MemoryDB to simplify your application architecture and lower costs by replacing usage of a database with a cache for durability and performance\. 