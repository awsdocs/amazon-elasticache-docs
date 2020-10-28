# Ensuring That You Have Enough Memory to Create a Redis Snapshot<a name="BestPractices.BGSAVE"></a>

**Redis snapshots and synchronizations in version 2\.8\.22 and later**  
Redis 2\.8\.22 introduces a forkless save process that allows you to allocate more of your memory to your application's use without incurring increased swap usage during synchronizations and saves\. For more information, see [How Synchronization and Backup are Implemented](Replication.Redis.Versions.md)\.

**Redis snapshots and synchronizations before version 2\.8\.22**

When you work with Redis ElastiCache, Redis calls a background write command in a number of cases:
+ When creating a snapshot for a backup\.
+ When synchronizing replicas with the primary in a replication group\.
+ When enabling the append\-only file feature \(AOF\) for Redis\.
+ When promoting a replica to master \(which causes a primary/replica sync\)\.

Whenever Redis executes a background write process, you must have sufficient available memory to accommodate the process overhead\. Failure to have sufficient memory available causes the process to fail\. Because of this, it is important to choose a node instance type that has sufficient memory when creating your Redis cluster\.

## Background Write Process and Memory Usage<a name="BestPractices.BGSAVE.Process"></a>

Whenever a background write process is called, Redis forks its process \(remember, Redis is single threaded\)\. One fork persists your data to disk in a Redis \.rdb snapshot file\. The other fork services all read and write operations\. To ensure that your snapshot is a point\-in\-time snapshot, all data updates and additions are written to an area of available memory separate from the data area\.

As long as you have sufficient memory available to record all write operations while the data is being persisted to disk, you should have no insufficient memory issues\. You are likely to experience insufficient memory issues if any of the following are true:
+ Your application performs many write operations, thus requiring a large amount of available memory to accept the new or updated data\.
+ You have very little memory available in which to write new or updated data\.
+ You have a large dataset that takes a long time to persist to disk, thus requiring a large number of write operations\.

The following diagram illustrates memory use when executing a background write process\.

![\[Image: Diagram of memory use during a background write.\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-bgsaveMemoryUseage.png)

For information on the impact of doing a backup on performance, see [Performance Impact of Backups](backups.md#backups-performance)\.

For more information on how Redis performs snapshots, see [http://redis\.io](http://redis.io)\.

For more information on regions and Availability Zones, see [Choosing Regions and Availability Zones](RegionsAndAZs.md)\.

## Avoiding Running Out of Memory When Executing a Background Write<a name="BestPractices.BGSAVE.memoryFix"></a>

Whenever a background write process such as BGSAVE or BGREWRITEAOF is called, to keep the process from failing, you must have more memory available than will be consumed by write operations during the process\. The worst\-case scenario is that during the background write operation every Redis record is updated and some new records are added to the cache\. Because of this, we recommend that you set `reserved-memory-percent` to 50 \(50 percent\) for Redis versions before 2\.8\.22 or 25 \(25 percent\) for Redis versions 2\.8\.22 and later\. 

The `maxmemory` value indicates the memory available to you for data and operational overhead\. Because you cannot modify the `reserved-memory` parameter in the default parameter group, you must create a custom parameter group for the cluster\. The default value for `reserved-memory` is 0, which allows Redis to consume all of *maxmemory* with data, potentially leaving too little memory for other uses, such as a background write process\. For `maxmemory` values by node instance type, see [Redis Node\-Type Specific Parameters](ParameterGroups.Redis.md#ParameterGroups.Redis.NodeSpecific)\.

You can also use `reserved-memory` parameter to reduce the amount of memory Redis uses on the box\.

For more information on Redis\-specific parameters in ElastiCache, see [Redis\-specific parameters](ParameterGroups.Redis.md)\.

For information on creating and modifying parameter groups, see [Creating a Parameter Group](ParameterGroups.Creating.md) and [Modifying a Parameter Group](ParameterGroups.Modifying.md)\.