# Step 4: Connect to the cluster's node<a name="GettingStarted.ConnectToCacheNode"></a>

Before you continue, complete [Step 3: Authorize access to the cluster](GettingStarted.AuthorizeAccess.md)\.

This section assumes that you've created an Amazon EC2 instance and can connect to it\. For instructions on how to do this, see the [Amazon EC2 Getting Started Guide](https://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)\.

An Amazon EC2 instance can connect to a cluster node only if you have authorized it to do so\.

## Find your node endpoints<a name="GettingStarted.FindEndpoints"></a>

When your cluster is in the *available* state and you've authorized access to it, you can log in to an Amazon EC2 instance and connect to the cluster\. To do so, you must first determine the endpoint\.

### Finding a Redis \(Cluster Mode Disabled\) Cluster's Endpoints \(Console\)<a name="Endpoints.Find.Redis-gs"></a>

If a Redis \(cluster mode disabled\) cluster has only one node, the node's endpoint is used for both reads and writes\. If the cluster has multiple nodes, there are three types of endpoints; the *primary endpoint*, the *reader endpoint* and the *node endpoints*\.

The primary endpoint is a DNS name that always resolves to the primary node in the cluster\. The primary endpoint is immune to changes to your cluster, such as promoting a read replica to the primary role\. For write activity, we recommend that your applications connect to the primary endpoint instead of connecting directly to the primary\.

A reader endpoint will evenly split incoming connections to the endpoint between all read replicas in a ElastiCache for Redis cluster\. Additional factors such as when the application creates the connections or how the application \(re\)\-uses the connections will determine the traffic distribution\. Reader endpoints keep up with cluster changes in real\-time as replicas are added or removed\. You can place your ElastiCache for Redis cluster’s multiple read replicas in different AWS Availability Zones \(AZ\) to ensure high availability of reader endpoints\.

**Note**
A reader endpoint is not a load balancer\. It is a DNS record that will resolve to an IP address of one of the replica nodes in a round robin fashion\.

For read activity, applications can also connect to any node in the cluster\. Unlike the primary endpoint, node endpoints resolve to specific endpoints\. If you make a change in your cluster, such as adding or deleting a replica, you must update the node endpoints in your application\.

**To find a Redis \(cluster mode disabled\) cluster's endpoints**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Redis**\.

   The clusters screen will appear with a list of Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\) clusters\. Choose the cluster you created in the [Creating a Redis \(cluster mode disabled\) cluster \(Console\)](GettingStarted.CreateCluster.md#Clusters.Create.CON.Redis-gs) section\.

1. To find the cluster's Primary and/or Reader endpoints, choose the box to the left of cluster's name\.
![\[Image: Primary endpoint for a Redis (cluster mode disabled) cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/Reader-Endpoint.png)

   *Primary and Reader endpoints for a Redis \(cluster mode disabled\) cluster*

   If there is only one node in the cluster, there is no primary endpoint and you can continue at the next step\.

1. If the Redis \(cluster mode disabled\) cluster has replica nodes, you can find the cluster's replica node endpoints by choosing the cluster's name\.

   The nodes screen appears with each node in the cluster, primary and replicas, listed with its endpoint\.
![\[Image: Node endpoints for a Redis (cluster mode disabled) cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-Endpoints-Redis-Node.png)

   *Node endpoints for a Redis \(cluster mode disabled\) cluster*

1. To copy an endpoint to your clipboard:

   1. One endpoint at a time, find then highlight the endpoint you want to copy\.

   1. Right\-click the highlighted endpoint, then choose **Copy** from the context menu\.

   The highlighted endpoint is now copied to your clipboard\. For information on using the endpoint to connect to a node, see [Connecting to nodes](nodes-connecting.md)\.

A Redis \(cluster mode disabled\) primary endpoint looks something like the following\. There is a difference depending upon whether or not In\-Transit encryption is enabled\.

**In\-transit encryption not enabled**

```
clusterName.xxxxxx.nodeId.regionAndAz.cache.amazonaws.com:port

redis-01.7abc2d.0001.usw2.cache.amazonaws.com:6379
```

**In\-transit encryption enabled**

```
master.clusterName.xxxxxx.regionAndAz.cache.amazonaws.com:port

master.ncit.ameaqx.use1.cache.amazonaws.com:6379
```

Now that you have the endpoint you need, copy it to your clipboard for use in connecting to the cluster in the next step\.

To further explore how to find your endpoints, see the relevant topics for the engine and cluster type you're running\.
+ [Finding connection endpoints](Endpoints.md)
+ [Finding Endpoints for a Redis \(Cluster Mode Enabled\) Cluster \(Console\)](Endpoints.md#Endpoints.Find.RedisCluster)—You need the cluster's Configuration endpoint\.
+ [Finding Endpoints \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI)
+ [Finding Endpoints \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API)

## Connect to a Redis cluster or replication group \(Linux\)<a name="GettingStarted.ConnectToCacheNode.Redis.Linux"></a>

Now that you have the endpoint you need, you can log in to an EC2 instance and connect to the cluster or replication group\. In the following example, you use the *redis\-cli* utility to connect to a cluster\. The latest version of redis\-cli also supports SSL/TLS for connecting encryption/authentication enabled clusters\.

The following example uses Amazon EC2 instances running Amazon Linux and Amazon Linux 2\. For details on installing and compiling redis\-cli with other Linux distributions, see the documentation for your specific operating system\.\.

**Note**
This process covers testing a connection using redis\-cli utility for unplanned use only\. For a list of supported Redis clients, see the [Redis documentation](https://redis.io/)\. For examples of using the AWS SDKs with ElastiCache, see [Getting Started with ElastiCache and AWS SDKs](ElastiCache-Getting-Started-Tutorials.md)\.

### Download and install redis\-cli<a name="Download-and-install-redis-cli"></a>



1. Connect to your Amazon EC2 instance using the connection utility of your choice\. For instructions on how to connect to an Amazon EC2 instance, see the [Amazon EC2 Getting Started Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)\.

1. Download and install redis\-cli utility by running following commands:

   Amazon Linux 2

   ```bash
   sudo amazon-linux-extras install epel -y
   sudo yum install gcc jemalloc-devel openssl-devel tcl tcl-devel -y
   sudo wget http://download.redis.io/redis-stable.tar.gz
   sudo tar xvzf redis-stable.tar.gz
   cd redis-stable
   sudo make BUILD_TLS=yes
   ```

   Amazon Linux

   ```
   $sudo yum install gcc jemalloc-devel openssl-devel tcl tcl-devel clang wget
   $sudo wget http://download.redis.io/redis-stable.tar.gz
   $sudo tar xvzf redis-stable.tar.gz
   $cd redis-stable
   $sudo CC=clang make BUILD_TLS=yes
   ```
**Note**
If the cluster you are connecting to isn't encrypted, you don't need the `Build_TLS=yes` option\.

### Connecting to a cluster mode disabled unencrypted\-cluster<a name="Connecting-to-a-cluster-mode-disabled-unencrypted-cluster"></a>

1. Run the following command to connect to the cluster and replace *cluster\-endpoint* and *port number* with the endpoint of your cluster and your port number\. \(The default port for Redis is 6379\.\)

   ```
   src/redis-cli -h cluster-endpoint -c -p port number
   ```
**Note**
In the preceding command, option \-c enables cluster mode following [\-ASK and \-MOVED redirections](https://redis.io/topics/cluster-spec)\.

   The result in a Redis command prompt looks similar to the following:

   ```
   cluster-endpoint:port number
   ```

1. You can now run Redis commands\. Note that redirection occurs because you enabled it using the \-c option\. If redirection isn't enabled, the command returns the MOVED error\. For more information on the MOVED error, see [Redis cluster specification](https://redis.io/topics/cluster-spec)\.

   ```
   set x Hi
   -> Redirected to slot [16287] located at 172.31.28.122:6379
   OK
   set y Hello
   OK
   get y
   "Hello"
   set z Bye
   -> Redirected to slot [8157] located at 172.31.9.201:6379
   OK
   get z
   "Bye"
   get x
   -> Redirected to slot [16287] located at 172.31.28.122:6379
   "Hi"
   ```

### Connecting to an Encryption/Authentication enabled cluster<a name="Connecting-to-an-Encryption-Authentication-enabled-cluster"></a>

By default, redis\-cli uses an unencrypted TCP connection when connecting to Redis\. The option `BUILD_TLS=yes` enables SSL/TLS at the time of redis\-cli compilation as shown in the preceding [Download and install redis\-cli](#Download-and-install-redis-cli) section\. Enabling AUTH is optional\. However, you must enable encryption in\-transit in order to enable AUTH\. For more details on ElastiCache encryption and authentication, see [ElastiCache for Redis in\-transit encryption \(TLS\)](in-transit-encryption.md)\.

**Note**
You can use the option `--tls` with redis\-cli to connect to both cluster mode enabled and disabled encrypted clusters\. If a cluster has an AUTH token set, then you can use the option `-a` to provide an AUTH password\.

In the following examples, be sure to replace *cluster\-endpoint* and *port number* with the endpoint of your cluster and your port number\. \(The default port for Redis is 6379\.\)

**Connect to cluster mode disabled encrypted clusters**

The following example connects to an encryption and authentication enabled cluster:

```
src/redis-cli -h cluster-endpoint --tls -a your-password -p port number
```

The following example connects to a cluster that has only encryption enabled:

```
src/redis-cli -h cluster-endpoint --tls -p port number
```

**Connect to cluster mode enabled encrypted clusters**

The following example connects to an encryption and authentication enabled cluster:

```
src/redis-cli -h cluster-endpoint --tls -a your-password -p port number
```

The following example connects to a cluster that has only encryption enabled:

```
src/redis-cli -h cluster-endpoint --tls -p port number
```

After you connect to the cluster, you can run the Redis commands as shown in the preceding examples for unencrypted clusters\.

### Redis\-cli alternative<a name="Redis-cli-alternative"></a>

If the cluster isn't cluster mode enabled and you need to make a connection to the cluster for a short test but without going through the redis\-cli compilation, you can use telnet or openssl\. In the following example commands, be sure to replace *cluster\-endpoint* and *port number* with the endpoint of your cluster and your port number\. \(The default port for Redis is 6379\.\)

The following example connects to an encryption and/or authentication enabled cluster mode disabled cluster:

```
openssl s_client -connect cluster-endpoint:port number
```

If the cluster has a password set, first connect to the cluster\. After connecting, authenticate the cluster using the following command, then press the `Enter` key\. In the following example, replace *your\-password* with the password for your cluster\.

```
Auth your-password
```

The following example connects to a cluster mode disabled cluster that doesn't have encryption or authentication enabled:

```
telnet cluster-endpoint port number
```

## Connect to a Redis cluster or replication group \(Windows\)<a name="GettingStarted.ConnectToCacheNode.Redis.Windows"></a>

In order to connect to the Redis Cluster from an EC2 Windows instance using the Redis CLI, you must download the *redis\-cli* package and use *redis\-cli\.exe* to connect to the Redis Cluster from an EC2 Windows instance\.

In the following example, you use the *redis\-cli* utility to connect to a cluster that is not encryption enabled and running Redis\. For more information about Redis and available Redis commands, see [Redis commands](http://redis.io/commands) on the Redis website\.

**To connect to a Redis cluster that is not encryption\-enabled using *redis\-cli***

1. Connect to your Amazon EC2 instance using the connection utility of your choice\. For instructions on how to connect to an Amazon EC2 instance, see the [Amazon EC2 Getting Started Guide](https://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)\.

1. Copy and paste the link [https://github.com/microsoftarchive/redis/releases/download/win-3.0.504/Redis-x64-3.0.504.zip](https://github.com/microsoftarchive/redis/releases/download/win-3.0.504/Redis-x64-3.0.504.zip) in an Internet browser to download the zip file for the Redis client from the available release at GitHub [https://github.com/microsoftarchive/redis/releases/tag/win-3.0.504](https://github.com/microsoftarchive/redis/releases/tag/win-3.0.504)

   Extract the zip file to you desired folder/path\.

   Open the Command Prompt and change to the Redis directory and run the command `c:\Redis>redis-cli -h Redis_Cluster_Endpoint -p 6379`\.

   For example:

   ```
   c:\Redis>redis-cli -h cmd.xxxxxxx.ng.0001.usw2.cache.amazonaws.com -p 6379
   ```

1. Run Redis commands\.

    You are now connected to the cluster and can run Redis commands like the following\.

   ```
   set a "hello"          // Set key "a" with a string value and no expiration
   OK
   get a                  // Get value for key "a"
   "hello"
   get b                  // Get value for key "b" results in miss
   (nil)
   set b "Good-bye" EX 5  // Set key "b" with a string value and a 5 second expiration
   "Good-bye"
   get b                  // Get value for key "b"
   "Good-bye"
                          // wait >= 5 seconds
   get b
   (nil)                  // key has expired, nothing returned
   quit                   // Exit from redis-cli
   ```

## Connecting to a cluster mode disabled unencrypted\-cluster<a name="Connecting-to-a-cluster-mode-disabled-unencrypted-cluster"></a>

1. Run the following command to connect to the cluster and replace *cluster\-endpoint* and *port number* with the endpoint of your cluster and your port number\. \(The default port for Redis is 6379\.\)

   ```
   src/redis-cli -h cluster-endpoint -c -p port number
   ```
**Note**
In the preceding command, option \-c enables cluster mode following [\-ASK and \-MOVED redirections](https://redis.io/topics/cluster-spec)\.

   The result in a Redis command prompt looks similar to the following:

   ```
   cluster-endpoint:port number
   ```

1. You can now run Redis commands\. Note that redirection occurs because you enabled it using the \-c option\. If redirection isn't enabled, the command returns the MOVED error\. For more information on the MOVED error, see [Redis cluster specification](https://redis.io/topics/cluster-spec)\.

   ```
   set x Hi
   -> Redirected to slot [16287] located at 172.31.28.122:6379
   OK
   set y Hello
   OK
   get y
   "Hello"
   set z Bye
   -> Redirected to slot [8157] located at 172.31.9.201:6379
   OK
   get z
   "Bye"
   get x
   -> Redirected to slot [16287] located at 172.31.28.122:6379
   "Hi"
   ```

## Connecting to an Encryption/Authentication enabled cluster<a name="Connecting-to-an-Encryption-Authentication-enabled-cluster"></a>

By default, redis\-cli uses an unencrypted TCP connection when connecting to Redis\. The option `BUILD_TLS=yes` enables SSL/TLS at the time of redis\-cli compilation as shown in the preceding [Download and install redis\-cli](#Download-and-install-redis-cli) section\. Enabling AUTH is optional\. However, you must enable encryption in\-transit in order to enable AUTH\. For more details on ElastiCache encryption and authentication, see [ElastiCache for Redis in\-transit encryption \(TLS\)](in-transit-encryption.md)\.

**Note**
You can use the option `--tls` with redis\-cli to connect to both cluster mode enabled and disabled encrypted clusters\. If a cluster has an AUTH token set, then you can use the option `-a` to provide an AUTH password\.

In the following examples, be sure to replace *cluster\-endpoint* and *port number* with the endpoint of your cluster and your port number\. \(The default port for Redis is 6379\.\)

**Connect to cluster mode disabled encrypted clusters**

The following example connects to an encryption and authentication enabled cluster:

```
src/redis-cli -h cluster-endpoint --tls -a your-password -p port number
```

The following example connects to a cluster that has only encryption enabled:

```
src/redis-cli -h cluster-endpoint --tls -p port number
```

**Connect to cluster mode enabled encrypted clusters**

The following example connects to an encryption and authentication enabled cluster:

```
src/redis-cli -h cluster-endpoint --tls -a your-password -p port number
```

The following example connects to a cluster that has only encryption enabled:

```
src/redis-cli -h cluster-endpoint --tls -p port number
```

After you connect to the cluster, you can run the Redis commands as shown in the preceding examples for unencrypted clusters\.