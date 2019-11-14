# How Synchronization and Backup are Implemented<a name="Replication.Redis.Versions"></a>

All supported versions of Redis support backup and synchronization between the primary and replica nodes\. However, the way that backup and synchronization is implemented varies depending on the Redis version\.

## Redis Version 2\.8\.22 and Later<a name="Replication.Redis.Version2-8-22"></a>

Redis replication, in versions 2\.8\.22 and later, choose between two methods\. For more information, see [Redis Versions Before 2\.8\.22](#Replication.Redis.Earlier2-8-22) and [Backup and Restore for ElastiCache for Redis ](backups.md)\.

During the forkless process, if the write loads are heavy, writes to the cluster are delayed to ensure that you don't accumulate too many changes and thus prevent a successful snapshot\. 

## Redis Versions Before 2\.8\.22<a name="Replication.Redis.Earlier2-8-22"></a>

Redis backup and synchronization in versions before 2\.8\.22 is a three\-step process\.

1. Fork, and in the background process, serialize the cluster data to disk\. This creates a point\-in\-time snapshot\.

1. In the foreground, accumulate a change log in the *client output buffer*\.
**Important**  
If the change log exceeds the *client output buffer* size, the backup or synchronization fails\. For more information, see [Ensuring That You Have Enough Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)\.

1. Finally, transmit the cache data and then the change log to the replica node\.