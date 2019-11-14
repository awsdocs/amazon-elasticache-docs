# Scaling Down Single\-Node Redis Clusters<a name="Scaling.RedisStandalone.ScaleDown"></a>

The ElastiCache process for scaling your Redis cluster down is completely manual and makes no attempt at data retention other than what you do\.

The following sections walk you through how to scale a single\-node Redis cluster down to a smaller node type\. Ensuring that the new, smaller node type is large enough to accommodate all the data and Redis overhead is important to the long\-term success of your new Redis cluster\. For more information, see [Ensuring That You Have Enough Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)\.

**Topics**
+ [Scaling Down a Single\-Node Redis Cluster \(Console\)](#Scaling.RedisStandalone.ScaleDown.CON)
+ [Scaling Down a Single\-Node Redis Cache Cluster \(AWS CLI\)](#Scaling.RedisStandalone.ScaleDown.CLI)
+ [Scaling Down a Single\-Node Cache Cluster for Redis \(Cluster Mode Disabled\) \(ElastiCache API\)](#Scaling.RedisStandalone.ScaleDown.API)

## Scaling Down a Single\-Node Redis Cluster \(Console\)<a name="Scaling.RedisStandalone.ScaleDown.CON"></a>

The following procedure walks you through scaling your single\-node Redis cluster down to a smaller node type using the ElastiCache console\.

**Important**  
If your parameter group uses `reserved-memory` to set aside memory for Redis overhead, before you begin scaling be sure that you have a custom parameter group that reserves the correct amount of memory for your new node type\. Alternatively, you can modify a custom parameter group so that it uses `reserved-memory-percent` and use that parameter group for your new cluster\.  
If you're using `reserved-memory-percent`, doing this is not necessary\.   
For more information, see [Managing Reserved Memory](redis-memory-management.md)\.

**To scale down your single\-node Redis cluster \(console\)**

1. Ensure that the smaller node type is adequate for your data and overhead needs\. For more information, see [Ensuring That You Have Enough Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)\.

1. If your parameter group uses `reserved-memory` to set aside memory for Redis overhead, ensure that you have a custom parameter group to set aside the correct amount of memory for your new node type\.

   Alternatively, you can modify your custom parameter group to use `reserved-memory-percent`\. For more information, see [Managing Reserved Memory](redis-memory-management.md)\.

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. Take a snapshot of the cluster\. For details on how to take a snapshot, see [Creating a Manual Backup \(Console\)](backups-manual.md#backups-manual-CON)\.

1. Restore from this snapshot specifying the new node type for the new cluster and, if necessary, a parameter group to reserve the correct amount of memory\. For more information, see [Restoring From a Backup \(Console\)](backups-restoring.md#backups-restoring-CON)\.

   Alternatively, you can launch a new cluster using the new node type and parameter group, and seeding it from the snapshot\. For more information, see [Seeding a New Cluster with an Externally Created Backup](backups-seeding-redis.md)\.

1. In your application, update the endpoints to the new cluster's endpoints\. For more information, see [Finding a Redis \(Cluster Mode Disabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Redis)\.

1. Delete the old cluster\. For more information, see [Using the AWS Management Console](Clusters.Delete.md#Clusters.Delete.CON)\.

1. If you no longer need it, delete the snapshot\. For more information, see [Deleting a Backup \(Console\)](backups-deleting.md#backups-deleting-CON)\.

**Tip**  
If you don't mind your cluster being unavailable while it's being created or restored, you can eliminate the need to update the endpoints in your application\. To do so, delete the old cluster right after taking the snapshot and reuse the old cluster's name for the new cluster\.

## Scaling Down a Single\-Node Redis Cache Cluster \(AWS CLI\)<a name="Scaling.RedisStandalone.ScaleDown.CLI"></a>

The following procedure walks you through scaling your single\-node Redis cache cluster down to a smaller node type using the AWS CLI\.

**To scale down a single\-node Redis cache cluster \(AWS CLI\)**

1. Ensure that the smaller node type is adequate for your data and overhead needs\. For more information, see [Ensuring That You Have Enough Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)\.

1. If your parameter group uses `reserved-memory` to set aside memory for Redis overhead, ensure that you have a custom parameter group to set aside the correct amount of memory for your new node type\.

   Alternatively, you can modify your custom parameter group to use `reserved-memory-percent`\. For more information, see [Managing Reserved Memory](redis-memory-management.md)\.

1. Create a snapshot of your existing Redis cache cluster\. For instructions, see [Creating a Manual Backup \(AWS CLI\)](backups-manual.md#backups-manual-CLI)\.

1. Restore from the snapshot using the new, smaller node type as the cache cluster's node type, and, if needed, the new parameter group\. For more information, see [Restoring From a Backup \(AWS CLI\)](backups-restoring.md#backups-restoring-CLI)\.

1. In your application, update the endpoints to the new cache cluster's endpoints\. For more information, see [Finding Endpoints for Nodes and Clusters \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI.Nodes)\.

1. Delete your old cache cluster\. For more information, see [Using the AWS CLI](Clusters.Delete.md#Clusters.Delete.CLI)\.

1. If you no longer need it, delete the snapshot\. For more information, see [Deleting a Backup \(AWS CLI\)](backups-deleting.md#backups-deleting-CLI)\.

**Tip**  
If you don't mind your cache cluster being unavailable while it's being created or restored, you can eliminate the need to update the endpoints in your application\. To do so, delete the old cache cluster right after taking the snapshot and reuse the old cache cluster's name for the new cache cluster\.

## Scaling Down a Single\-Node Cache Cluster for Redis \(Cluster Mode Disabled\) \(ElastiCache API\)<a name="Scaling.RedisStandalone.ScaleDown.API"></a>

The following procedure walks you through scaling your single\-node Redis cache cluster down to a smaller node type using the ElastiCache API\.

**To scale down a single\-node Redis cache cluster \(ElastiCache API\)**

1. Ensure that the smaller node type is adequate for your data and overhead needs\. For more information, see [Ensuring That You Have Enough Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)\.

1. If your parameter group uses `reserved-memory` to set aside memory for Redis overhead, ensure that you have a custom parameter group to set aside the correct amount of memory for your new node type\.

   Alternatively, you can modify your custom parameter group to use `reserved-memory-percent`\. For more information, see [Managing Reserved Memory](redis-memory-management.md)\.

1. Create a snapshot of your existing Redis cache cluster\. For instructions, see [Creating a Manual Backup \(ElastiCache API\)](backups-manual.md#backups-manual-API)\.

1. Restore from the snapshot using the new, smaller node type as the cache cluster's node type, and, if needed, the new parameter group\. For more information, see [Restoring From a Backup \(ElastiCache API\)](backups-restoring.md#backups-restoring-API)\.

1. In your application, update the endpoints to the new cache cluster's endpoints\. For more information, see [Finding Endpoints for Nodes and Clusters \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API.Nodes)\.

1. Delete your old cache cluster\. For more information, see [Using the ElastiCache API](Clusters.Delete.md#Clusters.Delete.API)\.

1. If you no longer need it, delete the snapshot\. For more information, see [Deleting a Backup \(ElastiCache API\)](backups-deleting.md#backups-deleting-API)\.

**Tip**  
If you don't mind your cache cluster being unavailable while it's being created or restored, you can eliminate the need to update the endpoints in your application\. To do so, delete the old cache cluster right after taking the snapshot and reuse the old cache cluster's name for the new cache cluster\.