# Managing Nodes<a name="CacheNodes"></a>

A node is the smallest building block of an Amazon ElastiCache deployment\. It is a fixed\-size chunk of secure, network\-attached RAM\. Each node runs the engine that was chosen when the cluster was created or last modified\. Each node has its own Domain Name Service \(DNS\) name and port\. Multiple types of ElastiCache nodes are supported, each with varying amounts of associated memory and computational power\.

For a more detailed discussion of which node size to use, see [Choosing Your Memcached Node Size](nodes-select-size.md#CacheNodes.SelectSize)\. 

**Topics**
+ [Connecting to Nodes](nodes-connecting.md)
+ [ElastiCache Reserved Nodes](CacheNodes.Reserved.md)
+ [Supported Node Types](CacheNodes.SupportedTypes.md)
+ [Replacing Nodes](CacheNodes.NodeReplacement.md)

Some important operations involving nodes are the following: 
+ [Adding Nodes to a Cluster](Clusters.AddNode.md)
+ [Removing Nodes from a Cluster](Clusters.DeleteNode.md)
+ [Scaling ElastiCache for Memcached Clusters](Scaling.md)
+ [Finding Connection Endpoints](Endpoints.md)
+ [Automatically Identify Nodes in your Memcached Cluster](AutoDiscovery.md)