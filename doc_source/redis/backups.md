# Backup and Restore for ElastiCache for Redis<a name="backups"></a>

Amazon ElastiCache clusters running Redis can back up their data\. You can use the backup to restore a cluster or seed a new cluster\. The backup consists of the cluster's metadata, along with all of the data in the cluster\. All backups are written to Amazon Simple Storage Service \(Amazon S3\), which provides durable storage\. At any time, you can restore your data by creating a new Redis cluster and populating it with data from a backup\. With ElastiCache, you can manage backups using the AWS Management Console, the AWS Command Line Interface \(AWS CLI\), and the ElastiCache API\.

Beginning with Redis version 2\.8\.22, the backup method is selected based upon available memory\. If there is sufficient available memory, a child process is spawned that writes all changes to the cache's reserved memory while the cache is being backed up\. Depending on the number of writes to the cache during the backup process, this child process can consume all reserved memory, causing the backup to fail\.

If there is insufficient memory available, a forkless, cooperative background process is employed\. The forkless method can affect both latency and throughput\. For more information, see [How Synchronization and Backup are Implemented](Replication.Redis.Versions.md)\.

For more information about the performance impact of the backup process, see [Performance Impact of Backups](#backups-performance)\.

Following, you can find an overview of working with backup and restore\. 

**Important**  
Though it's rare, sometimes the backup process fails to create a backup, including final backups\. Insufficient reserved memory is often the cause of backup failures\. Therefore, make sure that you have sufficient reserved memory before attempting a backup\. If you have insufficient memory, you can either evict some keys or increase the value of `reserved-memory-percent`\.  
For more information, see the following:  
 [Ensuring That You Have Enough Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)
 [Managing Reserved Memory](redis-memory-management.md)
If you plan to delete cluster and it's important to preserve the data, you can take an extra precaution\. To do this, create a manual backup first, verify that its status is *available*, and then delete the cluster\. Doing this makes sure that if the backup fails, you still have the cluster data available\. You can retry making a backup, following the best practices outlined preceding\.

**Topics**
+ [Backup Constraints](#backups-constraints)
+ [Backup Costs](#backups-costs)
+ [Performance Impact of Backups](#backups-performance)
+ [Backups When Running Redis 2\.8\.22 and Later](#backups-performance-2.8.22-later)
+ [Backups When Running Redis Versions before 2\.8\.22](#backups-performance-2.8.22-before)
+ [Improving Backup Performance](#backups-performance-improving)
+ [Scheduling Automatic Backups](backups-automatic.md)
+ [Making Manual Backups](backups-manual.md)
+ [Creating a Final Backup](backups-final.md)
+ [Describing Backups](backups-describing.md)
+ [Copying a Backup](backups-copying.md)
+ [Exporting a Backup](backups-exporting.md)
+ [Restoring From a Backup with Optional Cluster Resizing](backups-restoring.md)
+ [Seeding a New Cluster with an Externally Created Backup](backups-seeding-redis.md)
+ [Tagging Backups](backups-tagging.md)
+ [Deleting a Backup](backups-deleting.md)
+ [Append Only Files \(AOF\) in ElastiCache for Redis](RedisAOF.md)

## Backup Constraints<a name="backups-constraints"></a>

Consider the following constraints when planning or making backups:
+ At this time, backup and restore are supported only for clusters running on Redis\.
+ For Redis \(cluster mode disabled\) clusters, backup and restore aren't supported on `cache.t1.micro` nodes\. All other cache node types are supported\.
+ For Redis \(cluster mode enabled\) clusters, backup and restore are supported for all node types\.
+ During any contiguous 24\-hour period, you can create no more than 20 manual backups per node in the cluster\.
+ Redis \(cluster mode enabled\) only supports taking backups on the cluster level \(for the API or CLI, the replication group level\)\. Redis \(cluster mode enabled\) doesn't support taking backups at the shard level \(for the API or CLI, the node group level\)\.
+ During the backup process, you can't run any other API or CLI operations on the cluster\.

## Backup Costs<a name="backups-costs"></a>

Using ElastiCache, you can store one backup for each active Redis cluster free of charge\. Storage space for additional backups is charged at a rate of $0\.085/GB per month for all AWS Regions\. There are no data transfer fees for creating a backup, or for restoring data from a backup to a Redis cluster\.

## Performance Impact of Backups<a name="backups-performance"></a>

The backup process depends upon which Redis version you're running\. Beginning with Redis 2\.8\.22, the process is forkless\.

## Backups When Running Redis 2\.8\.22 and Later<a name="backups-performance-2.8.22-later"></a>

In versions 2\.8\.22 and later, Redis backups choose between two methods\. If there isn't enough memory to support a forked backup, ElastiCache use a forkless method that uses cooperative background processing\. If there is enough memory to support a forked save process, the same process is used as in earlier Redis versions\.

If the write load is high during a forkless backup, writes to the cache are delayed\. This delay makes sure that you don't accumulate too many changes and thus prevent a successful backup\.

## Backups When Running Redis Versions before 2\.8\.22<a name="backups-performance-2.8.22-before"></a>

Backups are created using Redis' native BGSAVE operation\. The Redis process on the cache node spawns a child process to write all the data from the cache to a Redis \.rdb file\. It can take up to 10 seconds to spawn the child process\. During this time, the parent process is unable to accept incoming application requests\. After the child process is running independently, the parent process resumes normal operations\. The child process exits when the backup operation is complete\. 

While the backup is being written, additional cache node memory is used for new writes\. If this additional memory usage exceeds the node's available memory, processing can become slow due to excessive paging, or fail\.

## Improving Backup Performance<a name="backups-performance-improving"></a>

The following are guidelines for improving backup performance\.
+ Set the `reserved-memory-percent` parameter – To mitigate excessive paging, we recommend that you set the *reserved\-memory\-percent* parameter\. This parameter prevents Redis from consuming all of the node's available memory, and can help reduce the amount of paging\. You might also see performance improvements by simply using a larger node\. For more information about the *reserved\-memory* and *reserved\-memory\-percent* parameters, see [Managing Reserved Memory](redis-memory-management.md)\.

   
+ Create backups from a read replica – If you are running Redis in a node group with more than one node, you can take a backup from the primary node or one of the read replicas\. Because of the system resources required during BGSAVE, we recommend that you create backups from one of the read replicas\. While the backup is being created from the replica, the primary node remains unaffected by BGSAVE resource requirements\. The primary node can continue serving requests without slowing down\.

If you delete a replication group and request a final backup, ElastiCache always takes the backup from the primary node\. This ensures that you capture the very latest Redis data, before the replication group is deleted\.