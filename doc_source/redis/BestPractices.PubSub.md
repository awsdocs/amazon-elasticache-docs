# Best practices: Pub/Sub<a name="BestPractices.PubSub"></a>

When using ElastiCache for Redis to support Pub/Sub workloads with high throughput requirements, we recommend that you use Cluster Mode Disabled configuration\. On Cluster Mode Enabled clusters, published messages are broadcast to all other nodes over a cluster bus\. The buffer management in the cluster bus could result in high `EngineCPUUtilization` when loaded with Pub/Sub traffic\. For more information, see [Clusterbus buffer management can consume significant memory and CPU utilization during pubsub](https://github.com/redis/redis/issues/10863)\. This condition does not occur in Cluster Mode Disabled clusters, where the published messages are sent over the replication link that uses a different buffer management approach\. 

When using Redis version 7 or later, we recommend using [sharded Pub/Sub](https://redis.io/docs/manual/pubsub/#sharded-pubsub)\. You also improve throughput and latency using [enhanced I/O multiplexing](https://aws.amazon.com/blogs/database/enhanced-io-multiplexing-for-amazon-elasticache-for-redis), which is automatically available when using Redis version 7 or later and requires no client changes\. It is ideal for pub/sub workloads, which often are throughput\-bound with multiple client connections\.