# Rebooting nodes \(cluster mode disabled only\)<a name="nodes.rebooting"></a>

Some changes require that cluster nodes be rebooted for the changes to be applied\. For example, for some parameters, changing the parameter value in a parameter group is only applied after a reboot\.

For Redis \(cluster mode disabled\) clusters, those parameters are:
+ activerehashing
+ databases

You are able to reboot a node using only the ElastiCache console\. You can only reboot a single node at a time\. To reboot multiple nodes you must repeat the process for each node\.

**Redis \(Cluster Mode Enabled\) parameter changes**  
If you make changes to the following parameters on a Redis \(cluster mode enabled\) cluster, follow the ensuing steps\.  
activerehashing
databases
Create a manual backup of your cluster\. See [Making manual backups](backups-manual.md)\.
Delete the Redis \(cluster mode enabled\) cluster\. See [Deleting a cluster](Clusters.Delete.md)\.
Restore the cluster using the altered parameter group and backup to seed the new cluster\. See [Restoring from a backup with optional cluster resizing](backups-restoring.md)\.
Changes to other parameters do not require this\.

## Using the AWS Management Console<a name="nodes.rebooting.con"></a>

You can reboot a node using the ElastiCache console\.

**To reboot a node \(console\)**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the list in the upper\-right corner, choose the AWS Region that applies\.

1. In the left navigation pane, choose **Redis**\.

   A list of clusters running Redis appears\.

1. Choose the cluster under **Cluster Name**\.

1. Under **Node name**, choose the radio button next to the node you want to reboot\.

1. Choose **Actions**, and then choose **Reboot node**\.

To reboot multiple nodes, repeat steps 2 through 5 for each node that you want to reboot\. You do not need to wait for one node to finish rebooting to reboot another\.