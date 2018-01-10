# Scheduling Automatic Backups<a name="backups-automatic"></a>

For any Redis cluster, you can enable *automatic* backups\. When automatic backups are enabled, ElastiCache creates a backup of the cluster on a daily basis\. Automatic backups can help guard against data loss\. In the event of a failure, you can create a new cluster, restoring your data from the most recent backup\. The result is a warm\-started cluster, pre\-loaded with your data and ready for use\. For more information, go to [Restoring From a Backup with Optional Cluster Resizing](backups-restoring.md)\.

When you schedule automatic backups, you should plan the following settings:

+ **Backup window** – A period during each day when ElastiCache will begin creating a backup\. The minimum length for the backup window is 60 minutes\. You can set the backup window for any time when it's most convenient for you, or for a time of day that avoids doing backups during particularly high\-utilization periods\.

  If you do not specify a backup window, ElastiCache will assign one automatically\.

   

+ **Backup retention limit** – The number of days the backup will be retained in Amazon S3\. For example, if you set the retention limit to 5, then a backup taken today would be retained for 5 days\. When the retention limit expires, the backup is automatically deleted\.

  The maximum backup retention limit is 35 days\. If the backup retention limit is set to 0, automatic backups are disabled for the cluster\.

You can enable or disable automatic backups on an existing Redis cluster or replication group by modifying it using the ElastiCache console, the AWS CLI, or the ElastiCache API\. For more information on how to enable or disable automatic backups on an existing cluster or replication group, go to [Modifying an ElastiCache Cluster](Clusters.Modify.md) or [Modifying a Cluster with Replicas](Replication.Modify.md)\.

You can enable or disable automatic backups when creating a Redis cluster or replication group using the ElastiCache console, the AWS CLI, or the ElastiCache API\. You can enable automatic backups when you create a Redis cluster by checking the **Enable Automatic Backups** box in the **Advanced Redis Settings** section\. For more information, see step 2 of [Creating a Redis \(cluster mode disabled\) Cluster \(Console\)](Clusters.Create.CON.Redis.md)\. You can enable automatic backups when you create a Redis replication group if you are not using an existing cluster as the primary cluster\. For more information, see [Creating a Redis Cluster with Replicas from Scratch](Replication.CreatingReplGroup.NoExistingCluster.md)\.