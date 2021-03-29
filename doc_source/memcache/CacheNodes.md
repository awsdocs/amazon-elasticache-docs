# Managing nodes<a name="CacheNodes"></a>

A node is the smallest building block of an Amazon ElastiCache deployment\. It is a fixed\-size chunk of secure, network\-attached RAM\. Each node runs the engine that was chosen when the cluster was created or last modified\. Each node has its own Domain Name Service \(DNS\) name and port\. Multiple types of ElastiCache nodes are supported, each with varying amounts of associated memory and computational power\.

For a more detailed discussion of which node size to use, see [Choosing your Memcached node size](nodes-select-size.md#CacheNodes.SelectSize)\. 

**Topics**
+ [Connecting to nodes](nodes-connecting.md)
+ [ElastiCache reserved nodes](CacheNodes.Reserved.md)
+ [Supported node types](CacheNodes.SupportedTypes.md)
+ [Replacing nodes](CacheNodes.NodeReplacement.md)

Some important operations involving nodes are the following: 
+ [Adding nodes to a cluster](Clusters.AddNode.md)
+ [Removing nodes from a cluster](Clusters.DeleteNode.md)
+ [Scaling ElastiCache for Memcached clusters](Scaling.md)
+ [Finding connection endpoints](Endpoints.md)
+ [Automatically Identify Nodes in your Memcached Cluster](AutoDiscovery.md)