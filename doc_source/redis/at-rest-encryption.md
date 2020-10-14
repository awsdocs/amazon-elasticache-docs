# At\-Rest Encryption in ElastiCache for Redis<a name="at-rest-encryption"></a>

To help keep your data secure, Amazon ElastiCache and Amazon S3 provide different ways to restrict access to data in your cache\. For more information, see [Amazon VPCs and ElastiCache Security](VPCs.md) and [Identity and Access Management in Amazon ElastiCache](IAM.md)\.

ElastiCache for Redis at\-rest encryption is an optional feature to increase data security by encrypting on\-disk data\. When enabled on a replication group, it encrypts the following aspects:
+ Disk during sync, backup and swap operations 
+ Backups stored in Amazon S3 

 ElastiCache for Redis offers default \(service managed\) encryption at rest, as well as ability to use your own symmetric customer managed customer master keys in [AWS Key Management Service \(KMS\)](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)\. 

At\-rest encryption can be enabled on a replication group only when it is created\. Because there is some processing needed to encrypt and decrypt the data, enabling at\-rest encryption can have a performance impact during these operations\. You should benchmark your data with and without at\-rest encryption to determine the performance impact for your use cases\. 

For information on encryption in transit, see [ElastiCache for Redis In\-Transit Encryption \(TLS\)](in-transit-encryption.md) 

**Topics**
+ [At\-Rest Encryption Conditions](#at-rest-encryption-constraints)
+ [Using Customer Managed CMKs from AWS KMS](#using-customer-managed-keys-for-elasticache-security)
+ [Enabling At\-Rest Encryption](#at-rest-encryption-enable)
+ [See Also](#at-rest-encryption-see-also)

## At\-Rest Encryption Conditions<a name="at-rest-encryption-constraints"></a>

The following constraints on ElastiCache at\-rest encryption should be kept in mind when you plan your implementation of ElastiCache encryption at\-rest:
+ At\-rest encryption is supported on replication groups running Redis version 3\.2\.6, 4\.0\.10 or later\.
+ At\-rest encryption is supported only for replication groups running in an Amazon VPC\.
+ At\-rest encryption is only supported for replication groups running the following node types\.
  + R5, R4, R3
  + M5, M4, M3
  + T3, T2

  For more information, see [Supported Node Types](CacheNodes.SupportedTypes.md)
+ At\-rest encryption is enabled by explicitly setting the parameter `AtRestEncryptionEnabled` to `true`\.
+ You can enable at\-rest encryption on a replication group only when creating the replication group\. You cannot toggle at\-rest encryption on and off by modifying a replication group\. For information on implementing at\-rest encryption on an existing replication group, see [Enabling At\-Rest Encryption](#at-rest-encryption-enable)\.
+ Encryption of data at rest is not available in the cn\-north\-1 \(Beijing\) and cn\-northwest\-1 \(Ningxia\), ap\-northeast\-3 \(Asia Pacific Osaka\-Local\) regions\. Additionally, the option to use customer managed CMK for encryption at rest is not available in AWS GovCloud \(us\-gov\-east\-1 and us\-gov\-west\-1\) regions\. 

Implementing at\-rest encryption can reduce performance during backup and node sync operations\. Benchmark at\-rest encryption compared to no encryption on your own data to determine its impact on performance for your implementation\.

## Using Customer Managed CMKs from AWS KMS<a name="using-customer-managed-keys-for-elasticache-security"></a>

ElastiCache for Redis supports symmetric customer managed customer master keys \(CMK\) for encryption at rest\. Customer\-managed CMKs are encryption keys that you create, own and manage in your AWS account\. For more information, see [Customer Master Keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys) in the *AWS Key Management Service Developer Guide*\. The keys must be created in AWS KMS before they can be used with Elasticache\.

To learn how to create AWS KMS master keys, see [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*\. 

ElastiCache for Redis allows you to integrate with AWS KMS\. For more information, see [Using Grants](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html) in the *AWS Key Management Service Developer Guide*\. No customer action is needed to enable Amazon ElastiCache integration with AWS KMS\. 

Note that Amazon ElastiCache currently does not support [kms:ViaService](https://docs.aws.amazon.com/kms/latest/developerguide/policy-conditions.html#conditions-kms-via-service)\. Providing/denying access to Amazon ElastiCache using ViaService will have no effect on key permissions\. 

You can use [AWS CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) to track the requests that Amazon ElastiCache sends to AWS Key Management Service on your behalf\. All API calls to AWS Key Management Service related to customer managed CMKs have corresponding CloudTrail logs\. You can also see the grants that ElastiCache creates by calling the [ListGrants](https://docs.aws.amazon.com/kms/latest/APIReference/API_ListGrants.html) KMS API call\. 

Once a replication group is encrypted using customer managed CMK, all backups for the replication group are encrypted as follows:
+ Automatic daily backups are encrypted using the customer managed CMK associated with the cluster\.
+ Final backup created when replication group is deleted, is also encrypted using the customer managed CMK associated with the replication group\.
+ Manually created backups are encrypted by default to use the CMK associated with the replication group\. You may override this by choosing another customer managed CMK\.
+ Copying a backup defaults to using customer managed CMK associated with the source backup\. You may override this by choosing another customer managed CMK\.

**Note**  
Customer managed CMKs cannot be used when exporting backups to your selected Amazon S3 bucket\. However, all backups exported to Amazon S3 are encrypted using [Server side encryption\.](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html) You may choose to copy the backup file to a new S3 object and encrypt using a customer managed CMK, copy the file to another S3 bucket that is set up with default encryption using a CMK or change an encryption option in the file itself\.
You can also use customer managed CMKs to encrypt manually\-created backups for replication groups that do not use customer managed CMKs for encryption\. With this option, the backup file stored in Amazon S3 is encrypted using a CMK, even though the data is not encrypted on the original replication group\. 
Restoring from a backup allows you to choose from available encryption options, similar to encryption choices available when creating a new replication group\.  
Also consider:
+ If you delete the key or [disable](https://docs.aws.amazon.com/kms/latest/developerguide/enabling-keys.html) the key and [revoke grants](https://docs.aws.amazon.com/kms/latest/APIReference/API_RevokeGrant.html) for the key that you used to encrypt a replication group, the replication group becomes irrecoverable\. In other words, it cannot be modified or recovered after a hardware failure\. AWS KMS deletes master keys only after a waiting period of at least seven days\. After the key is deleted, you can use a different customer managed CMK to create a backup for archival purposes\. 
+ Automatic key rotation preserves the properties of your AWS KMS master keys, so the rotation has no effect on your ability to access your ElastiCache data\. Encrypted Amazon ElastiCache replication groups don't support manual key rotation, which involves creating a new master key and updating any references to the old key\. To learn more, see [Rotating Customer Master Keys](https://docs.aws.amazon.com/kms/latest/developerguide/rotate-keys.html) in the *AWS Key Management Service Developer Guide*\. 
+ Encrypting an ElastiCache replication group using CMK requires one grant per replication group\. This grant is used throughout the lifespan of the replication group\. Additionally, one grant per backup is used during backup creation\. This grant is retired once the backup is created\. 
+ For more information on AWS KMS grants and limits, see [Limits](https://docs.aws.amazon.com/kms/latest/developerguide/limits.html) in the *AWS Key Management Service Developer Guide*\.

## Enabling At\-Rest Encryption<a name="at-rest-encryption-enable"></a>

You can enable ElastiCache at\-rest encryption when you create a Redis replication group by setting the parameter `AtRestEncryptionEnabled` to `true`\. You can't enable at\-rest encryption on existing replication groups\.

 You can enable at\-rest encryption when you create an ElastiCache for Redis replication group\. You can do so using the AWS Management Console, the AWS CLI, or the ElastiCache API\.

When creating a replication group, you can pick one of the following options:
+ **Default** – This option uses service managed encryption at rest\. 
+ **Customer managed CMK ** – This option allows you to provide the Key ID/ARN from AWS KMS for encryption at rest\. 

To learn how to create AWS KMS master keys, see [Create Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide* 

**Contents**
+ [Enabling At\-Rest Encryption on an Existing Redis Cluster](#at-reset-encryption-enable-existing-cluster)
+ [Enabling At\-Rest Encryption Using the AWS Management Console](#at-rest-encryption-enable-con)
+ [Enabling At\-Rest Encryption Using the AWS CLI](#at-rest-encryption-enable-cli)
  + [On a Redis \(Cluster Mode Disabled\) Cluster with Replicas](#at-rest-encryption-enable-cli-redis-classic-rg)
  + [On a Cluster for Redis \(Cluster Mode Enabled\)](#at-rest-encryption-enable-cli-clustered-redis)
+ [Enabling At\-Rest Encryption Using the ElastiCache API](#at-rest-encryption-enable-api)
  + [On a Redis \(Cluster Mode Disabled\) Cluster with Replicas](#at-rest-encryption-enable-api-redis-classic-rg)
  + [On a Cluster for Redis \(Cluster Mode Enabled\)](#at-rest-encryption-enable-api-redis-cluster)

### Enabling At\-Rest Encryption on an Existing Redis Cluster<a name="at-reset-encryption-enable-existing-cluster"></a>

You can only enable at\-rest encryption when you create a Redis replication group\. If you have an existing replication group on which you want to enable at\-rest encryption, do the following\.

**To enable at\-rest encryption on an existing replication group**

1. Create a manual backup of your existing replication group\. For more information, see [Making Manual Backups](backups-manual.md)\.

1. Create a new replication group by restoring from the backup\. On the new replication group, enable at\-rest encryption\. For more information, see [Restoring From a Backup with Optional Cluster Resizing](backups-restoring.md)\.

1. Update the endpoints in your application to point to the new replication group\.

1. Delete the old replication group\. For more information, see [Deleting a Cluster](Clusters.Delete.md) or [Deleting a Replication Group](Replication.DeletingRepGroup.md)\.

### Enabling At\-Rest Encryption Using the AWS Management Console<a name="at-rest-encryption-enable-con"></a>

To enable at\-rest encryption when creating a replication group using the AWS Management Console, make the following selections:
+ Choose redis as your engine\.
+ Choose version 3\.2\.6, 4\.0\.10 or later as your engine version\.
+ Choose **Yes** from the **Encryption at\-rest** list\.

For the step\-by\-step procedure, see the following:
+ [Creating a Cluster Mode Disabled Cluster \(Console\)](Clusters.Create.CON.Redis.md)
+ [Creating a Redis \(Cluster Mode Enabled\) Cluster \(Console\)](Clusters.Create.CON.RedisCluster.md)

### Enabling At\-Rest Encryption Using the AWS CLI<a name="at-rest-encryption-enable-cli"></a>

To enable at\-rest encryption when creating a Redis cluster using the AWS CLI, use the *\-\-at\-rest\-encryption\-enabled* parameter when creating a replication group\.

#### Enabling At\-Rest Encryption on a Redis \(Cluster Mode Disabled\) Cluster \(CLI\)<a name="at-rest-encryption-enable-cli-redis-classic-rg"></a>

The following operation creates the Redis \(cluster mode disabled\) replication group `my-classic-rg` with three nodes \(*\-\-num\-cache\-clusters*\), a primary and two read replicas\. At\-rest encryption is enabled for this replication group \(*\-\-at\-rest\-encryption\-enabled*\)\.

The following parameters and their values are necessary to enable encryption on this replication group:

**Key Parameters**
+ **\-\-engine**—Must be `redis`\.
+ **\-\-engine\-version**—Must be 3\.2\.6, 4\.0\.10 or later\.
+ **\-\-at\-rest\-encryption\-enabled**—Required to enable at\-rest encryption\.

**Example 1: Redis \(Cluster Mode Disabled\) Cluster with Replicas**  
For Linux, macOS, or Unix:  

```
aws elasticache create-replication-group \
    --replication-group-id my-classic-rg \
    --replication-group-description "3 node replication group" \
    --cache-node-type cache.m4.large \
    --engine redis \
    --engine-version 4.0.10 \
    --at-rest-encryption-enabled \  
    --num-cache-clusters 3 \
    --cache-parameter-group default.redis4.0
```
For Windows:  

```
aws elasticache create-replication-group ^
    --replication-group-id my-classic-rg ^
    --replication-group-description "3 node replication group" ^
    --cache-node-type cache.m4.large ^
    --engine redis ^
    --engine-version 4.0.10 ^
    --at-rest-encryption-enabled ^  
    --num-cache-clusters 3 ^
    --cache-parameter-group default.redis4.0
```

For additional information, see the following:
+ [Creating a Redis \(Cluster Mode Disabled\) Replication Group from Scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.CLI)
+ [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

 

#### Enabling At\-Rest Encryption on a Cluster for Redis \(Cluster Mode Enabled\) \(CLI\)<a name="at-rest-encryption-enable-cli-clustered-redis"></a>

The following operation creates the Redis \(cluster mode enabled\) replication group `my-clustered-rg` with three node groups or shards \(*\-\-num\-node\-groups*\)\. Each has three nodes, a primary and two read replicas \(*\-\-replicas\-per\-node\-group*\)\. At\-rest encryption is enabled for this replication group \(*\-\-at\-rest\-encryption\-enabled*\)\.

The following parameters and their values are necessary to enable encryption on this replication group:

**Key Parameters**
+ **\-\-engine**—Must be `redis`\.
+ **\-\-engine\-version**—Must be 3\.2\.6, 4\.0\.10 or later\.
+ **\-\-at\-rest\-encryption\-enabled**—Required to enable at\-rest encryption\.
+ **\-\-cache\-parameter\-group**—Must be `default-redis4.0.cluster.on` or one derived from it to make this a cluster mode enabled replication group\.

**Example 2: A Redis \(Cluster Mode Enabled\) Cluster**  
For Linux, macOS, or Unix:  

```
aws elasticache create-replication-group \
   --replication-group-id my-clustered-rg \
   --replication-group-description "redis clustered cluster" \
   --cache-node-type cache.m3.large \
   --num-node-groups 3 \
   --replicas-per-node-group 2 \
   --engine redis \
   --engine-version 4.0.10 \
   --at-rest-encryption-enabled \
   --cache-parameter-group default.redis4.0.cluster.on
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
   --engine-version 4.0.10 ^
   --at-rest-encryption-enabled ^
   --cache-parameter-group default.redis4.0.cluster.on
```

For additional information, see the following:
+ [Creating a Redis \(Cluster Mode Enabled\) Replication Group from Scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.CLI)
+ [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

### Enabling At\-Rest Encryption Using the ElastiCache API<a name="at-rest-encryption-enable-api"></a>

To enable at\-rest encryption when creating a Redis replication group using the ElastiCache API, set the parameter `AtRestEncryptionEnabled` to `true` with `CreateReplicationGroup`\.

#### Enabling At\-Rest Encryption on a Redis \(Cluster Mode Disabled\) Cluster \(API\)<a name="at-rest-encryption-enable-api-redis-classic-rg"></a>

The following operation creates the Redis \(cluster mode disabled\) replication group `my-classic-rg` with three nodes \(*NumCacheClusters*\), a primary and two read replicas\. At\-rest encryption is enabled for this replication group \(*AtRestEncryptionEnabled*`=true`\)\.

The following parameters and their values are necessary to enable encryption on this replication group:
+ **Engine**—Must be `redis`\.
+ **EngineVersion**—Must be 3\.2\.6, 4\.0\.10 or later\.
+ **AtRestEncryptionEnabled**—Required to be `true` to enable at\-rest encryption\.

**Example 3: A Redis \(Cluster Mode Disabled\) Cluster with Replicas**  
Line breaks are added for ease of reading\.  

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=CreateReplicationGroup 
   &AtRestEncryptionEnabled=true
   &CacheNodeType=cache.m3.large
   &CacheParameterGroup=default.redis4.0
   &Engine=redis
   &EngineVersion=4.0.10
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
+ [Creating a Redis \(cluster mode disabled\) Replication Group from Scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.API)
+ [CreateReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)

 

#### Enabling At\-Rest Encryption on a Cluster for Redis \(Cluster Mode Enabled\) \(API\)<a name="at-rest-encryption-enable-api-redis-cluster"></a>

The following operation creates the Redis \(cluster mode enabled\) replication group `my-clustered-rg` with three node groups/shards \(*NumNodeGroups*\), each with three nodes, a primary and two read replicas \(*ReplicasPerNodeGroup*\)\. At\-rest encryption is enabled for this replication group \(*AtRestEncryptionEnabled*`=true`\)\.

The following parameters and their values are necessary to enable encryption on this replication group:
+ **Engine**—Must be `redis`\.
+ **AtRestEncryptionEnabled**—Required to be `true` to enable at\-rest encryption\.
+ **EngineVersion**—Must be 3\.2\.6, 4\.0\.10 or later\.
+ **CacheParameterGroup**—Must be `default-redis4.0.cluster.on`, or one derived from it for this to be a Redis \(cluster mode enabled\) cluster\.

**Example 4: A Redis \(Cluster Mode Enabled\) Cluster**  
Line breaks are added for ease of reading\.  

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=CreateReplicationGroup 
   &AtRestEncryptionEnabled=true
   &CacheNodeType=cache.m3.large
   &CacheParemeterGroup=default.redis4.0.cluster.on
   &Engine=redis
   &EngineVersion=4.0.10
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
+ [Creating a Replication Group in Redis \(Cluster Mode Enabled\) from Scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.API)
+ [CreateReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)

 

## See Also<a name="at-rest-encryption-see-also"></a>
+ [Amazon VPCs and ElastiCache Security](VPCs.md)
+ [Identity and Access Management in Amazon ElastiCache](IAM.md)