# Canceling Pending Add or Delete Node Operations<a name="Clusters.CancelPending"></a>

## Canceling Pending Add or Delete Node Operations \(Console\)<a name="Clusters.CancelPending.CON"></a>

 If you elected to not apply a change immediately, the operation has **pending** status until it is performed at your next maintenance window\. You can cancel any pending operation\.

**To cancel a pending operation**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the dropdown in the upper right corner, choose the region you want to cancel a pending add or delete node operation in\.

1. In the navigation pane, click **Memcached** or **Redis**\. A list of clusters running the chosen engine will appear\.

1. In the list of clusters, choose the name of the cluster that has pending operations you want to cancel\.

1. To determine what operations are pending, choose the **Description** tab and check to see how many pending creations or deletions are shown\. You cannot have both pending creations and pending deletions\.   
![\[Image: Cluster description tab\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ModifyCacheCluster-DescriptionTab-PendActions.png)

1. Choose the **Nodes** tab\.

1. To cancel all pending operations, click **Cancel Pending**\. The **Cancel Pending** dialog box appears\.

1. Confirm that you want to cancel all pending operations by choosing the **Cancel Pending** button, or to keep the operations, choose **Cancel**\.