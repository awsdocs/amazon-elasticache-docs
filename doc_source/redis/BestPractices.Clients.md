# Best practices: Redis clients and ElastiCache for Redis<a name="BestPractices.Clients"></a>

To learn more about best practices for interacting with ElastiCache for Redis resources with commonly used open\-source Redis client libraries, see [Best practices: Redis clients and Amazon ElastiCache for Redis](http://aws.amazon.com/blogs/database/best-practices-redis-clients-and-amazon-elasticache-for-redis/)\. 

**Note**  
Cluster mode disabled clusters don’t support the cluster discovery commands and aren’t compatible with all clients dynamic topology discovery functionality\.  
Cluster mode disabled with ElastiCache isn’t compatible with Lettuce’s `MasterSlaveTopologyRefresh`\. Instead, for cluster mode disabled you can configure a `StaticMasterReplicaTopologyProvider` and provide the cluster read and write endpoints\.  
For more information on connecting to cluster mode disabled clusters, see [Finding a Redis \(Cluster Mode Disabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Redis)\.  
If you wish to use Lettuce’s dynamic topology discovery functionality, then you can create a cluster mode enabled cluster with the same shard configuration as your existing cluster\. However, for cluster mode enabled clusters we recommend configuring at least 3 shards with at least one 1 replica to support fast failover\.

**Topics**
+ [Lettuce client configuration](BestPractices.Clients-lettuce.md)