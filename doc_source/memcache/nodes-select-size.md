# Choosing Your Node Size<a name="nodes-select-size"></a>

The node size you select for your cluster impacts costs, performance, and fault tolerance\. 

## Choosing Your Memcached Node Size<a name="CacheNodes.SelectSize"></a>

Memcached clusters contain one or more nodes with the cluster's data partitioned across the nodes\. Because of this, the memory needs of the cluster and the memory of a node are related, but not the same\. You can attain your needed cluster memory capacity by having a few large nodes or several smaller nodes\. Further, as your needs change, you can add nodes to or remove nodes from the cluster and thus pay only for what you need\.

The total memory capacity of your cluster is calculated by multiplying the number of nodes in the cluster by the RAM capacity of each node after deducting system overhead\. The capacity of each node is based on the node type\.

```
cluster_capacity = number_of_nodes * (node_capacity - system_overhead)
```

The number of nodes in the cluster is a key factor in the availability of your cluster running Memcached\. The failure of a single node can have an impact on the availability of your application and the load on your back\-end database while ElastiCache provisions a replacement for the failed node and it gets repopulated\. You can reduce this potential availability impact by spreading your memory and compute capacity over a larger number of nodes, each with smaller capacity, rather than using a fewer number of high capacity nodes\.

In a scenario where you want to have 40 GB of cache memory, you can set it up in any of the following configurations:
+ 13 `cache.t2.medium` nodes with 3\.22 GB of memory and 2 threads each = 41\.86 GB and 26 threads\.
+ 7 `cache.m3.large` nodes with 6\.05 GB of memory and 2 threads each = 42\.35 GB and 14 threads\.
+ 7 `cache.m4.large` nodes with 6\.42 GB of memory and 2 threads each = 44\.94 GB and 14 threads\.
+ 3 `cache.r3.large` nodes with 13\.50 GB of memory and 2 threads each = 40\.50 GB and 6 threads\.
+ 3 `cache.m4.xlarge` nodes with 14\.28 GB of memory and 4 threads each = 42\.84 GB and 12 threads\.


**Comparing node options**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/nodes-select-size.html)

These options each provide similar memory capacity but different computational capacity and cost\. To compare the costs of your specific options, see [Amazon ElastiCache Pricing](https://aws.amazon.com/elasticache/pricing/)\.

For clusters running Memcached, some of the available memory on each node is used for connection overhead\. For more information, see [Memcached Connection Overhead](ParameterGroups.Memcached.md#ParameterGroups.Memcached.Overhead)

Using multiple nodes will require spreading the keys across them\. Each node has its own endpoint\. For easy endpoint management, you can use the ElastiCache the Auto Discovery feature, which enables client programs to automatically identify all of the nodes in a cluster\. For more information, see [Automatically Identify Nodes in your Memcached Cluster](AutoDiscovery.md)\.

If you're unsure about how much capacity you need, for testing we recommend starting with one `cache.m3.medium` node and monitoring the memory usage, CPU utilization, and cache hit rate with the ElastiCache metrics that are published to CloudWatch\. For more information on CloudWatch metrics for ElastiCache, see [Monitoring Use with CloudWatch Metrics](CacheMetrics.md)\. For production and larger workloads, the R3 nodes provide the best performance and RAM cost value\.

If your cluster does not have the desired hit rate, you can easily add more nodes, thereby increasing the total available memory in your cluster\.

If your cluster turns out to be bound by CPU but it has sufficient hit rate, try setting up a new cluster with a node type that provides more compute power\.