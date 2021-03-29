# Scaling clusters in Redis \(Cluster Mode Enabled\)<a name="scaling-redis-cluster-mode-enabled"></a>

As demand on your clusters changes, you might decide to improve performance or reduce costs by changing the number of shards in your Redis \(cluster mode enabled\) cluster\. We recommend using online horizontal scaling to do so, because it allows your cluster to continue serving requests during the scaling process\.

Conditions under which you might decide to rescale your cluster include the following:
+ **Memory pressure:**

  If the nodes in your cluster are under memory pressure, you might decide to scale out so that you have more resources to better store data and serve requests\.

  You can determine whether your nodes are under memory pressure by monitoring the following metrics: *FreeableMemory*, *SwapUsage*, and *BytesUseForCache*\.
+ **CPU or network bottleneck:**

  If latency/throughput issues are plaguing your cluster, you might need to scale out to resolve the issues\.

  You can monitor your latency and throughput levels by monitoring the following metrics: *CPUUtilization*, *NetworkBytesIn*, *NetworkBytesOut*, *CurrConnections*, and *NewConnections*\.
+ **Your cluster is over\-scaled:**

  Current demand on your cluster is such that scaling in doesn't hurt performance and reduces your costs\.

  You can monitor your cluster's use to determine whether or not you can safely scale in using the following metrics: *FreeableMemory*, *SwapUsage*, *BytesUseForCache*, *CPUUtilization*, *NetworkBytesIn*, *NetworkBytesOut*, *CurrConnections*, and *NewConnections*\.

**Performance Impact of Scaling**  
When you scale using the offline process, your cluster is offline for a significant portion of the process and thus unable to serve requests\. When you scale using the online method, because scaling is a compute\-intensive operation, there is some degradation in performance, nevertheless, your cluster continues to serve requests throughout the scaling operation\. How much degradation you experience depends upon your normal CPU utilization and your data\.

There are two ways to scale your Redis \(cluster mode enabled\) cluster; horizontal and vertical scaling\.
+ Horizontal scaling allows you to change the number of node groups \(shards\) in the replication group by adding or removing node groups \(shards\)\. The online resharding process allows scaling in/out while the cluster continues serving incoming requests\. 

  Configure the slots in your new cluster differently than they were in the old cluster\. Offline method only\.
+ Vertical Scaling \- Change the node type to resize the cluster\. The online vertical scaling allows scaling up/down while the cluster continues serving incoming requests\.

If you are reducing the size and memory capacity of the cluster, by either scaling in or scaling down, ensure that the new configuration has sufficient memory for your data and Redis overhead\. 

For more information, see [Choosing your node size](nodes-select-size.md#CacheNodes.SelectSize)\.

**Contents**
+ [Offline resharding and shard rebalancing for Redis \(cluster mode enabled\)](redis-cluster-resharding-offline.md)
+ [Online resharding and shard rebalancing for Redis \(cluster mode enabled\)](redis-cluster-resharding-online.md)
  + [Adding shards with online resharding](redis-cluster-resharding-online.md#redis-cluster-resharding-online-add)
    + [Adding shards \(Console\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-add-console)
    + [Adding shards \(AWS CLI\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-add-cli)
    + [Adding shards \(ElastiCache API\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-add-api)
  + [Removing shards with online resharding](redis-cluster-resharding-online.md#redis-cluster-resharding-online-remove)
    + [Removing shards \(Console\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-remove-console)
    + [Removing shards \(AWS CLI\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-remove-cli)
    + [Removing shards \(ElastiCache API\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-remove-api)
  + [Online shard rebalancing](redis-cluster-resharding-online.md#redis-cluster-resharding-online-rebalance)
    + [Online Shard Rebalancing \(Console\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-rebalance-console)
    + [Online shard rebalancing \(AWS CLI\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-rebalance-cli)
    + [Online shard rebalancing \(ElastiCache API\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-rebalance-api)
+ [Online vertical scaling by modifying node type](redis-cluster-vertical-scaling.md)
  + [Online scaling up](redis-cluster-vertical-scaling-scaling-up.md)
    + [Scaling up Redis cache clusters \(Console\)](redis-cluster-vertical-scaling-scaling-up.md#redis-cluster-vertical-scaling-console)
    + [Scaling up Redis cache clusters \(AWS CLI\)](redis-cluster-vertical-scaling-scaling-up.md#Scaling.RedisStandalone.ScaleUp.CLI)
    + [Scaling up Redis cache clusters \(ElastiCache API\)](redis-cluster-vertical-scaling-scaling-up.md#VeticalScaling.RedisReplGrps.ScaleUp.API)
  + [Online scaling down](redis-cluster-vertical-scaling-scaling-down.md)
    + [Scaling down Redis cache clusters \(Console\)](redis-cluster-vertical-scaling-scaling-down.md#redis-cluster-vertical-scaling-down-console)
    + [Scaling down Redis cache clusters \(AWS CLI\)](redis-cluster-vertical-scaling-scaling-down.md#Scaling.RedisStandalone.ScaleDown.CLI)
    + [Scaling down Redis cache clusters \(ElastiCache API\)](redis-cluster-vertical-scaling-scaling-down.md#Scaling.Vertical.ScaleDown.API)