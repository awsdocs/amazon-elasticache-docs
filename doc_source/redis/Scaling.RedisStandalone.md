# Scaling single\-node clusters for Redis \(Cluster Mode Disabled\)<a name="Scaling.RedisStandalone"></a>

Redis \(cluster mode disabled\) nodes must be large enough to contain all the cache's data plus Redis overhead\. To change the data capacity of your Redis \(cluster mode disabled\) cluster, you must scale vertically; scaling up to a larger node type to increase data capacity, or scaling down to a smaller node type to reduce data capacity\.

The ElastiCache for Redis scaling up process is designed to make a best effort to retain your existing data and requires successful Redis replication\. For Redis \(cluster mode disabled\) clusters, we recommend that sufficient memory be made available to Redis\. 

You cannot partition your data across multiple Redis \(cluster mode disabled\) clusters\. However, if you only need to increase or decrease your cluster's read capacity, you can create a Redis \(cluster mode disabled\) cluster with replica nodes and add or remove read replicas\. To create a Redis \(cluster mode disabled\) cluster with replica nodes using your single\-node Redis cache cluster as the primary cluster, see [Creating a Redis \(cluster mode disabled\) cluster \(Console\)](GettingStarted.CreateCluster.md#Clusters.Create.CON.Redis-gs)\.

After you create the cluster with replicas, you can increase read capacity by adding read replicas\. Later, if you need to, you can reduce read capacity by removing read replicas\. For more information, see [Increasing read capacity](Scaling.RedisReplGrps.ScaleOut.md) or [Decreasing read capacity](Scaling.RedisReplGrps.ScaleIn.md)\.

In addition to being able to scale read capacity, Redis \(cluster mode disabled\) clusters with replicas provide other business advantages\. For more information, see [High availability using replication groups](Replication.md)\.

**Important**  
If your parameter group uses `reserved-memory` to set aside memory for Redis overhead, before you begin scaling be sure that you have a custom parameter group that reserves the correct amount of memory for your new node type\. Alternatively, you can modify a custom parameter group so that it uses `reserved-memory-percent` and use that parameter group for your new cluster\.  
If you're using `reserved-memory-percent`, doing this is not necessary\.   
For more information, see [Managing Reserved Memory](redis-memory-management.md)\.

**Topics**
+ [Scaling up single\-node clusters for Redis \(Cluster Mode Disabled\)](Scaling.RedisStandalone.ScaleUp.md)
+ [Scaling down single\-node Redis clusters](Scaling.RedisStandalone.ScaleDown.md)