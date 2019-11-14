# Connecting to Nodes<a name="nodes-connecting"></a>

Before attempting to connect to the nodes in your Redis cluster, you must have the endpoints for the nodes\. To find the endpoints, see the following:
+ [Finding a Redis \(Cluster Mode Disabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Redis)
+ [Finding Endpoints for a Redis \(Cluster Mode Enabled\) Cluster \(Console\)](Endpoints.md#Endpoints.Find.RedisCluster)
+ [Finding Endpoints \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI)
+ [Finding Endpoints \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API)

In the following example, you use the *redis\-cli* utility to connect to a cluster that is running Redis\.

**Note**  
For more information about Redis and available Redis commands, see the [http://redis\.io/commands](http://redis.io/commands) webpage\.

**To connect to a Redis cluster using the *redis\-cli***

1. Connect to your Amazon EC2 instance using the connection utility of your choice\. 
**Note**  
For instructions on how to connect to an Amazon EC2 instance, see the [Amazon EC2 Getting Started Guide](https://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)\. 

1. To build `redis-cli`, download and install the GNU Compiler Collection \(`gcc`\)\. At the command prompt of your EC2 instance, enter the following command and enter `y` at the confirmation prompt\.

   ```
   sudo yum install gcc
   ```

   Output similar to the following appears\.

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

1. Download and compile the *redis\-cli* utility\. This utility is included in the Redis software distribution\. At the command prompt of your EC2 instance, type the following commands:
**Note**  
For Ubuntu systems, before running `make`, run `make distclean`\.

   ```
   wget http://download.redis.io/redis-stable.tar.gz
   tar xvzf redis-stable.tar.gz
   cd redis-stable
   make distclean      // ubuntu systems only
   make
   ```

1. At the command prompt of your EC2 instance, type the following command, substituting the endpoint of your cluster for the one shown in this example\.

   Repeat this step for each node in your cluster that you want to connect to\.

   ```
   src/redis-cli -c -h mycachecluster.eaogs8.0001.usw2.cache.amazonaws.com -p 6379
   ```

   A Redis command prompt similar to the following appears\.

   ```
   redis mycachecluster.eaogs8.0001.usw2.cache.amazonaws.com 6379>
   ```

1. Test the connection by running Redis commands\.

    You are now connected to the cluster and can run Redis commands\. The following are some example commands with their Redis responses\. 

   ```
   set a "hello"          // Set key "a" with a string value and no expiration
   OK
   get a                  // Get value for key "a"
   "hello"
   get b                  // Get value for key "b" results in miss
   (nil)				
   set b "Good-bye" EX 5  // Set key "b" with a string value and a 5 second expiration
   get b
   "Good-bye"
                      // wait 5 seconds
   get b
   (nil)                  // key has expired, nothing returned
   quit                   // Exit from redis-cli
   ```

For connecting to nodes or clusters which have Secure Sockets Layer \(SSL\) encryption \(in\-transit enabled\), see [ElastiCache for Redis In\-Transit Encryption \(TLS\)](in-transit-encryption.md)\.