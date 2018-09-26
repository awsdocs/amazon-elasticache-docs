# Scaling Redis \(cluster mode enabled\) Clusters<a name="scaling-redis-cluster-mode-enabled"></a>

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

  You can monitor your cluster's use to determine whether or not you can safely scale in using the following metrics: *FreeableMemory*, *SwapUseage*, *BytesUseForCache*, *CPUUtilization*, *NetworkBytesIn*, *NetworkBytesOut*, *CurrConnections*, and *NewConnections*\.

**Performance Impact of Scaling**  
When you scale using the offline process, your cluster is offline for a significant portion of the process and thus unable to serve requests\. When you scale using the online method, because scaling is a compute\-intensive operation, there is some degradation in performance, nevertheless, your cluster continues to serve requests throughout the scaling operation\. How much degradation you experience depends upon your normal CPU utilization and your data\.

There are two ways to scale your Redis \(cluster mode enabled\) cluster; offline and online\. Whichever you choose, you can do the following:
+ Change the number of node groups \(shards\) in the replication group by adding or removing node groups\.
+ Configure the slots in your new cluster differently than they were in the old cluster\. Offline method only\.
+ Change the node type of the cluster's nodes\. If you are changing to a smaller node type, be sure that the new node size has sufficient memory for your data and Redis overhead\. Offline method only\.

  For more information, see [Choosing Your Node Size](nodes-select-size.md#CacheNodes.SelectSize)\.

**Contents**
+ [Offline Resharding and Shard Rebalancing for Redis \(cluster mode enabled\)](redis-cluster-resharding-offline.md)
+ [Online Resharding and Shard Rebalancing for Redis \(cluster mode enabled\)](redis-cluster-resharding-online.md)
  + [Adding Shards with Online Resharding](redis-cluster-resharding-online.md#redis-cluster-resharding-online-add)
    + [Adding Shards \(Console\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-add-console)
    + [Adding Shards \(AWS CLI\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-add-cli)
    + [Adding Shards \(ElastiCache API\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-add-api)
  + [Removing Shards with Online Resharding](redis-cluster-resharding-online.md#redis-cluster-resharding-online-remove)
    + [Removing Shards \(Console\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-remove-console)
    + [Removing Shards \(AWS CLI\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-remove-cli)
    + [Removing Shards \(ElastiCache API\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-remove-api)
  + [Online Shard Rebalancing](redis-cluster-resharding-online.md#redis-cluster-resharding-online-rebalance)
    + [Online Shard Rebalancing \(Console\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-rebalance-console)
    + [Online Shard Rebalancing \(AWS CLI\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-rebalance-cli)
    + [Online Shard Rebalancing \(ElastiCache API\)](redis-cluster-resharding-online.md#redis-cluster-resharding-online-rebalance-api)