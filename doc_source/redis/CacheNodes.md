# Managing nodes<a name="CacheNodes"></a>

A node is the smallest building block of an Amazon ElastiCache deployment\. It is a fixed\-size chunk of secure, network\-attached RAM\. Each node runs the engine that was chosen when the cluster or replication group was created or last modified\. Each node has its own Domain Name Service \(DNS\) name and port\. Multiple types of ElastiCache nodes are supported, each with varying amounts of associated memory and computational power\.

Generally speaking, due to its support for sharding, Redis \(cluster mode enabled\) deployments have a number of smaller nodes\. In contrast, Redis \(cluster mode disabled\) deployments have fewer, larger nodes in a cluster\. For a more detailed discussion of which node size to use, see [Choosing your node size](nodes-select-size.md#CacheNodes.SelectSize)\. 

**Topics**
+ [Redis nodes and shards](CacheNodes.NodeGroups.md)
+ [Connecting to nodes](nodes-connecting.md)
+ [Supported node types](CacheNodes.SupportedTypes.md)
+ [Rebooting nodes \(cluster mode disabled only\)](nodes.rebooting.md)
+ [Replacing nodes](CacheNodes.NodeReplacement.md)
+ [ElastiCache reserved nodes](CacheNodes.Reserved.md)
+ [Migrating previous generation nodes](CacheNodes.NodeMigration.md)

Some important operations involving nodes are the following: 
+ [Adding nodes to a cluster](Clusters.AddNode.md)
+ [Removing nodes from a cluster](Clusters.DeleteNode.md)
+ [Scaling ElastiCache for Redis clusters](Scaling.md)
+ [Finding connection endpoints](Endpoints.md)