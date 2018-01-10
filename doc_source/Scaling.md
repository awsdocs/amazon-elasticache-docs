# Scaling<a name="Scaling"></a>

The amount of data your application needs to process is seldom static\. It increases and decreases as your business grows or experiences normal fluctuations in demand\. If you self\-manage your cache, you need to provision sufficient hardware for your demand peaks, which can be expensive\. By using Amazon ElastiCache you can scale to meet current demand, paying only for what you use\. ElastiCache enables you to scale your cache to match demand\.

The following helps you find the correct topic for the scaling actions you want to perform\.


**Scaling Memcached Clusters**  

| Action | Topic/Link | 
| --- | --- | 
|  Scaling out  |  [Adding Nodes to a Cluster](Clusters.AddNode.md)  | 
|  Scaling in  |  [Removing Nodes from a Cluster](Clusters.DeleteNode.md)  | 
|  Changing node types  |  [Scaling Memcached Vertically](Scaling.Memcached.md#Scaling.Memcached.Vertically)  | 


**Scaling Redis Clusters**  

| Action | Redis \(cluster mode disabled\) | Redis \(cluster mode enabled\) | 
| --- | --- | --- | 
|  Scaling in  |  [Removing Nodes from a Cluster](Clusters.DeleteNode.md)  |   | 
|  Changing node types  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/Scaling.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/Scaling.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/Scaling.html)  | 
|  Changing the number of node groups  |  Not supported for Redis \(cluster mode disabled\) clusters  |  [Offline Resharding and Cluster Reconfiguration for ElastiCache for Redis—Redis \(cluster mode enabled\)](scaling-redis-cluster-mode-enabled.md#redis-cluster-resharding-offline) [Online Resharding and Shard Rebalancing for ElastiCache for Redis—Redis \(cluster mode enabled\)](scaling-redis-cluster-mode-enabled.md#redis-cluster-resharding-online)  | 


+ [Scaling Memcached](Scaling.Memcached.md)
+ [Scaling Single\-Node Redis \(cluster mode disabled\) Clusters](Scaling.RedisStandalone.md)
+ [Scaling Redis \(cluster mode disabled\) Clusters with Replica Nodes](Scaling.RedisReplGrps.md)
+ [Scaling for Amazon ElastiCache for Redis—Redis \(cluster mode enabled\)](scaling-redis-cluster-mode-enabled.md)