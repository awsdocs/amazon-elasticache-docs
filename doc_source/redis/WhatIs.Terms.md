# ElastiCache for Redis terminology<a name="WhatIs.Terms"></a>

In October 2016, Amazon ElastiCache launched support for Redis 3\.2\. At that point, we added support for partitioning your data across up to 250 shards \(called node groups in the ElastiCache API and AWS CLI\)\. To preserve compatibility with previous versions, we extended API version 2015\-02\-02 operations to include the new Redis functionality\. 

At the same time, we began using terminology in the ElastiCache console that is used in this new functionality and common across the industry\. These changes mean that at some points, the terminology used in the API and CLI might be different from the terminology used in the console\. The following list identifies terms that might differ between the API and CLI and the console\.

**Cache cluster or node vs\. node**  
There is a one\-to\-one relationship between a node and a cache cluster when there are no replica nodes\. Thus, the ElastiCache console often used the terms interchangeably\. The console now uses the term *node* throughout\. The one exception is the **Create Cluster** button, which launches the process to create a cluster with or without replica nodes\.  
The ElastiCache API and AWS CLI continue to use the terms as they have in the past\.

**Cluster vs\. replication group**  
The console now uses the term *cluster* for all ElastiCache for Redis clusters\. The console uses the term cluster in all these circumstances:   
+ When the cluster is a single node Redis cluster\.
+ When the cluster is a Redis \(cluster mode disabled\) cluster that supports replication within a single shard \(in the API and CLI, called a *node group*\)\.
+ When the cluster is a Redis \(cluster mode enabled\) cluster that supports replication within 1–250 shards or up to 500 with a limit increase request\. To request a limit increase, see [AWS service limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) and choose the limit type **Nodes per cluster per instance type**\. 
The following diagram illustrates the various topologies of ElastiCache for Redis clusters from the console's perspective\.  

![\[Image: ElastiCache for Redis clusters (Console view)\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-Clusters-ConsoleView.png)
The ElastiCache API and AWS CLI operations still distinguish single node ElastiCache for Redis clusters from multi\-node replication groups\. The following diagram illustrates the various ElastiCache for Redis topologies from the ElastiCache API and AWS CLI perspective\.  

![\[Image: ElastiCache for Redis cluster and replication groups (API and CLI view)\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-Clusters-APIView.png)