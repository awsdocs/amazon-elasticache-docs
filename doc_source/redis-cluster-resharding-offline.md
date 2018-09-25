# Offline Resharding and Shard Rebalancing for Redis \(cluster mode enabled\)<a name="redis-cluster-resharding-offline"></a>

The main advantage you get from offline shard reconfiguration is that you can do more than merely add or remove shards from your replication group\. When you reshard offline, in addition to changing the number of shards in your replication group, you can do the following:
+ Change the node type of your replication group\.
+ Specify the Availability Zone for each node in the replication group\.
+ Upgrade to a newer engine version\.
+ Specify the number of replica nodes in each shard independently\.
+ Specify the keyspace for each shard\.

The main disadvantage of offline shard reconfiguration is that your cluster is offline beginning with the restore portion of the process and continuing until you update the endpoints in your application\. The length of time that your cluster is offline varies with the amount of data in your cluster\.

**To reconfigure your shards Redis \(cluster mode enabled\) cluster offline**

1. Create a manual backup of your existing Redis cluster\. For more information, see [Making Manual Backups](backups-manual.md)\.

1. Create a new cluster by restoring from the backup\. For more information, see [Restoring From a Backup with Optional Cluster Resizing](backups-restoring.md)\.

1. Update the endpoints in your application to the new cluster's endpoints\. For more information, see [Finding Connection Endpoints](Endpoints.md)\.