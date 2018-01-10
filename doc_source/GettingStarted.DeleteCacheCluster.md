# Step 6: Delete Your Cluster \[Avoid Unnecessary Charges\]<a name="GettingStarted.DeleteCacheCluster"></a>

Before you continue, be sure you have completed at least as far as [Step 2: Launch a Cluster](GettingStarted.CreateCluster.md)\.

**Important**  
It is almost always a good idea to delete clusters that you are not using\. Until a cluster's status is *deleted* you continue to incur charges for it\.

**To delete a cluster**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the ElastiCache console dashboard, select the engine the cluster you want to delete is running, either Memcached or Redis\.

   A list of all clusters running the selected engine appears\.

1. To select the cluster to delete, select the cluster's name from the list of clusters\.
**Important**  
You can only delete one cluster at a time from the ElastiCache console\. Selecting multiple clusters disables the delete operation\.

1. Select the **Actions** button and then select **Delete** from the list of actions\.

1. In the **Delete Cluster** confirmation screen:

   1. If this is a Redis cluster, specify whether or not a final snapshot should be taken, and, if you want a final snapshot, the name of the snapshot\.

   1. Choose **Delete** to delete the cluster, or select **Cancel** to keep the cluster\.

   If you chose **Delete**, the status of the cluster changes to *deleting*\.

As soon as your cluster is no longer listed in the list of clusters, you stop incurring charges for it\.

Congratulations\! You have successfully launched, authorized access to, connected to, viewed, and deleted a Redis cluster\.