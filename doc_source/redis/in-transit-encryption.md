# ElastiCache for Redis In\-Transit Encryption \(TLS\)<a name="in-transit-encryption"></a>

To help keep your data secure, Amazon ElastiCache and Amazon EC2 provide mechanisms to guard against unauthorized access of your data on the server\. By providing in\-transit encryption capability, ElastiCache gives you a tool you can use to help protect your data when it is moving from one location to another\. For example, you might move data from a primary node to a read replica node within a replication group, or between your replication group and your application\.

In\-transit encryption is optional and can only be enabled on Redis replication groups when they are created\. You enable in\-transit encryption on a replication group by setting the parameter `TransitEncryptionEnabled` to `true` \(CLI: `--transit-encryption-enabled`\) when you create the replication group\. You can do this whether you are creating the replication group using the AWS Management Console, the AWS CLI, or the ElastiCache API\. If you enable in\-transit encryption, you must also provide a value for `CacheSubnetGroup`\.

**Important**  
The parameters `TransitEncryptionEnabled` \(CLI: `--transit-encryption-enabled`\) are only available when using the `CreateReplicationGroup` \(CLI: `create-replication-group`\) operation\.

**Topics**
+ [In\-Transit Encryption Overview](#in-transit-encryption-overview)
+ [In\-Transit Encryption Conditions](#in-transit-encryption-constraints)
+ [Enabling In\-Transit Encryption](#in-transit-encryption-enable)
+ [Connecting to Amazon ElastiCache for Redis Nodes Enabled with In\-Transit Encryption Using redis\-cli](#connect-tls)
+ [See Also](#in-transit-encryption-see-also)
+ [Authenticating Users with the Redis AUTH Command](auth.md)

## In\-Transit Encryption Overview<a name="in-transit-encryption-overview"></a>

Amazon ElastiCache in\-transit encryption is an optional feature that allows you to increase the security of your data at its most vulnerable points—when it is in transit from one location to another\. Because there is some processing needed to encrypt and decrypt the data at the endpoints, enabling in\-transit encryption can have some performance impact\. You should benchmark your data with and without in\-transit encryption to determine the performance impact for your use cases\.

ElastiCache in\-transit encryption implements the following features:
+ **Encrypted connections**—both the server and client connections are Secure Socket Layer \(SSL\) encrypted\.
+ **Encrypted replication—**data moving between a primary node and replica nodes is encrypted\.
+ **Server authentication**—clients can authenticate that they are connecting to the right server\.
+ **Client authentication**—using the Redis AUTH feature, the server can authenticate the clients\.

## In\-Transit Encryption Conditions<a name="in-transit-encryption-constraints"></a>

The following constraints on Amazon ElastiCache in\-transit encryption should be kept in mind when you plan your implementation:
+ In\-transit encryption is supported on replication groups running Redis versions 3\.2\.6, 4\.0\.10 and later\.
+ In\-transit encryption is supported only for replication groups running in an Amazon VPC\.
+ In\-transit encryption is only supported for replication groups running the following node types\.
  + R5, R4, R3
  + M5, M4, M3
  + T3, T2

  For more information, see [Supported Node Types](CacheNodes.SupportedTypes.md)\.
+ In\-transit encryption is enabled by explicitly setting the parameter `TransitEncryptionEnabled` to `true`\.
+ You can enable in\-transit encryption on a replication group only when creating the replication group\. You cannot toggle in\-transit encryption on and off by modifying a replication group\. For information on implementing in\-transit encryption on an existing replication group, see [Enabling In\-Transit Encryption](#in-transit-encryption-enable)\.
+ To connect to an in\-transit encryption enabled replication group, a database must be enabled for transport layer security \(TLS\)\. To connect to a replication group that is not in\-transit encryption enabled, the database cannot be TLS\-enabled\.

Because of the processing required to encrypt and decrypt the data at the endpoints, implementing in\-transit encryption can reduce performance\. Benchmark in\-transit encryption compared to no encryption on your own data to determine its impact on performance for your implementation\.

**Tip**  
Because creating new connections can be expensive, you can reduce the performance impact of in\-transit encryption by persisting your SSL connections\.

## Enabling In\-Transit Encryption<a name="in-transit-encryption-enable"></a>

You can enable in\-transit encryption when you create an ElastiCache for Redis replication group using the AWS Management Console, the AWS CLI, or the ElastiCache API\.

 

### Enabling In\-Transit Encryption on an Existing Cluster<a name="in-transit-encryption-enable-existing"></a>

You can only enable in\-transit encryption when you create a Redis replication group\. If you have an existing replication group on which you want to enable in\-transit encryption, do the following\.

**To enable in\-transit encryption for an existing Redis replication group**

1. Create a manual backup of the replication group\. For more information, see [Making Manual Backups](backups-manual.md)\.

1. Create a new replication group by restoring from the backup setting the engine version to 3\.2\.6, 4\.0\.10 and later, and the parameter `TransitEncryptionEnabled` to `true` \(CLI:` --transit-encryption-enabled`\)\. For more information, see [Restoring From a Backup with Optional Cluster Resizing](backups-restoring.md)\.

1. Update the endpoints in your application to the new replication group's endpoints\. For more information, see [Finding Connection Endpoints](Endpoints.md)\.

1. Delete the old replication group\. For more information, see the following:
   + [Deleting a Cluster](Clusters.Delete.md) 
   + [Deleting a Replication Group](Replication.DeletingRepGroup.md)

 

### Enabling In\-Transit Encryption Using the AWS Management Console<a name="in-transit-encryption-enable-con"></a>

To enable in\-transit encryption when creating a replication group using the AWS Management Console, make the following selections:
+ Choose Redis as your engine\.
+ Choose engine version 3\.2\.6, 4\.0\.10 or later\.
+ Choose **Yes** from the **Encryption in\-transit** list\.

For the step\-by\-step process, see the following:
+ [Creating a Cluster Mode Disabled Cluster \(Console\)](Clusters.Create.CON.Redis.md)
+ [Creating a Redis \(Cluster Mode Enabled\) Cluster \(Console\)](Clusters.Create.CON.RedisCluster.md)

 

### Enabling In\-Transit Encryption Using the AWS CLI<a name="in-transit-encryption-enable-cli"></a>

To enable in\-transit encryption when creating a Redis replication group using the AWS CLI, use the parameter `transit-encryption-enabled`\.

#### Enabling In\-Transit Encryption on Redis \(Cluster Mode Disabled\) Cluster \(CLI\)<a name="in-transit-encryption-enable-cli-redis-classic-rg"></a>

Use the AWS CLI operation `create-replication-group` and the following parameters to create a Redis replication group with replicas that has in\-transit encryption enabled:

**Key Parameters:**
+ **\-\-engine**—Must be `redis`\.
+ **\-\-engine\-version**—Must be 3\.2\.6, 4\.0\.10 or later\.
+ **\-\-transit\-encryption\-enabled**—Required\. If you enable in\-transit encryption you must also provide a value for the `--cache-subnet-group` parameter\.
+ **\-\-num\-cache\-clusters**—Must be at least 1\. The maximum value for this parameter is six\.

For more information, see the following:
+ [Creating a Redis \(Cluster Mode Disabled\) Replication Group from Scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.CLI)
+ [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

#### Enabling In\-Transit Encryption on a Cluster for Redis \(Cluster Mode Enabled\) \(CLI\)<a name="in-transit-encryption-enable-cli-redis-cluster"></a>

Use the AWS CLI operation `create-replication-group` and the following parameters to create a Redis \(cluster mode enabled\) replication group that has in\-transit encryption enabled:

**Key Parameters:**
+ **\-\-engine**—Must be `redis`\.
+ **\-\-engine\-version**—Must be 3\.2\.6, 4\.0\.10 or later\.
+ **\-\-transit\-encryption\-enabled**—Required\. If you enable in\-transit encryption you must also provide a value for the `--cache-subnet-group` parameter\.
+ Use one of the following parameter sets to specify the configuration of the replication group's node groups:
  + **\-\-num\-node\-groups**—Specifies the number of shards \(node groups\) in this replication group\. The maximum value of this parameter is 90\.

    **\-\-replicas\-per\-node\-group**—Specifies the number of replica nodes in each node group\. The value specified here is applied to all shards in this replication group\. The maximum value of this parameter is 5\.
  + **\-\-node\-group\-configuration**—Specifies the configuration of each shard independently\.

For more information, see the following:
+ [Creating a Redis \(Cluster Mode Enabled\) Replication Group from Scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.CLI)
+ [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

 

### Enabling In\-Transit Encryption Using the AWS API<a name="in-transit-encryption-enable-api"></a>

To enable in\-transit encryption when creating a Redis replication group using the ElastiCache API, set the parameter `TransitEncryptionEnabled` to `true` with either `CreateCacheCluster` for a single node Redis replication group, or `CreateReplicationGroup` for a replication group with read replicas\.

#### Enabling In\-Transit Encryption on a Cluster for Redis \(Cluster Mode Disabled\) \(API\)<a name="in-transit-encryption-enable-api-redis-classic-rg"></a>

Use the ElastiCache API operation `CreateReplicationGroup` and the following parameters to create a Redis \(cluster mode disabled\) replication group that has in\-transit encryption enabled:

**Key Parameters**
+ **Engine**—Must be `redis`\.
+ **EngineVersion**—Must be 3\.2\.6, 4\.0\.10 or later\.
+ **TransitEncryptionEnabled**—Must set to `true`\.

  When `TransitEncryptionEnabled` is set to `true`, you must also provide a value for `CacheSubnetGroup`\.
+ **NumCacheClusters**—Must be at least 1\. The maximum value for this parameter is six\.

For more information, see the following:
+ [Creating a Redis \(cluster mode disabled\) Replication Group from Scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.API)
+ [CreateReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)

#### Enabling In\-Transit Encryption on a Cluster for Redis \(Cluster Mode Enabled\) \(API\)<a name="in-transit-encryption-enable-api-redis-cluster"></a>

Use the ElastiCache API operation `CreateReplicationGroup` and the following parameters to create a Redis \(cluster mode enabled\) replication group that has in\-transit encryption enabled:

**Key Parameters**
+ **Engine**—Must be `redis`\.
+ **EngineVersion**—Must be 3\.2\.6, 4\.0\.10 or later\.
+ **TransitEncryptionEnabled**—Must set to `true`\.

  When `TransitEncryptionEnabled` is set to `true`, you must also provide a value for `CacheSubnetGroup`\.
+ Use one of the following parameter sets to specify the configuration of the replication group's node groups:
  + **NumNodeGroups**—Specifies the number of shards \(node groups\) in this replication group\. The maximum value of this parameter is 90 but can be increased to a maximum of 250 by a service limit increase request\. For more information, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html?id=docs_gateway)\.

    **ReplicasPerNodeGroup**—Specifies the number of replica nodes in each node group\. The value specified here is applied to all shards in this replication group\. The maximum value of this parameter is 5\.
  + **NodeGroupConfiguration**—Specifies the configuration of each shard independently\.

For more information, see the following:
+ [Creating a Replication Group in Redis \(Cluster Mode Enabled\) from Scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.API)
+ [CreateReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)

## Connecting to Amazon ElastiCache for Redis Nodes Enabled with In\-Transit Encryption Using redis\-cli<a name="connect-tls"></a>

To access data from ElastiCache for Redis nodes enabled with in\-transit encryption, you use clients that work with Secure Socket Layer \(SSL\)\. However, redis\-cli doesn't support SSL or Transport Layer Security \(TLS\)\. 

To work around this, you can use the `stunnel` command to create an SSL tunnel to the redis nodes\. You then use redis\-cli to connect to the tunnel to access data from encrypted Redis nodes\.

**To use redis\-cli to connect to a Redis cluster enabled with in\-transit encryption**

1. From an SSH client, install `stunnel`\.

   ```
   sudo yum install stunnel
   ```

1. Run the following command to create and edit file `'/etc/stunnel/redis-cli.conf'` simultaneously to add a ElastiCache for Redis cluster endpoint to one or more connection parameters, using provided output below as template:\.

   ```
   vi /etc/stunnel/redis-cli.conf
   
   				
   fips = no
   setuid = root
   setgid = root
   pid = /var/run/stunnel.pid
   debug = 7 
   delay = yes
   options = NO_SSLv2
   options = NO_SSLv3
   [redis-cli]
      client = yes
      accept = 127.0.0.1:6379
      connect = master.ssltest.wif01h.use1.cache.amazonaws.com:6379
   [redis-cli-replica]
      client = yes
      accept = 127.0.0.1:6380
      connect = ssltest-02.ssltest.wif01h.use1.cache.amazonaws.com:6379
   ```

   In this example, the config file has two connections, the `redis-cli` and the `redis-cli-replica`\. The parameters are set as follows:
   + **client** is set to yes to specify this stunnel instance is a client\.
   + **accept** is set to the client IP\. In this example, the master is set to the Redis default 127\.0\.0\.1 on port 6379\. The replica must call a different port and set to 6380\. You can use ephemeral ports 1024–65535\. For more information, see [Ephemeral Ports](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ACLs.html#VPC_ACLs_Ephemeral_Ports) in the *Amazon VPC User Guide\.*
   + **connect** is set to the Redis server endpoint\. For more information, see [Finding Connection Endpoints](Endpoints.md)\.

1. Start `stunnel`\.

   ```
   sudo stunnel /etc/stunnel/redis-cli.conf
   ```

   Use the `netstat` command to confirm that the tunnels started\.

   ```
   sudo netstat -tulnp | grep -i stunnel
   				
   tcp        0      0 127.0.0.1:6379              0.0.0.0:*                   LISTEN      3189/stunnel        
   tcp        0      0 127.0.0.1:6380              0.0.0.0:*                   LISTEN      3189/stunnel
   ```

1. Connect to the encrypted Redis node using the local endpoint of the tunnel\.
   + If no AUTH password was used during ElastiCache for Redis cluster creation, this example uses the redis\-cli to connect to the ElastiCache for Redis server using complete path for redis\-cli, on Amazon Linux: 

     ```
     /home/ec2-user/redis-stable/src/redis-cli -h localhost -p 6379
     ```

     If AUTH password was used during Redis cluster creation, this example uses redis\-cli to connect to the Redis server using complete path for redis\-cli, on Amazon Linux: 

     ```
      /home/ec2-user/redis-stable/src/redis-cli -h localhost -p 6379 -a my-secret-password
     ```

   OR
   + Change directory to redis\-stable and do the following:

     If no AUTH password was used during ElastiCache for Redis cluster creation, this example uses the redis\-cli to connect to the ElastiCache for Redis server using complete path for redis\-cli, on Amazon Linux: 

     ```
     src/redis-cli -h localhost -p 6379
     ```

     If AUTH password was used during Redis cluster creation, this example uses redis\-cli to connect to the Redis server using complete path for redis\-cli, on Amazon Linux: 

     ```
     src/redis-cli -h localhost -p 6379 -a my-secret-password	
     ```

   This example uses Telnet to connect to the Redis server\.

   ```
   telnet localhost 6379
   			
   Trying 127.0.0.1...
   Connected to localhost.
   Escape character is '^]'.
   auth MySecretPassword
   +OK
   get foo
   $3
   bar
   ```

1. To stop and close the SSL tunnels, `pkill` the stunnel process\.

   ```
   sudo pkill stunnel
   ```

## See Also<a name="in-transit-encryption-see-also"></a>
+ [At\-Rest Encryption in ElastiCache for Redis](at-rest-encryption.md)
+ [Authenticating Users with the Redis AUTH Command](auth.md)
+ [](Clusters.RBAC.md)
+ [Amazon VPCs and ElastiCache Security](VPCs.md)
+ [Identity and Access Management in Amazon ElastiCache](IAM.md)