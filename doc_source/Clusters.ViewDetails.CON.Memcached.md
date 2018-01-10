# Viewing a Cluster's Details: Memcached \(Console\)<a name="Clusters.ViewDetails.CON.Memcached"></a>

You can view the details of a Memcached cluster using the ElastiCache console, the AWS CLI for ElastiCache, or the ElastiCache API\.

The following procedure details how to view the details of a Memcached cluster using the ElastiCache console\.

**To view a Memcached cluster's details**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the dropdown in the upper right corner, choose the region you are interested in\.

1. In the ElastiCache console dashboard, choose **Memcached**\. This will display a list of all your clusters that are running any version of Memcached\.

1. To see details of a cluster, choose the box to the left of the cluster's name\.

1. To view node information:

   1. Choose the cluster's name\.

   1. Choose the **Nodes** tab\.

   1. To view metrics on one or more nodes, choose the box to the left of the Node ID, and then choose the time range for the metrics from the **Time range** list\. Selecting multiple nodes will generate overlay graphs\.  
![\[Image: Metrics over the last hour for two Memcached nodes\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-Memcached-Metrics.png)

      *Metrics over the last hour for two Memcached nodes*