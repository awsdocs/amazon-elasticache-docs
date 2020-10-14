# Managing Your ElastiCache Clusters<a name="Clusters"></a>

A *cluster* is a collection of one or more cache nodes, all of which run an instance of the Redis cache engine software\. When you create a cluster, you specify the engine and version for all of the nodes to use\.

The following diagram illustrates a typical Redis cluster\. Redis clusters can contain a single node or up to six nodes inside a shard \(API/CLI: node group\), A single\-node Redis \(cluster mode disabled\) cluster has no shard, and a multi\-node Redis \(cluster mode disabled\) cluster has a single shard\. Redis \(cluster mode enabled\) clusters can have up to 90 shards, with your data partitioned across the shards\. When you have multiple nodes in a shard, one of the nodes is a read/write primary node\. All other nodes in the shard are read\-only replicas\.

Typical Redis clusters look as follows\.

![\[Image: Typical Redis Clusters\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-Cluster-Redis.png)

Most ElastiCache operations are performed at the cluster level\. You can set up a cluster with a specific number of nodes and a parameter group that controls the properties for each node\. All nodes within a cluster are designed to be of the same node type and have the same parameter and security group settings\. 

Every cluster must have a cluster identifier\. The cluster identifier is a customer\-supplied name for the cluster\. This identifier specifies a particular cluster when interacting with the ElastiCache API and AWS CLI commands\. The cluster identifier must be unique for that customer in an AWS Region\.

ElastiCache supports multiple engine versions\. Unless you have specific reasons, we recommend using the latest version\.

ElastiCache clusters are designed to be accessed using an Amazon EC2 instance\. If you launch your cluster in a virtual private cloud \(VPC\) based on the Amazon VPC service, you can access it from outside AWS\. For more information, see [Accessing ElastiCache Resources from Outside AWS](accessing-elasticache.md#access-from-outside-aws)\.

For a list of supported Redis versions, see [Supported ElastiCache for Redis Versions](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/supported-engine-versions.html)\.