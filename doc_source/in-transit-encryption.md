# Amazon ElastiCache for Redis In\-Transit Encryption<a name="in-transit-encryption"></a>

To help keep your data secure, Amazon ElastiCache and Amazon EC2 provide mechanisms to guard against unauthorized access of your data on the server\. By providing in\-transit encryption capability, ElastiCache gives you a tool you can use to help protect your data when it is moving from one location to another\. For example, you might move data from a primary node to a read replica node within a replication group, or between your replication group and your application\.

In\-transit encryption is optional and can only be enabled on Redis replication groups when they are created\. You enable in\-transit encryption on a replication group by setting the parameter `TransitEncryptionEnabled` \(CLI: `--transit-encryption-enabled`\) to `true` when you create the replication group\. You can do this whether you are creating the replication group using the AWS Management Console, the AWS CLI, or the ElastiCache API\. If `TransitEncryptionEnabled` is set to `true`, you must also provide a value for `CacheSubnetGroup`\.

**Important**  
The parameters `TransitEncryptionEnabled` \(CLI: `--transit-encryption-enabled` are only available when using the `CreateReplicationGroup` \(CLI: `create-replication-group` operation\.


+ [In\-Transit Encryption Overview](#in-transit-encryption-overview)
+ [In\-Transit Encryption Constraints](#in-transit-encryption-constraints)
+ [Enabling In\-Transit Encryption](#in-transit-encryption-enable)
+ [See Also](#in-transit-encryption-see-also)

## In\-Transit Encryption Overview<a name="in-transit-encryption-overview"></a>

Amazon ElastiCache in\-transit encryption is an optional feature that allows you to increase the security of your data at its most vulnerable points—when it is in transit from one location to another\. Because there is some processing needed to encrypt and decrypt the data at the endpoints, enabling in\-transit encryption can have some performance impact\. You should benchmark your data with and without in\-transit encryption to determine the performance impact for your use cases\.

ElastiCache in\-transit encryption implements the following features:

+ **Encrypted connections**—both the server and client connections are Secure Socket Layer \(SSL\) encrypted\.

+ **Encrypted replication—**data moving between a primary node and replica nodes is encrypted\.

+ **Server authentication**—clients can authenticate that they are connecting to the right server\.

+ **Client authentication**—using the Redis AUTH feature, the server can authenticate the clients\.

## In\-Transit Encryption Constraints<a name="in-transit-encryption-constraints"></a>

The following constraints on Amazon ElastiCache in\-transit encryption should be kept in mind when you plan your implementation:

+ In\-transit encryption is supported on replication groups running Redis version 3\.2\.6\. It is not supported on clusters running Memcached\.

  In\-transit encryption is supported only for replication groups running in an Amazon VPC\.

+ In\-transit encryption is only supported for replication groups running most current generation node types\. It is not supported on replication groups running on previous generation node types\. For more information, see [Supported Node Types](CacheNodes.SupportedTypes.md)

+ In\-transit encryption is enabled by setting the parameter `TransitEncryptionEnabled` to `true`\. Because the default value of `TransitEncryptionEnabled` is `false`, you must explicitly set it to `true` when creating your replication group\. If `TransitEncryptionEnabled` is set to `true`, you must also provide a value for `CacheSubnetGroup`\.

+ You can enable in\-transit encryption on a replication group only when creating the replication group\. You cannot toggle in\-transit encryption on and off by modifying a replication group\. For information on implementing in\-transit encryption on an existing replication group, see [Enabling In\-Transit Encryption](#in-transit-encryption-enable)\.

+ To connect to an in\-transit encryption enabled replication group, a database must be enabled for transport layer security \(TLS\)\. To connect to a replication group that is not in\-transit encryption enabled, the database cannot be TLS\-enabled\.

Because of the processing required to encrypt and decrypt the data at the endpoints, implementing in\-transit encryption can reduce performance\. Benchmark in\-transit encryption compared to no encryption on your own data to determine its impact on performance for your implementation\.

You can reduce the performance impact of in\-transit encryption by persisting your SSL connections, because creating new connections can be expensive\.

## Enabling In\-Transit Encryption<a name="in-transit-encryption-enable"></a>

You can enable in\-transit encryption when you create an ElastiCache for Redis replication group using the AWS Management Console, the AWS CLI, or the ElastiCache API\.

 

### Enabling In\-Transit Encryption on an Existing Cluster<a name="in-transit-encryption-enable-existing"></a>

You can only enable in\-transit encryption when you create a Redis replication group\. If you have an existing replication group on which you want to enable in\-transit encryption, do the following\.

**To enable in\-transit encryption for an existing Redis replication group**

1. Create a manual backup of the replication group\. For more information, see [Making Manual Backups](backups-manual.md)\.

1. Create a new replication group by restoring from the backup setting the engine version to 3\.2\.6 and the parameter `TransitEncryptionEnabled` to `true` \(CLI:` --transit-encryption-enabled`\)\. For more information, see [Restoring From a Backup with Optional Cluster Resizing](backups-restoring.md)\.

1. Update the endpoints in your application to the new replication group's endpoints\. For more information, see [Finding Your ElastiCache Endpoints](Endpoints.md)\.

1. Delete the old replication group\. For more information, see the following:

   + [Deleting a Cluster](Clusters.Delete.md) 

   + [Deleting a Cluster with Replicas](Replication.DeletingRepGroup.md)

 

### Enabling In\-Transit Encryption Using the AWS Management Console<a name="in-transit-encryption-enable-con"></a>

To enable in\-transit encryption when creating a replication group using the AWS Management Console, make the following selections:

+ Choose Redis as your engine\.

+ Choose engine version 3\.2\.6\.

+ Choose **Yes** from the **Encryption in\-transit** list\.

For the step\-by\-step process, see the following:

+ [Creating a Redis \(cluster mode disabled\) Cluster \(Console\)](Clusters.Create.CON.Redis.md)

+ [Creating a Redis \(cluster mode enabled\) Cluster \(Console\)](Clusters.Create.CON.RedisCluster.md)

 

### Enabling In\-Transit Encryption Using the AWS CLI<a name="in-transit-encryption-enable-cli"></a>

To enable in\-transit encryption when creating a Redis replication group using the AWS CLI, use the parameter `transit-encryption-enabled`\.

#### Enabling In\-Transit Encryption on a Redis \(cluster mode disabled\) Cluster \(CLI\)<a name="in-transit-encryption-enable-cli-redis-classic-rg"></a>

Use the AWS CLI operation `create-replication-group` and the following parameters to create a Redis replication group with replicas that has in\-transit encryption enabled:

+ `--engine` must be `redis`\.

+ `--engine-version` must be 3\.2\.6\.

+ `--transit-encryption-enabled`\. If you enable in\-transit encryption you must also provide a value for the `--cache-subnet-group` parameter\.

+ `--num-cache-clusters` must be at least 1\. The maximum value for this parameter is 6\.

For more information, see the following:

+ [Creating a Redis \(cluster mode disabled\) Cluster with Replicas from Scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.CLI)

+ [create\-replication\-group](http://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

#### Enabling In\-Transit Encryption on a Redis \(cluster mode enabled\) Cluster \(CLI\)<a name="in-transit-encryption-enable-cli-redis-cluster"></a>

Use the AWS CLI operation `create-replication-group` and the following parameters to create a Redis \(cluster mode enabled\) replication group that has in\-transit encryption enabled:

+ `--engine` must be `redis`\.

+ `--engine-version` must be 3\.2\.6\.

+ `--transit-encryption-enabled`\. If you enable in\-transit encryption you must also provide a value for the `--cache-subnet-group` parameter\.

+ Use one of the following parameter sets to specify the configuration of the replication group's node groups:

  + `--num-node-groups` to specify the number of shards \(node groups\) in this replication group\. The maximum value of this parameter is 15\.

    `--replicas-per-node-group` to specify the number of replica nodes in each node group\. The value specified here is applied to all shards in this replication group\. The maximum value of this parameter is 5\.

  + `--node-group-configuration` to specify the configuration of each shard independently\.

For more information, see the following:

+ [Creating a Redis \(cluster mode enabled\) Cluster with Replicas from Scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.CLI)

+ [create\-replication\-group](http://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

 

### Enabling In\-Transit Encryption Using the AWS API<a name="in-transit-encryption-enable-api"></a>

To enable in\-transit encryption when creating a Redis replication group using the ElastiCache API, set the parameter `TransitEncryptionEnabled` to `true` with either `CreateCacheCluster` for a single node Redis replication group, or `CreateReplicationGroup` for a replication group with read replicas\.

#### Enabling In\-Transit Encryption on a Redis \(cluster mode disabled\) Cluster \(API\)<a name="in-transit-encryption-enable-api-redis-classic-rg"></a>

Use the ElastiCache API operation `CreateReplicationGroup` and the following parameters to create a Redis \(cluster mode disabled\) replication group that has in\-transit encryption enabled:

+ `Engine` must be `redis`\.

+ `EngineVersion` must be 3\.2\.6\.

+ `TransitEncryptionEnabled` must set to `true`\.

  If `TransitEncryptionEnabled` is set to `true`, you must also provide a value for `CacheSubnetGroup`\.

+ `NumCacheClusters` must be at least 1\. The maximum value for this parameter is 6\.

For more information, see the following:

+ [Creating a Redis \(cluster mode disabled\) Cluster with Replicas from Scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.API)

+ [CreateReplicationGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)

#### Enabling In\-Transit Encryption on a Redis \(cluster mode enabled\) Cluster \(API\)<a name="in-transit-encryption-enable-api-redis-cluster"></a>

Use the ElastiCache API operation `CreateReplicationGroup` and the following parameters to create a Redis \(cluster mode enabled\) replication group that has in\-transit encryption enabled:

+ `Engine` must be `redis`\.

+ `EngineVersion` must be 3\.2\.6\.

+ `TransitEncryptionEnabled` must set to `true`\.

+ Use one of the following parameter sets to specify the configuration of the replication group's node groups:

  + `NumNodeGroups` to specify the number of shards \(node groups\) in this replication group\. The maximum value of this parameter is 15\.

    `ReplicasPerNodeGroup` to specify the number of replica nodes in each node group\. The value specified here is applied to all shards in this replication group\. The maximum value of this parameter is 5\.

  + `NodeGroupConfiguration` to specify the configuration of each shard independently\.

For more information, see the following:

+ [Creating a Redis \(cluster mode enabled\) Cluster with Replicas from Scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.API)

+ [CreateReplicationGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)

## See Also<a name="in-transit-encryption-see-also"></a>

+ [Amazon ElastiCache for Redis At\-Rest Encryption](at-rest-encryption.md)

+ [Authenticating Users with AUTH \(Redis\)](auth.md)

+ [ElastiCache and Security Groups](VPCs.md)

+ [Authentication and Access Control for Amazon ElastiCache](IAM.md)