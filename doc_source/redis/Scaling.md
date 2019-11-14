# Scaling ElastiCache for Redis Clusters<a name="Scaling"></a>

The amount of data your application needs to process is seldom static\. It increases and decreases as your business grows or experiences normal fluctuations in demand\. If you self\-manage your cache, you need to provision sufficient hardware for your demand peaks, which can be expensive\. By using Amazon ElastiCache you can scale to meet current demand, paying only for what you use\. ElastiCache enables you to scale your cache to match demand\.

The following helps you find the correct topic for the scaling actions you want to perform\.


**Scaling Redis Clusters**  

| Action | Redis \(cluster mode disabled\) | Redis \(cluster mode enabled\) | 
| --- | --- | --- | 
|  Scaling in  |  [Removing Nodes from a Cluster](Clusters.DeleteNode.md)  |  [Scaling Clusters in Redis \(Cluster Mode Enabled\)](scaling-redis-cluster-mode-enabled.md)  | 
|  Changing node types  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Scaling.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Scaling.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Scaling.html)  | 
|  Changing the number of node groups  |  Not supported for Redis \(cluster mode disabled\) clusters  |  [Scaling Clusters in Redis \(Cluster Mode Enabled\)](scaling-redis-cluster-mode-enabled.md)  | 

**Topics**
+ [Scaling Clusters for Redis \(Cluster Mode Disabled\)](scaling-redis-classic.md)
+ [Scaling Clusters in Redis \(Cluster Mode Enabled\)](scaling-redis-cluster-mode-enabled.md)