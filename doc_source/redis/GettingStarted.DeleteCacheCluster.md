# Delete Your Cluster \(Avoid Unnecessary Charges\)<a name="GettingStarted.DeleteCacheCluster"></a>

**Important**  
It is almost always a good idea to delete clusters that you are not actively using\. Until a cluster's status is *deleted*, you continue to incur charges for it\.

Before you continue, complete at least as far as [Create a Cluster](GettingStarted.CreateCluster.md)\.

**To delete a cluster**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. To see a list of all your clusters running Redis, in the navigation pane, choose **Redis**\.

1. To select the cluster to delete, select the cluster's name from the list of clusters\.
**Tip**  
You can only delete one cluster at a time from the ElastiCache console\. Selecting multiple clusters disables the delete button\. To delete multiple clusters, repeat this process for each cluster\. You do not need to wait for one cluster to finish deleting before you delete another cluster\.

1. For **Actions**, choose **Delete**\.

1. In the **Delete Cluster** confirmation screen, choose **Delete** to delete the cluster, or **Cancel** to keep the cluster\.

   If you choose **Delete**, the status of the cluster changes to *deleting*\.

As soon as your cluster is no longer listed in the list of clusters, you stop incurring charges for it\.

Now you have successfully launched, authorized access to, connected to, viewed, and deleted an ElastiCache for Redis cluster\.