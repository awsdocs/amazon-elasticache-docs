# ElastiCache for Redis in\-transit encryption \(TLS\)<a name="in-transit-encryption"></a>

To help keep your data secure, Amazon ElastiCache and Amazon EC2 provide mechanisms to guard against unauthorized access of your data on the server\. By providing in\-transit encryption capability, ElastiCache gives you a tool you can use to help protect your data when it is moving from one location to another\. For example, you might move data from a primary node to a read replica node within a replication group, or between your replication group and your application\.

In\-transit encryption is optional and can only be enabled on Redis replication groups when they are created\. You enable in\-transit encryption on a replication group by setting the parameter `TransitEncryptionEnabled` to `true` \(CLI: `--transit-encryption-enabled`\) when you create the replication group\. You can do this whether you are creating the replication group using the AWS Management Console, the AWS CLI, or the ElastiCache API\. If you enable in\-transit encryption, you must also provide a value for `CacheSubnetGroup`\.

**Important**  
The parameters `TransitEncryptionEnabled` \(CLI: `--transit-encryption-enabled`\) are only available when using the `CreateReplicationGroup` \(CLI: `create-replication-group`\) operation\.

**Topics**
+ [In\-transit encryption overview](#in-transit-encryption-overview)
+ [In\-transit encryption conditions](#in-transit-encryption-constraints)
+ [Enabling in\-transit encryption](#in-transit-encryption-enable)
+ [Connecting to Amazon ElastiCache for Redis nodes enabled with in\-transit encryption using redis\-cli](#connect-tls)
+ [See also](#in-transit-encryption-see-also)
+ [Authenticating users with the Redis AUTH command](auth.md)

## In\-transit encryption overview<a name="in-transit-encryption-overview"></a>

Amazon ElastiCache in\-transit encryption is an optional feature that allows you to increase the security of your data at its most vulnerable points—when it is in transit from one location to another\. Because there is some processing needed to encrypt and decrypt the data at the endpoints, enabling in\-transit encryption can have some performance impact\. You should benchmark your data with and without in\-transit encryption to determine the performance impact for your use cases\.

ElastiCache in\-transit encryption implements the following features:
+ **Encrypted connections**—both the server and client connections are Secure Socket Layer \(SSL\) encrypted\.
+ **Encrypted replication—**data moving between a primary node and replica nodes is encrypted\.
+ **Server authentication**—clients can authenticate that they are connecting to the right server\.
+ **Client authentication**—using the Redis AUTH feature, the server can authenticate the clients\.

## In\-transit encryption conditions<a name="in-transit-encryption-constraints"></a>

The following constraints on Amazon ElastiCache in\-transit encryption should be kept in mind when you plan your implementation:
+ In\-transit encryption is supported on replication groups running Redis versions 3\.2\.6, 4\.0\.10 and later\.
+ In\-transit encryption is supported only for replication groups running in an Amazon VPC\.
+ In\-transit encryption is only supported for replication groups running the following node types\.
  + R6g, R5, R4, R3
  + M6g, M5, M4, M3
  + T3, T2

  For more information, see [Supported node types](CacheNodes.SupportedTypes.md)\.
+ In\-transit encryption is enabled by explicitly setting the parameter `TransitEncryptionEnabled` to `true`\.
+ You can enable in\-transit encryption on a replication group only when creating the replication group\. You cannot toggle in\-transit encryption on and off by modifying a replication group\. For information on implementing in\-transit encryption on an existing replication group, see [Enabling in\-transit encryption](#in-transit-encryption-enable)\.
+ To connect to an in\-transit encryption enabled replication group, a database must be enabled for transport layer security \(TLS\)\. To connect to a replication group that is not in\-transit encryption enabled, the database cannot be TLS\-enabled\.

Because of the processing required to encrypt and decrypt the data at the endpoints, implementing in\-transit encryption can reduce performance\. Benchmark in\-transit encryption compared to no encryption on your own data to determine its impact on performance for your implementation\.

**Tip**  
Because creating new connections can be expensive, you can reduce the performance impact of in\-transit encryption by persisting your SSL connections\.

## Enabling in\-transit encryption<a name="in-transit-encryption-enable"></a>

You can enable in\-transit encryption when you create an ElastiCache for Redis replication group using the AWS Management Console, the AWS CLI, or the ElastiCache API\.

 

### Enabling in\-transit encryption on an existing cluster<a name="in-transit-encryption-enable-existing"></a>

You can only enable in\-transit encryption when you create a Redis replication group\. If you have an existing replication group on which you want to enable in\-transit encryption, do the following\.

**To enable in\-transit encryption for an existing Redis replication group**

1. Create a manual backup of the replication group\. For more information, see [Making manual backups](backups-manual.md)\.

1. Create a new replication group by restoring from the backup setting the engine version to 3\.2\.6, 4\.0\.10 and later, and the parameter `TransitEncryptionEnabled` to `true` \(CLI:` --transit-encryption-enabled`\)\. For more information, see [Restoring from a backup with optional cluster resizing](backups-restoring.md)\.

1. Update the endpoints in your application to the new replication group's endpoints\. For more information, see [Finding connection endpoints](Endpoints.md)\.

1. Delete the old replication group\. For more information, see the following:
   + [Deleting a cluster](Clusters.Delete.md) 
   + [Deleting a replication group](Replication.DeletingRepGroup.md)

 

### Enabling in\-transit encryption using the AWS Management Console<a name="in-transit-encryption-enable-con"></a>

To enable in\-transit encryption when creating a replication group using the AWS Management Console, make the following selections:
+ Choose Redis as your engine\.
+ Choose engine version 3\.2\.6, 4\.0\.10 or later\.
+ Choose **Yes** from the **Encryption in\-transit** list\.

For the step\-by\-step process, see the following:
+ [Creating a Redis \(cluster mode disabled\) cluster \(Console\)](GettingStarted.CreateCluster.md#Clusters.Create.CON.Redis-gs)
+ [Creating a Redis \(cluster mode enabled\) cluster \(Console\)](Clusters.Create.md#Clusters.Create.CON.RedisCluster)

 

### Enabling in\-transit encryption using the AWS CLI<a name="in-transit-encryption-enable-cli"></a>

To enable in\-transit encryption when creating a Redis replication group using the AWS CLI, use the parameter `transit-encryption-enabled`\.

#### Enabling in\-transit encryption on Redis \(Cluster Mode Disabled\) cluster \(CLI\)<a name="in-transit-encryption-enable-cli-redis-classic-rg"></a>

Use the AWS CLI operation `create-replication-group` and the following parameters to create a Redis replication group with replicas that has in\-transit encryption enabled:

**Key parameters:**
+ **\-\-engine**—Must be `redis`\.
+ **\-\-engine\-version**—Must be 3\.2\.6, 4\.0\.10 or later\.
+ **\-\-transit\-encryption\-enabled**—Required\. If you enable in\-transit encryption you must also provide a value for the `--cache-subnet-group` parameter\.
+ **\-\-num\-cache\-clusters**—Must be at least 1\. The maximum value for this parameter is six\.

For more information, see the following:
+ [Creating a Redis \(Cluster Mode Disabled\) replication group from scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.CLI)
+ [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

#### Enabling in\-transit encryption on a cluster for Redis \(Cluster Mode Enabled\) \(CLI\)<a name="in-transit-encryption-enable-cli-redis-cluster"></a>

Use the AWS CLI operation `create-replication-group` and the following parameters to create a Redis \(cluster mode enabled\) replication group that has in\-transit encryption enabled:

**Key parameters:**
+ **\-\-engine**—Must be `redis`\.
+ **\-\-engine\-version**—Must be 3\.2\.6, 4\.0\.10 or later\.
+ **\-\-transit\-encryption\-enabled**—Required\. If you enable in\-transit encryption you must also provide a value for the `--cache-subnet-group` parameter\.
+ Use one of the following parameter sets to specify the configuration of the replication group's node groups:
  + **\-\-num\-node\-groups**—Specifies the number of shards \(node groups\) in this replication group\. The maximum value of this parameter is 500\.

    **\-\-replicas\-per\-node\-group**—Specifies the number of replica nodes in each node group\. The value specified here is applied to all shards in this replication group\. The maximum value of this parameter is 5\.
  + **\-\-node\-group\-configuration**—Specifies the configuration of each shard independently\.

  

For more information, see the following:
+ [Creating a Redis \(Cluster Mode Enabled\) replication group from scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.CLI)
+ [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

 

### Enabling in\-transit encryption using the AWS API<a name="in-transit-encryption-enable-api"></a>

To enable in\-transit encryption when creating a Redis replication group using the ElastiCache API, set the parameter `TransitEncryptionEnabled` to `true` with either `CreateReplicationGroup` for a single node Redis replication group, or `CreateReplicationGroup` for a replication group with read replicas\.

#### Enabling in\-transit encryption on a cluster for Redis \(Cluster Mode Disabled\) \(API\)<a name="in-transit-encryption-enable-api-redis-classic-rg"></a>

Use the ElastiCache API operation `CreateReplicationGroup` and the following parameters to create a Redis \(cluster mode disabled\) replication group that has in\-transit encryption enabled:

**Key parameters**
+ **Engine**—Must be `redis`\.
+ **EngineVersion**—Must be 3\.2\.6, 4\.0\.10 or later\.
+ **TransitEncryptionEnabled**—Must set to `true`\.

  When `TransitEncryptionEnabled` is set to `true`, you must also provide a value for `CacheSubnetGroup`\.
+ **NumCacheClusters**—Must be at least 1\. The maximum value for this parameter is six\.

For more information, see the following:
+ [Creating a Redis \(cluster mode disabled\) replication group from scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.API)
+ [CreateReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)

#### Enabling in\-transit encryption on a cluster for Redis \(Cluster Mode Enabled\) \(API\)<a name="in-transit-encryption-enable-api-redis-cluster"></a>

Use the ElastiCache API operation `CreateReplicationGroup` and the following parameters to create a Redis \(cluster mode enabled\) replication group that has in\-transit encryption enabled:

**Key parameters**
+ **Engine**—Must be `redis`\.
+ **EngineVersion**—Must be 3\.2\.6, 4\.0\.10 or later\.
+ **TransitEncryptionEnabled**—Must set to `true`\.

  When `TransitEncryptionEnabled` is set to `true`, you must also provide a value for `CacheSubnetGroup`\.
+ Use one of the following parameter sets to specify the configuration of the replication group's node groups:
  + **NumNodeGroups**—Specifies the number of shards \(node groups\) in this replication group\. The maximum value of this parameter is 500 but can be increased to a maximum of 250 by a service limit increase request\. For more information, see [AWS service limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html?id=docs_gateway)\.

    **ReplicasPerNodeGroup**—Specifies the number of replica nodes in each node group\. The value specified here is applied to all shards in this replication group\. The maximum value of this parameter is 5\.
  + **NodeGroupConfiguration**—Specifies the configuration of each shard independently\.

  

For more information, see the following:
+ [Creating a replication group in Redis \(Cluster Mode Enabled\) from scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.API)
+ [CreateReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)

## Connecting to Amazon ElastiCache for Redis nodes enabled with in\-transit encryption using redis\-cli<a name="connect-tls"></a>



To access data from ElastiCache for Redis nodes enabled with in\-transit encryption, you use clients that work with Secure Socket Layer \(SSL\)\. You can also use redis\-cli with TLS/SSL on Amazon linux and Amazon Linux 2\. 

**To use redis\-cli to connect to a Redis cluster enabled with in\-transit encryption on Amazon Linux 2 or Amazon Linux**

1. Download and compile the redis\-cli utility\. This utility is included in the Redis software distribution\.

1. At the command prompt of your EC2 instance, type the following commands:

   Amazon Linux 2

   ```
   $ sudo yum -y install openssl-devel gcc
   $ wget http://download.redis.io/redis-stable.tar.gz
   $ tar xvzf redis-stable.tar.gz
   $ cd redis-stable
   $ make distclean
   $ make redis-cli BUILD_TLS=yes
   $ sudo install -m 755 src/redis-cli /usr/local/bin/
   ```

   Amazon Linux

   ```
   $ sudo yum install gcc jemalloc-devel openssl-devel tcl tcl-devel clang wget
   $ wget http://download.redis.io/redis-stable.tar.gz
   $ tar xvzf redis-stable.tar.gz
   $ cd redis-stable
   $ make redis-cli CC=clang BUILD_TLS=yes
   $ sudo install -m 755 src/redis-cli /usr/local/bin/
   ```

   On Amazon Linux, you may also need to run the following additional steps:

   ```
   sudo yum install clang
   CC=clang make
   sudo make install
   ```

1. After this, it is recommended that you run the optional `make-test` command\.

1. At the command prompt of your EC2 instance, type the following command, substituting the endpoint of your cluster and port for what is shown in this example\.

   ```
   redis-cli -h Primary or Configuration Endpoint --tls -p 6379
   ```

   For more information on finding the endpoint, see [Find your Node Endpoints](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/GettingStarted.ConnectToCacheNode.html#GettingStarted.FindEndpoints)\.

   The following example connects to a cluster with encryption and authentication enabled:

   ```
   redis-cli -h Primary or Configuration Endpoint --tls -a 'your-password' -p 6379
   ```

To work around this, you can use the `stunnel` command to create an SSL tunnel to the redis nodes\. You then use redis\-cli to connect to the tunnel to access data from encrypted Redis nodes\.

**To use redis\-cli to connect to a Redis cluster enabled with in\-transit encryption using stunnel**

1. Use SSH to connect to your client and install `stunnel`\.

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
      connect = primary.ssltest.wif01h.use1.cache.amazonaws.com:6379
   [redis-cli-replica]
      client = yes
      accept = 127.0.0.1:6380
      connect = ssltest-02.ssltest.wif01h.use1.cache.amazonaws.com:6379
   ```

   In this example, the config file has two connections, the `redis-cli` and the `redis-cli-replica`\. The parameters are set as follows:
   + **client** is set to yes to specify this stunnel instance is a client\.
   + **accept** is set to the client IP\. In this example, the primary is set to the Redis default 127\.0\.0\.1 on port 6379\. The replica must call a different port and set to 6380\. You can use ephemeral ports 1024–65535\. For more information, see [Ephemeral ports](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ACLs.html#VPC_ACLs_Ephemeral_Ports) in the *Amazon VPC User Guide\.*
   + **connect** is set to the Redis server endpoint\. For more information, see [Finding connection endpoints](Endpoints.md)\.

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

## See also<a name="in-transit-encryption-see-also"></a>
+ [At\-Rest Encryption in ElastiCache for Redis](at-rest-encryption.md)
+ [Authenticating users with the Redis AUTH command](auth.md)
+ [Authenticating Users with Role\-Based Access Control \(RBAC\)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Clusters.RBAC.html)
+ [Amazon VPCs and ElastiCache security](VPCs.md)
+ [Identity and access management in Amazon ElastiCache](IAM.md)