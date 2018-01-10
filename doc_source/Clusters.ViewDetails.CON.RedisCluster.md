# Viewing a Redis \(cluster mode enabled\) Cluster's Details \(Console\)<a name="Clusters.ViewDetails.CON.RedisCluster"></a>

You can view the details of a Redis \(cluster mode enabled\) cluster using the ElastiCache console, the AWS CLI for ElastiCache, or the ElastiCache API\.

The following procedure details how to view the details of a Redis \(cluster mode enabled\) cluster using the ElastiCache console\.

**To view a Redis \(cluster mode enabled\) cluster's details**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the dropdown in the upper right corner, choose the region you are interested in\.

1. In the ElastiCache console dashboard, choose **Redis** to display a list of all your clusters that are running any version of Redis\.

1. To see details of a Redis \(cluster mode enabled\) cluster, choose the box to the left of the cluster's name\. Make sure you choose a cluster running the Clustered Redis engine, not just Redis\.

   The screen expands below the cluster and display details about the cluster, including the cluster's configuration endpoint\.

1. To see a listing of the cluster's shards and the number of nodes in each shard, choose the cluster's name\.

1. To view specific information on a node:

   1. Choose the shard's ID\.

   1. Choose the **Nodes** tab\.

      This will display information about each node, including each node's endpoint that you need to use to read data from the cluster\.

   1. To view metrics on one or more nodes, choose the box to the left of the node's id, and then choose the time range for the metrics from the **Time range** list\. Selecting multiple nodes will generate overlay graphs\.  
![\[Image: Metrics over the last hour for two Redis nodes\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-RedisCluster-Metrics.png)

      *Metrics over the last hour for two Redis nodes*