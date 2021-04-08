# Scaling ElastiCache for Redis clusters<a name="Scaling"></a>

The amount of data your application needs to process is seldom static\. It increases and decreases as your business grows or experiences normal fluctuations in demand\. If you self\-manage your cache, you need to provision sufficient hardware for your demand peaks, which can be expensive\. By using Amazon ElastiCache you can scale to meet current demand, paying only for what you use\. ElastiCache enables you to scale your cache to match demand\.

The following helps you find the correct topic for the scaling actions that you want to perform\.


**Scaling Redis clusters**  

| Action | Redis \(cluster mode disabled\) | Redis \(cluster mode enabled\) | 
| --- | --- | --- | 
|  Scaling in  |  [Removing nodes from a cluster](Clusters.DeleteNode.md)  |  [Scaling clusters in Redis \(Cluster Mode Enabled\)](scaling-redis-cluster-mode-enabled.md)  | 
|  Scaling out  |  [Adding nodes to a cluster](Clusters.AddNode.md)  |  [Online resharding and shard rebalancing for Redis \(cluster mode enabled\)](redis-cluster-resharding-online.md)  | 
|  Changing node types  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Scaling.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Scaling.html)  |  [Online vertical scaling by modifying node type](redis-cluster-vertical-scaling.md)  | 
|  Changing the number of node groups  |  Not supported for Redis \(cluster mode disabled\) clusters  |  [Scaling clusters in Redis \(Cluster Mode Enabled\)](scaling-redis-cluster-mode-enabled.md)  | 

**Topics**
+ [Scaling clusters for Redis \(Cluster Mode Disabled\)](scaling-redis-classic.md)
+ [Scaling clusters in Redis \(Cluster Mode Enabled\)](scaling-redis-cluster-mode-enabled.md)