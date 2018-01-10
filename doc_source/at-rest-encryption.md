# Amazon ElastiCache for Redis At\-Rest Encryption<a name="at-rest-encryption"></a>

To help keep your data secure, Amazon ElastiCache and Amazon S3 provide mechanisms to restrict access to your data when it's in your cache\. For more information, see [ElastiCache and Security Groups](VPCs.md) and [Authentication and Access Control for Amazon ElastiCache](IAM.md)\.

When ElastiCache for Redis at\-rest encryption is enabled on a replication group, your data is encrypted when it's on the disk during sync and backup operations\. This approach is different from ElastiCache for Redis in\-transit encryption\. In\-transit encryption encrypts your data when it is moving from one place to another, such as between your replication group and your application\. For information about ElastiCache for Redis in\-transit encryption, see [Amazon ElastiCache for Redis In\-Transit Encryption](in-transit-encryption.md)\. At\-rest encryption is optional, and can be enables on a replication group only when it is created\. 


+ [At\-Rest Encryption Overview](#at-rest-encryption-overview)
+ [At\-Rest Encryption Constraints](#at-rest-encryption-constraints)
+ [Enabling At\-Rest Encryption](#at-rest-encryption-enable)
+ [See Also](#at-rest-encryption-see-also)

## At\-Rest Encryption Overview<a name="at-rest-encryption-overview"></a>

Amazon ElastiCache for Redis at\-rest encryption is an optional feature to increase data security by encrypting on\-disk data during sync and backup or snapshot operations\. Because there is some processing needed to encrypt and decrypt the data, enabling at\-rest encryption can have some performance impact during these operations\. You should benchmark your data with and without at\-rest encryption to determine the performance impact for your use cases\. 

## At\-Rest Encryption Constraints<a name="at-rest-encryption-constraints"></a>

The following constraints on ElastiCache at\-rest encryption should be kept in mind when you plan your implementation of ElastiCache encryption at\-rest:

+ At\-rest encryption is supported only on replication groups running Redis version 3\.2\.6\. It is not supported on clusters running Memcached\.

+ At\-rest encryption is supported only for replication groups running in an Amazon VPC\.

+ At\-rest encryption is supported for replication groups running any node type\.

  You can implement at\-rest encryption on a replication group only when creating the replication group\. You cannot toggle at\-rest encryption on and off on a replication group\. For more information, see [Enabling At\-Rest Encryption on an Existing Redis Cluster](#at-reset-encryption-enable-existing-cluster)\.

Implementing at\-rest encryption can reduce performance during backup and node sync operations\. Benchmark at\-rest encryption compared to no encryption on your own data to determine its impact on performance for your implementation\.

## Enabling At\-Rest Encryption<a name="at-rest-encryption-enable"></a>

You can enable ElastiCache at\-rest encryption when you create a Redis replication group by setting the parameter `AtRestEncryptionEnabled` to `true`\. You can't enable at\-rest encryption on existing replication groups\.

 You can enable at\-rest encryption when you create an ElastiCache for Redis replication group using the AWS Management Console, the AWS CLI, or the ElastiCache API\.



### Enabling At\-Rest Encryption on an Existing Redis Cluster<a name="at-reset-encryption-enable-existing-cluster"></a>

You can only enable at\-rest encryption when you create a Redis replication group\. If you have an existing replication group on which you want to enable at\-rest encryption, do the following\.

**To enable at\-rest encryption on an existing replication group**

1. Create a manual backup of your existing replication group\. For more information, see [Making Manual Backups](backups-manual.md)\.

1. Create a new replication grouo by restoring from the backup\. On the new replication group, enable at\-rest encryption\. For more information, see [Restoring From a Backup with Optional Cluster Resizing](backups-restoring.md)\.

1. Update the endpoints in your application to point to the new replication group\.

1. Delete the old replication group\. For more information, see [Deleting a Cluster](Clusters.Delete.md) or [Deleting a Cluster with Replicas](Replication.DeletingRepGroup.md)\.

### Enabling At\-Rest Encryption Using the AWS Management Console<a name="at-rest-encryption-enable-con"></a>

To enable at\-rest encryption when creating a replication group using the AWS Management Console, make the following selections:

+ Choose Redis as your engine\.

+ Choose version 3\.2\.6 as your engine version\.

+ Choose **Yes** from the **Encryption at\-rest** list\.

For the step\-by\-step procedure, see the following:

+ [Creating a Redis \(cluster mode disabled\) Cluster \(Console\)](Clusters.Create.CON.Redis.md)

+ [Creating a Redis \(cluster mode enabled\) Cluster \(Console\)](Clusters.Create.CON.RedisCluster.md)

### Enabling At\-Rest Encryption Using the AWS CLI<a name="at-rest-encryption-enable-cli"></a>

To enable at\-rest encryption when creating a Redis cluster using the AWS CLI, use the *\-\-at\-rest\-encryption\-enabled* parameter when creating a replication group\.

#### Enabling At\-Rest Encryption on a Redis \(cluster mode disabled\) Cluster \(CLI\)<a name="at-rest-encryption-enable-cli-redis-classic-rg"></a>

The following operation creates the Redis \(cluster mode disabled\) replication group `my-classic-rg` with three nodes \(*\-\-num\-cache\-clusters*\), a primary and two read replicas\. At\-rest encryption is enabled for this replication group \(*\-\-at\-rest\-encryption\-enabled*\)\.

The following parameters and their values are necessary to enable encryption on this replication group:

+ *\-\-engine* must be `redis`\.

+ *\-\-at\-rest\-encryption\-enabled* must be included to enable at\-rest encryption\.

**Example A Redis \(cluster mode disabled\) Cluster with Replicas**  
For Linux, macOS, or Unix:  

```
aws elasticache create-replication-group \
    --replication-group-id my-classic-rg \
    --replication-group-description "3 node replication group" \
    --cache-node-type cache.m4.large \
    --engine redis \
    --engine-version 2.8.24 \
    --at-rest-encryption-enabled \  
    --num-cache-clusters 3 \
    --cache-parameter-group default.redis3.2
```
For Windows:  

```
aws elasticache create-replication-group ^
    --replication-group-id my-classic-rg ^
    --replication-group-description "3 node replication group" ^
    --cache-node-type cache.m4.large ^
    --engine redis ^
    --engine-version 2.8.24 ^
    --at-rest-encryption-enabled ^  
    --num-cache-clusters 3 ^
    --cache-parameter-group default.redis3.2
```

For additional information, see the following:

+ [Creating a Redis \(cluster mode disabled\) Cluster with Replicas from Scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.CLI)

+ [create\-replication\-group](http://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

 

#### Enabling At\-Rest Encryption on a Redis \(cluster mode enabled\) Cluster \(CLI\)<a name="at-rest-encryption-enable-cli-clustered-redis"></a>

The following operation creates the Redis \(cluster mode enabled\) replication group `my-clustered-rg` with three node groups or shards \(*\-\-num\-node\-groups*\)\. Each has three nodes, a primary and two read replicas \(*\-\-replicas\-per\-node\-group*\)\. At\-rest encryption is enabled for this replication group \(*\-\-at\-rest\-encryption\-enabled*\)\.

The following parameters and their values are necessary to enable encryption on this replication group:

+ *\-\-engine* must be `redis`\.

+ *\-\-at\-rest\-encryption\-enabled* must be included to enable at\-rest encryption\.

+ To make the replication group a Redis \(cluster mode enabled\) replication group the following parameters are also required\. These don't affect whether at\-rest encryption is enabled\.

  + *\-\-engine\-version* must be 3\.2\.6\.

  + *\-\-cache\-parameter\-group* must be `default-redis3.2.cluster.on` or one derived from it to make this a cluster mode enabled replication group\.

**Example A Redis \(cluster mode enabled\) Cluster**  
For Linux, macOS, or Unix:  

```
aws elasticache create-replication-group \
   --replication-group-id my-clustered-rg \
   --replication-group-description "redis clustered cluster" \
   --cache-node-type cache.m3.large \
   --num-node-groups 3 \
   --replicas-per-node-group 2 \
   --engine redis \
   --engine-version 3.2.6 \
   --at-rest-encryption-enabled \
   --cache-parameter-group default.redis3.2.cluster.on
```
For Windows:  

```
aws elasticache create-replication-group ^
   --replication-group-id my-clustered-rg ^
   --replication-group-description "redis clustered cluster" ^
   --cache-node-type cache.m3.large ^
   --num-node-groups 3 ^
   --replicas-per-node-group 2 ^
   --engine redis ^
   --engine-version 3.2.6 ^
   --at-rest-encryption-enabled ^
   --cache-parameter-group default.redis3.2.cluster.on
```

For additional information, see the following:

+ [Creating a Redis \(cluster mode enabled\) Cluster with Replicas from Scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.CLI)

+ [create\-replication\-group](http://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

### Enabling At\-Rest Encryption Using the ElastiCache API<a name="at-rest-encryption-enable-api"></a>

To enable at\-rest encryption when creating a Redis replication group using the ElastiCache API, set the parameter `AtRestEncryptionEnabled` to `true` with `CreateReplicationGroup`\.

#### Enabling At\-Rest Encryption on a Redis \(cluster mode disabled\) Cluster \(API\)<a name="at-rest-encryption-enable-api-redis-classic-rg"></a>

The following operation creates the Redis \(cluster mode disabled\) replication group `my-classic-rg` with three nodes \(*NumCacheClusters*\), a primary and two read replicas\. At\-rest encryption is enabled for this replication group \(*AtRestEncryptionEnabled*`=true`\)\.

The following parameters and their values are necessary to enable encryption on this replication group:

+ *Engine* must be `redis`\.

+ *AtRestEncryptionEnabled* must be `true`\.

**Example – A Redis \(cluster mode disabled\) Cluster with Replicas**  
Line breaks are added for ease of reading\.  

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=CreateReplicationGroup 
   &AtRestEncryptionEnabled=true
   &CacheNodeType=cache.m3.large
   &CacheParameterGroup=default.redis3.2
   &Engine=redis
   &EngineVersion=2.8.24
   &NumCacheClusters=3
   &ReplicationGroupDescription=test%20group
   &ReplicationGroupId=my-classic-rg
   &Version=2015-02-02
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```

For additional information, see the following:

+ [Creating a Redis \(cluster mode disabled\) Cluster with Replicas from Scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.API)

+ [CreateReplicationGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)

 

#### Enabling At\-Rest Encryption on a Redis \(cluster mode enabled\) Cluster \(API\)<a name="at-rest-encryption-enable-api-redis-cluster"></a>

The following operation creates the Redis \(cluster mode enabled\) replication group `my-clustered-rg` with three node groups/shards \(*NumNodeGroups*\), each with three nodes, a primary and two read replicas \(*ReplicasPerNodeGroup*\)\. At\-rest encryption is enabled for this replication group \(*AtRestEncryptionEnabled*`=true`\)\.

The following parameters and their values are necessary to enable encryption on this replication group:

+ *Engine* must be `redis`\.

+ *AtRestEncryptionEnabled* must be `true`\.

+ To make the replication group a Redis \(cluster mode enabled\) cluster the following parameters are also required\. These don't affect whether at\-rest encryption is enabled\.

  + *EngineVersion* must be 3\.2\.6\.

  + *CacheParameterGroup* must be `default-redis3.2.cluster.on`, or one derived from it\.

**Example – A Redis \(cluster mode enabled\) Cluster**  
Line breaks are added for ease of reading\.  

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=CreateReplicationGroup 
   &AtRestEncryptionEnabled=true
   &CacheNodeType=cache.m3.large
   &CacheParemeterGroup=default.redis3.2.cluster.on
   &Engine=redis
   &EngineVersion=3.2.6
   &NumNodeGroups=3
   &ReplicasPerNodeGroup=2
   &ReplicationGroupDescription=test%20group
   &ReplicationGroupId=my-clustered-rg
   &Version=2015-02-02
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```

For additional information, see the following:

+ [Creating a Redis \(cluster mode enabled\) Cluster with Replicas from Scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.API)

+ [CreateReplicationGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)

 

## See Also<a name="at-rest-encryption-see-also"></a>

+ [ElastiCache and Security Groups](VPCs.md)

+ [Authentication and Access Control for Amazon ElastiCache](IAM.md)