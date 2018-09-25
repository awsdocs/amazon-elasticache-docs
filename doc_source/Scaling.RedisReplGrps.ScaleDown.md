# Scaling Down Redis Clusters with Replicas<a name="Scaling.RedisReplGrps.ScaleDown"></a>

The following sections walk you through how to scale a Redis \(cluster mode disabled\) cache cluster with replica nodes down to a smaller node type\. Ensuring that the new, smaller node type is large enough to accommodate all the data and overhead is very important to success\. For more information, see [Ensuring You Have Sufficient Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)\.

**Important**  
If your parameter group uses `reserved-memory` to set aside memory for Redis overhead, before you begin scaling be sure that you have a custom parameter group that reserves the correct amount of memory for your new node type\. Alternatively, you can modify a custom parameter group so that it uses `reserved-memory-percent` and use that parameter group for your new cluster\.  
If you're using `reserved-memory-percent`, doing this is not necessary\.   
For more information, see [Managing Reserved Memory](redis-memory-management.md)\.

**Topics**
+ [Scaling Down a Redis Cluster with Replicas \(Console\)](#Scaling.RedisReplGrps.ScaleDown.CON)
+ [Scaling Down a Redis Replication Group \(AWS CLI\)](#Scaling.RedisReplGrps.ScaleDown.CLI)
+ [Scaling Down a Redis Replication Group \(ElastiCache API\)](#Scaling.RedisReplGrps.ScaleDown.API)

## Scaling Down a Redis Cluster with Replicas \(Console\)<a name="Scaling.RedisReplGrps.ScaleDown.CON"></a>

The following process scales your Redis cluster with replica nodes to a smaller node type using the ElastiCache console\.

**To scale down a Redis cluster with replica nodes \(console\)**

1. Ensure that the smaller node type is adequate for your data and overhead needs\. For more information, see [Ensuring You Have Sufficient Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)\.

1. If your parameter group uses `reserved-memory` to set aside memory for Redis overhead, ensure that you have a custom parameter group to set aside the correct amount of memory for your new node type\.

   Alternatively, you can modify your custom parameter group to use `reserved-memory-percent`\. For more information, see [Managing Reserved Memory](redis-memory-management.md)\.

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. Take a snapshot of the cluster’s primary node\. For details on how to take a snapshot, see [Creating a Manual Backup \(Console\)](backups-manual.md#backups-manual-CON)\.

1. Restore from this snapshot specifying the new node type for the new cluster\. For more information, see [Restoring From a Backup \(Console\)](backups-restoring.md#backups-restoring-CON)\.

   Alternatively, you can launch a new cluster using the new node type and seeding it from the snapshot\. For more information, see [Seeding a New Cluster with an Externally Created Backup](backups-seeding-redis.md)\.

1. In your application, update the endpoints to the new cluster's endpoints\. For more information, see [Finding a Redis \(cluster mode disabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Redis)\.

1. Delete the old cluster\. For more information, see [Deleting a Replication Group \(Console\)](Replication.DeletingRepGroup.md#Replication.DeletingRepGroup.CON)\.

1. If you no longer need it, delete the snapshot\. For more information, see [Deleting a Backup \(Console\)](backups-deleting.md#backups-deleting-CON)\.

**Tip**  
If you don't mind being unable to use your replication group while it's being created or restored, you can eliminate the need to update the endpoints in your application\. To do so, delete the old cluster right after taking the snapshot and reuse the old cluster's name for the new cluster\.

## Scaling Down a Redis Replication Group \(AWS CLI\)<a name="Scaling.RedisReplGrps.ScaleDown.CLI"></a>

The following process scales your Redis replication group to a smaller node type using the AWS CLI\.

**To scale down a Redis replication group \(AWS CLI\)**

1. Ensure that the smaller node type is adequate for your data and overhead needs\. For more information, see [Choosing Your Node Size](nodes-select-size.md#CacheNodes.SelectSize)\.

1. If your parameter group uses `reserved-memory` to set aside memory for Redis overhead, ensure that you have a custom parameter group to set aside the correct amount of memory for your new node type\.

   Alternatively, you can modify your custom parameter group to use `reserved-memory-percent`\. For more information, see [Managing Reserved Memory](redis-memory-management.md)\.

1. Create a snapshot of your existing Redis node\. For instructions, see [Creating a Manual Backup \(AWS CLI\)](backups-manual.md#backups-manual-CLI)\.

1. Restore from the snapshot using the new, smaller node type as the new node type and, if needed, the new parameter group\. For more information, see [Restoring From a Backup \(AWS CLI\)](backups-restoring.md#backups-restoring-CLI)\.

1. In your application, update the endpoints to the new cache cluster's endpoints\. For more information, see [Finding the Endpoints for Replication Groups \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI.ReplGroups)\.

1. Delete your old replication group\. For more information, see [Deleting a Replication Group \(AWS CLI\)](Replication.DeletingRepGroup.md#Replication.DeletingRepGroup.CLI)\.

1. If you no longer need it, delete the snapshot\. For more information, see [Deleting a Backup \(AWS CLI\)](backups-deleting.md#backups-deleting-CLI)\.

**Tip**  
If you don't mind being unable to use your replication group while it's being created or restored, you can eliminate the need to update the endpoints in your application\. To do so, delete the old replication group right after taking the snapshot and reuse the old replication group's name for the new replication group\.

## Scaling Down a Redis Replication Group \(ElastiCache API\)<a name="Scaling.RedisReplGrps.ScaleDown.API"></a>

The following process scales your Redis replication group to a smaller node type using the ElastiCache API\.

**To scale down a Redis replication group \(ElastiCache API\)**

1. Ensure that the smaller node type is adequate for your data and overhead needs\. For more information, see [Choosing Your Node Size](nodes-select-size.md#CacheNodes.SelectSize)\.

1. If your parameter group uses `reserved-memory` to set aside memory for Redis overhead, ensure that you have a custom parameter group to set aside the correct amount of memory for your new node type\.

   Alternatively, you can modify your custom parameter group to use `reserved-memory-percent`\. For more information, see [Managing Reserved Memory](redis-memory-management.md)\.

1. Create a snapshot of your existing Redis cache cluster\. For instructions, see [Creating a Manual Backup \(ElastiCache API\)](backups-manual.md#backups-manual-API)\.

1. Restore from the snapshot using the new, smaller node type as the new node type and, if needed, the new parameter group\. For more information, see [Restoring From a Backup \(ElastiCache API\)](backups-restoring.md#backups-restoring-API)\.

1. In your application, update the endpoints to the new cache cluster's endpoints\. For more information, see [Finding Endpoints \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API)\.

1. Delete your old replication group\. For more information, see [Deleting a Replication Group \(ElastiCache API\)](Replication.DeletingRepGroup.md#Replication.DeletingRepGroup.API)\.

1. If you no longer need it, delete the snapshot\. For more information, see [Deleting a Backup \(ElastiCache API\)](backups-deleting.md#backups-deleting-API)\.

**Tip**  
If you don't mind being unable to use your replication group while it's being created or restored, you can eliminate the need to update the endpoints in your application\. To do so, delete the old replication group right after taking the snapshot and reuse the old replication group's name for the new replication group\.