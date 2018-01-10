# Step 3: \(Optional\) View Cluster Details<a name="GettingStarted.ViewClusterDetails"></a>

Before you continue, make sure you have completed [Step 2: Launch a Cluster](GettingStarted.CreateCluster.md)\.

**To view a Redis \(cluster mode disabled\) cluster's details**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the ElastiCache console dashboard, choose **Redis** to display a list of all your clusters that are running any version of Redis\.  
![\[Image: Memcached cluster list\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-ViewDetails-Memcached-ClusterList.png)

1. To see details of a cluster, select the check box to the left of the cluster's name\. Make sure you select a cluster running the Redis engine, not Clustered Redis\. Doing this displays details about the cluster, including the cluster's primary endpoint\.

1. To view node information:

   1. Choose the cluster's name\.

   1. Choose the **Nodes** tab\. Doing this displays details about each node, including the node's endpoint which you need to use to read from the cluster\.

   1. To view metrics on one or more nodes, select the box to the left of the node ID, then select the time range for the metrics from the **Time range** list\. If you select multiple nodes, you can see overlay graphs\.  
![\[Image: Metrics over the last hour for two Redis nodes\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-Redis-Metrics.png)

      Metrics over the last hour for two Redis nodes