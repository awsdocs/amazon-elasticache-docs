# Managing Reserved Memory<a name="redis-memory-management"></a>

Reserved memory is memory set aside for nondata use\. When performing a backup or failover, Redis uses available memory to record write operations to your cluster while the cluster's data is being written to the \.rdb file\. If you don't have sufficient memory available for all the writes, the process fails\. Following, you can find information on options for managing reserved memory for ElastiCache for Redis and how to apply those options\.

**Topics**
+ [How Much Reserved Memory Do You Need?](#redis-memory-management-need)
+ [Parameters to Manage Reserved Memory](#redis-memory-management-parameters)
+ [Specifying Your Reserved Memory Management Parameter](#redis-reserved-memory-management-change)

## How Much Reserved Memory Do You Need?<a name="redis-memory-management-need"></a>

If you are running a version of Redis before 2\.8\.22, reserve more memory for backups and failovers than if you are running Redis 2\.8\.22 or later\. This requirement is due to the different ways that ElastiCache for Redis implements the backup process\. The rule of thumb is to reserve half of a node type's `maxmemory` value for Redis overhead for versions before 2\.8\.22, and one\-fourth for Redis versions 2\.8\.22 and later\. 

When using clusters with data tiering, we recommend increasing maxmemory to up to half your node's available memory if your workload is write\-heavy\.

For more information, see the following:
+ [Ensuring that you have enough memory to create a Redis snapshot](BestPractices.BGSAVE.md)
+ [How synchronization and backup are implemented](Replication.Redis.Versions.md)
+ [Data tiering](data-tiering.md)

## Parameters to Manage Reserved Memory<a name="redis-memory-management-parameters"></a>

As of March 16, 2017, Amazon ElastiCache for Redis provides two mutually exclusive parameters for managing your Redis memory, `reserved-memory` and `reserved-memory-percent`\. Neither of these parameters is part of the Redis distribution\. 

Depending upon when you became an ElastiCache customer, one or the other of these parameters is the default memory management parameter\. This parameter applies when you create a new Redis cluster or replication group and use a default parameter group\. 
+ For customers who started before March 16, 2017 – When you create a Redis cluster or replication group using the default parameter group, your memory management parameter is `reserved-memory`\. In this case, zero \(0\) bytes of memory are reserved\. 
+ For customers who started on or after March 16, 2017 – When you create a Redis cluster or replication group using the default parameter group, your memory management parameter is `reserved-memory-percent`\. In this case, 25 percent of your node's `maxmemory` value is reserved for nondata purposes\.

After reading about the two Redis memory management parameters, you might prefer to use the one that isn't your default or with nondefault values\. If so, you can change to the other reserved memory management parameter\. 

To change the value of that parameter, you can create a custom parameter group and modify it to use your preferred memory management parameter and value\. You can then use the custom parameter group whenever you create a new Redis cluster or replication group\. For existing clusters or replication groups, you can modify them to use your custom parameter group\.

 For more information, see the following: 
+ [Specifying Your Reserved Memory Management Parameter](#redis-reserved-memory-management-change)
+ [Creating a parameter group](ParameterGroups.Creating.md)
+ [Modifying a parameter group](ParameterGroups.Modifying.md)
+ [Modifying an ElastiCache cluster](Clusters.Modify.md)
+ [Modifying a replication group](Replication.Modify.md)

### The reserved\-memory Parameter<a name="redis-memory-management-parameters-reserved-memory"></a>

Before March 16, 2017, all ElastiCache for Redis reserved memory management was done using the parameter `reserved-memory`\. The default value of `reserved-memory` is 0\. This default reserves no memory for Redis overhead and allows Redis to consume all of a node's memory with data\. 

Changing `reserved-memory` so you have sufficient memory available for backups and failovers requires you to create a custom parameter group\. In this custom parameter group, you set `reserved-memory` to a value appropriate for the Redis version running on your cluster and cluster's node type\. For more information, see [How Much Reserved Memory Do You Need?](#redis-memory-management-need)

The ElastiCache for Redis parameter `reserved-memory` is specific to ElastiCache for Redis and isn't part of the Redis distribution\.

The following procedure shows how to use `reserved-memory` to manage the memory on your Redis cluster\.

**To reserve memory using reserved\-memory**

1. Create a custom parameter group specifying the parameter group family matching the engine version you’re running—for example, specifying the `redis2.8` parameter group family\. For more information, see [Creating a parameter group](ParameterGroups.Creating.md)\.

   ```
   aws elasticache create-cache-parameter-group \
      --cache-parameter-group-name redis28-m3xl \
      --description "Redis 2.8.x for m3.xlarge node type" \
      --cache-parameter-group-family redis2.8
   ```

1. Calculate how many bytes of memory to reserve for Redis overhead\. You can find the value of `maxmemory` for your node type at [Redis node\-type specific parameters](ParameterGroups.Redis.md#ParameterGroups.Redis.NodeSpecific)\.

1. Modify the custom parameter group so that the parameter `reserved-memory` is the number of bytes you calculated in the previous step\. The following AWS CLI example assumes you’re running a version of Redis before 2\.8\.22 and need to reserve half of the node’s `maxmemory`\. For more information, see [Modifying a parameter group](ParameterGroups.Modifying.md)\.

   ```
   aws elasticache modify-cache-parameter-group \
      --cache-parameter-group-name redis28-m3xl \
      --parameter-name-values "ParameterName=reserved-memory, ParameterValue=7130316800"
   ```

   You need a separate custom parameter group for each node type that you use, because each node type has a different `maxmemory` value\. Thus, each node type needs a different value for `reserved-memory`\.

1. Modify your Redis cluster or replication group to use your custom parameter group\.

   The following CLI example modifies the cluster ` my-redis-cluster` to use the custom parameter group `redis28-m3xl` beginning immediately\. For more information, see [Modifying an ElastiCache cluster](Clusters.Modify.md)\.

   ```
   aws elasticache modify-cache-cluster \
      --cache-cluster-id my-redis-cluster \
      --cache-parameter-group-name redis28-m3xl \
      --apply-immediately
   ```

   The following CLI example modifies the replication group `my-redis-repl-grp` to use the custom parameter group `redis28-m3xl` beginning immediately\. For more information, [Modifying a replication group](Replication.Modify.md)\.

   ```
   aws elasticache modify-replication-group \
      --replication-group-id my-redis-repl-grp \
      --cache-parameter-group-name redis28-m3xl \
      --apply-immediately
   ```

### The reserved\-memory\-percent parameter<a name="redis-memory-management-parameters-reserved-memory-percent"></a>

On March 16, 2017, Amazon ElastiCache introduced the parameter `reserved-memory-percent` and made it available on all versions of ElastiCache for Redis\. The purpose of `reserved-memory-percent` is to simplify reserved memory management across all your clusters\. It does so by enabling you to have a single parameter group for each parameter group family \(such as `redis2.8`\) to manage your clusters' reserved memory, regardless of node type\. The default value for `reserved-memory-percent` is 25 \(25 percent\)\.

The ElastiCache for Redis parameter `reserved-memory-percent` is specific to ElastiCache for Redis and isn't part of the Redis distribution\.

If your cluster is using a node type from the r6gd family and your memory usage reaches 75 percent, data\-tiering will automatically be triggered\. For more information, see [Data tiering](data-tiering.md)\.

**To reserve memory using reserved\-memory\-percent**  
To use `reserved-memory-percent` to manage the memory on your ElastiCache for Redis cluster, do one of the following:
+ If you are running Redis 2\.8\.22 or later, assign the default parameter group to your cluster\. The default 25 percent should be adequate\. If not, take the steps described following to change the value\.
+ If you are running a version of Redis before 2\.8\.22, you probably need to reserve more memory than `reserved-memory-percent`'s default 25 percent\. To do so, use the following procedure\. 

**To change the percent value of reserved\-memory\-percent**

1. Create a custom parameter group specifying the parameter group family matching the engine version you’re running—for example, specifying the `redis2.8` parameter group family\. A custom parameter group is necessary because you can't modify a default parameter group\. For more information, see [Creating a parameter group](ParameterGroups.Creating.md)\.

   ```
   aws elasticache create-cache-parameter-group \
      --cache-parameter-group-name redis28-50 \
      --description "Redis 2.8.x 50% reserved" \
      --cache-parameter-group-family redis2.8
   ```

   Because `reserved-memory-percent` reserves memory as a percent of a node’s `maxmemory`, you don't need a custom parameter group for each node type\.

1. Modify the custom parameter group so that `reserved-memory-percent` is 50 \(50 percent\)\. For more information, see [Modifying a parameter group](ParameterGroups.Modifying.md)\.

   ```
   aws elasticache modify-cache-parameter-group \
      --cache-parameter-group-name redis28-50 \
      --parameter-name-values "ParameterName=reserved-memory-percent, ParameterValue=50"
   ```

1. Use this custom parameter group for any Redis clusters or replication groups running a version of Redis older than 2\.8\.22\.

   The following CLI example modifies the Redis cluster `my-redis-cluster` to use the custom parameter group `redis28-50` beginning immediately\. For more information, see [Modifying an ElastiCache cluster](Clusters.Modify.md)\.

   ```
   aws elasticache modify-cache-cluster \
      --cache-cluster-id my-redis-cluster \
      --cache-parameter-group-name redis28-50 \
      --apply-immediately
   ```

   The following CLI example modifies the Redis replication group `my-redis-repl-grp` to use the custom parameter group `redis28-50` beginning immediately\. For more information, see [Modifying a replication group](Replication.Modify.md)\.

   ```
   aws elasticache modify-replication-group \
      --replication-group-id my-redis-repl-grp \
      --cache-parameter-group-name redis28-50 \
      --apply-immediately
   ```

## Specifying Your Reserved Memory Management Parameter<a name="redis-reserved-memory-management-change"></a>

If you were a current ElastiCache customer on March 16, 2017, your default reserved memory management parameter is `reserved-memory` with zero \(0\) bytes of reserved memory\. If you became an ElastiCache customer after March 16, 2017, your default reserved memory management parameter is `reserved-memory-percent` with 25 percent of the node's memory reserved\. This is true no matter when you created your ElastiCache for Redis cluster or replication group\. However, you can change your reserved memory management parameter using either the AWS CLI or ElastiCache API\.

The parameters `reserved-memory` and `reserved-memory-percent` are mutually exclusive\. A parameter group always has one but never both\. You can change which parameter a parameter group uses for reserved memory management by modifying the parameter group\. The parameter group must be a custom parameter group, because you can't modify default parameter groups\. For more information, see [Creating a parameter group](ParameterGroups.Creating.md)\.

**To specify reserved\-memory\-percent**  
To use `reserved-memory-percent` as your reserved memory management parameter, modify a custom parameter group using the `modify-cache-parameter-group` command\. Use the `parameter-name-values` parameter to specify `reserved-memory-percent` and a value for it\.

The following CLI example modifies the custom parameter group `redis32-cluster-on` so that it uses `reserved-memory-percent` to manage reserved memory\. A value must be assigned to `ParameterValue` for the parameter group to use the `ParameterName` parameter for reserved memory management\. For more information, see [Modifying a parameter group](ParameterGroups.Modifying.md)\.

```
aws elasticache modify-cache-parameter-group \
   --cache-parameter-group-name redis32-cluster-on \
   --parameter-name-values "ParameterName=reserved-memory-percent, ParameterValue=25"
```

**To specify reserved\-memory**  
To use `reserved-memory` as your reserved memory management parameter, modify a custom parameter group using the `modify-cache-parameter-group` command\. Use the `parameter-name-values` parameter to specify `reserved-memory` and a value for it\.

The following CLI example modifies the custom parameter group `redis32-m3xl` so that it uses `reserved-memory` to manage reserved memory\. A value must be assigned to `ParameterValue` for the parameter group to use the `ParameterName` parameter for reserved memory management\. Because the engine version is newer than 2\.8\.22, we set the value to `3565158400` which is 25 percent of a `cache.m3.xlarge`’s `maxmemory`\. For more information, see [Modifying a parameter group](ParameterGroups.Modifying.md)\.

```
aws elasticache modify-cache-parameter-group \
   --cache-parameter-group-name redis32-m3xl \
   --parameter-name-values "ParameterName=reserved-memory, ParameterValue=3565158400"
```