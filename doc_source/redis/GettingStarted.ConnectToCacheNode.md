# Step 3: Connect to the cluster's node<a name="GettingStarted.ConnectToCacheNode"></a>

Before you continue, complete [Step 2: Authorize access to the cluster](GettingStarted.AuthorizeAccess.md)\.

This section assumes that you've created an Amazon EC2 instance and can connect to it\. For instructions on how to do this, see the [Amazon EC2 Getting Started Guide](https://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)\. 

An Amazon EC2 instance can connect to a cluster node only if you have authorized it to do so\. 

## Find your node endpoints<a name="GettingStarted.FindEndpoints"></a>

When your cluster is in the *available* state and you've authorized access to it, you can log in to an Amazon EC2 instance and connect to the cluster\. To do so, you must first determine the endpoint\.

To find your endpoints, see the relevant topic following for the engine and cluster type you're running\. When you find the endpoint you need, copy it to your clipboard for use in the next step\.
+ [Finding connection endpoints](Endpoints.md)
+ [Finding a Redis \(Cluster Mode Disabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Redis)—You need the primary endpoint of a replication group or the node endpoint of a standalone node\.
+ [Finding Endpoints for a Redis \(Cluster Mode Enabled\) Cluster \(Console\)](Endpoints.md#Endpoints.Find.RedisCluster)—You need the cluster's Configuration endpoint\.
+ [Finding Endpoints \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI)
+ [Finding Endpoints \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API)

## Connect to a Redis cluster or replication group \(Linux\)<a name="GettingStarted.ConnectToCacheNode.Redis.Linux"></a>

Now that you have the endpoint you need, you can log in to an EC2 instance and connect to the cluster or replication group\. In the following example, you use the *redis\-cli* utility to connect to a cluster\. The latest version of redis\-cli also supports SSL/TLS for connecting encryption/authentication enabled clusters\.

The following example uses Amazon EC2instances running Amazon Linux and Amazon Linux 2\. For details on installing and compiling redis\-cli with other Linux distributions, see the documentation for your specific operating system\.\.

**Note**  
This process covers testing a connection using redis\-cli utility for unplanned use only\. For a list of supported Redis clients, see the [Redis documentation](https://redis.io/)\.

### Download and install redis\-cli<a name="Download-and-install-redis-cli"></a>



1. Connect to your Amazon EC2 instance using the connection utility of your choice\. For instructions on how to connect to an Amazon EC2 instance, see the [Amazon EC2 Getting Started Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)\. 

1. Download and install redis\-cli utility by running following commands:

   Amazon Linux 2

   ```
   $sudo amazon-linux-extras install epel -y
   $sudo yum install gcc jemalloc-devel openssl-devel tcl tcl-devel -y
   $sudo wget http://download.redis.io/redis-stable.tar.gz
   $sudo tar xvzf redis-stable.tar.gz
   $cd redis-stable
   $sudo make BUILD_TLS=yes
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

## Redis\-cli alternative<a name="Redis-cli-alternative"></a>

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