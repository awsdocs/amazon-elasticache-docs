# ElastiCache Nodes<a name="CacheNodes"></a>

A node is the smallest building block of an Amazon ElastiCache deployment\. It is a fixed\-size chunk of secure, network\-attached RAM\. Each node runs either Memcached or Redis, depending on what was chosen when the cluster was created\. Each node has its own Domain Name Service \(DNS\) name and port\. Multiple types of ElastiCache nodes are supported, each with varying amounts of associated memory\.

The node instance type you need for your deployment is influenced by both the amount of data you want in your cluster and the engine you use\. Generally speaking, due to its support for sharding, Memcached deployments will have more and smaller nodes while Redis deployments will use fewer, larger node types\. See [Choosing Your Node Size for Memcached Clusters](CacheNodes.SelectSize.md#CacheNodes.SelectSize.Memcached) and [Choosing Your Node Size for Redis Clusters](CacheNodes.SelectSize.md#CacheNodes.SelectSize.Redis) for a more detailed discussion of which node size to use\.


+ [Redis Nodes and Shards](#CacheNodes.NodeGroups)
+ [Choosing Your Node Size](CacheNodes.SelectSize.md)
+ [Connecting to Nodes](nodes-connecting.md)
+ [ElastiCache Reserved Nodes](CacheNodes.Reserved.md)
+ [Supported Node Types](CacheNodes.SupportedTypes.md)
+ [Actions You Can Take When a Node is Scheduled for Replacement](CacheNodes.NodeReplacement.md)

Other ElastiCache Node Operations

Additional operations involving nodes: 

+ [Adding Nodes to a Cluster](Clusters.AddNode.md)

+ [Removing Nodes from a Cluster](Clusters.DeleteNode.md)

+ [Scaling](Scaling.md)

+ [Finding Your ElastiCache Endpoints](Endpoints.md)

+ [Node Auto Discovery \(Memcached\)](AutoDiscovery.md)

## Redis Nodes and Shards<a name="CacheNodes.NodeGroups"></a>

A shard \(API/CLI: node group\) is a hierarchical arrangement of nodes \(each wrapped in a cluster\)\. Shards support replication\. Within a shard, one node functions as the read/write primary node\. All the other nodes in a shard function as read\-only replicas of the primary node\. Redis version 3\.2 and later support multiple shards within a cluster \(API/CLI: replication group\) thereby enabling partitioning your data in a Redis \(cluster mode enabled\) cluster\. 

The following diagram illustrates the differences between a Redis \(cluster mode disabled\) cluster and a Redis \(cluster mode enabled\) cluster\.

![\[Image: Redis (cluster mode disabled) & Redis (cluster mode enabled) shards	(API/CLI: node groups)\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-NodeGroups.png)

Both Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\) support replication via shards\. The API operation, [DescribeReplicationGroups](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReplicationGroups.html) \(CLI: [describe\-replication\-groups](http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-replication-groups.html)\) lists the node groups with the member nodes, the node’s role within the node group as well as other information\.

When you create a Redis cluster, you specify whether or not you want to create a cluster with clustering enabled\. Redis \(cluster mode disabled\) clusters never have more than one shard which can be scaled horizontally by adding \(up to a total of 5\) or deleting read replica nodes\. For more information, see [ElastiCache Replication \(Redis\)](Replication.md), [Adding a Read Replica to a Redis Cluster](Replication.AddReadReplica.md) or [Deleting a Read Replica](Replication.RemoveReadReplica.md)\. Redis \(cluster mode disabled\) clusters can also scale vertically by changing node types\. For more information, see [Scaling Redis \(cluster mode disabled\) Clusters with Replica Nodes](Scaling.RedisReplGrps.md)\.

When you create a Redis \(cluster mode enabled\) cluster, you specify from 1 to 15 shards\. Currently, however, unlike Redis \(cluster mode disabled\) clusters, once a Redis \(cluster mode enabled\) cluster is created, its structure cannot be altered in any way; you cannot add or delete nodes or shards\. If you need to add or delete nodes, or change node types, you must create the cluster anew\.

When you create a new cluster, as long as the cluster group has the same number of shards as the old cluster, you can seed it with data from the old cluster so it doesn’t start out empty\. This can be helpful if you need change your node type or engine version\. For more information, see [Making Manual Backups](backups-manual.md) and [Restoring From a Backup with Optional Cluster Resizing](backups-restoring.md)\.