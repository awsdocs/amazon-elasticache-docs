# Connect to a Cluster's Node<a name="GettingStarted.ConnectToCacheNode"></a>

Before you continue, complete [Authorize Access](GettingStarted.AuthorizeAccess.md)\.

This section assumes that you've created an Amazon EC2 instance and can connect to it\. For instructions on how to do this, see the [Amazon EC2 Getting Started Guide](https://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)\. 

An Amazon EC2 instance can connect to a cluster node only if you have authorized it to do so\. For more information, see [Authorize Access](GettingStarted.AuthorizeAccess.md)\.

## Find your Node Endpoints<a name="GettingStarted.FindEndpoints"></a>

When your cluster is in the *available* state and you've authorized access to it, you can log in to an Amazon EC2 instance and connect to the cluster\. To do so, you must first determine the endpoint\.

To find your endpoints, see the relevant topic below for the engine and cluster type you're running\. When you find the endpoint you need, copy it to your clipboard for use in the next step\.
+ [Finding Connection Endpoints](Endpoints.md)
+ [Finding a Redis \(Cluster Mode Disabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Redis)—You need the primary endpoint of a replication group or the node endpoint of a standalone node\.
+ [Finding Endpoints for a Redis \(Cluster Mode Enabled\) Cluster \(Console\)](Endpoints.md#Endpoints.Find.RedisCluster)—You need the cluster's Configuration endpoint\.
+ [Finding Endpoints \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI)
+ [Finding Endpoints \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API)

## Connect to a Redis Cluster or Replication Group \(Linux\)<a name="GettingStarted.ConnectToCacheNode.Redis.Linux"></a>

Now that you have the endpoint you need, you can log in to an EC2 instance and connect to the cluster or replication group\.

In the following example, you use the *redis\-cli* utility to connect to a cluster that is not encryption enabled and running Redis\. For more information about Redis and available Redis commands, see [Redis commands](http://redis.io/commands) on the Redis website\.

**To connect to a Redis cluster that is not encryption\-enabled using *redis\-cli***

1. Connect to your Amazon EC2 instance using the connection utility of your choice\. For instructions on how to connect to an Amazon EC2 instance, see the [Amazon EC2 Getting Started Guide](https://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)\. 

1. Download and install the GNU Compiler Collection \(gcc\)\.

   At the command prompt of your EC2 instance, type the following command then, at the confirmation prompt, type **y** \.

   ```
   sudo yum install gcc
   ```

   Doing this produces output similar to the following\.

   ```
   Loaded plugins: priorities, security, update-motd, upgrade-helper
   Setting up Install Process
   Resolving Dependencies
   --> Running transaction check
   
   ...(output omitted)...
   
   Total download size: 27 M
   Installed size: 53 M
   Is this ok [y/N]: y
   Downloading Packages:
   (1/11): binutils-2.22.52.0.1-10.36.amzn1.x86_64.rpm      | 5.2 MB     00:00     
   (2/11): cpp46-4.6.3-2.67.amzn1.x86_64.rpm                | 4.8 MB     00:00     
   (3/11): gcc-4.6.3-3.10.amzn1.noarch.rpm                  | 2.8 kB     00:00     
   
   ...(output omitted)...
   
   Complete!
   ```

1. Download and compile the `redis-cli` utility\. This utility is included in the Redis software distribution\.

   At the command prompt of your EC2 instance, type the following commands:

   ```
   wget http://download.redis.io/redis-stable.tar.gz
   tar xvzf redis-stable.tar.gz
   cd redis-stable
   make distclean  // Ubuntu systems only
   make
   ```

1. At the command prompt of your EC2 instance, type the following command, substituting the endpoint of your cluster and port for what is shown in this example\.

   ```
   src/redis-cli -c -h mycachecluster.eaogs8.0001.usw2.cache.amazonaws.com -p 6379
   ```

   This results in a Redis command prompt similar to the following\.

   ```
   redis mycachecluster.eaogs8.0001.usw2.cache.amazonaws.com 6379>
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

## Connect to a Redis Cluster or Replication Group \(Windows\)<a name="GettingStarted.ConnectToCacheNode.Redis.Windows"></a>

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