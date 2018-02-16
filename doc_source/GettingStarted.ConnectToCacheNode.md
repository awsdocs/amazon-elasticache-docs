# Step 5: Connect to a Cluster's Node<a name="GettingStarted.ConnectToCacheNode"></a>

Before you continue, be sure you have completed [Step 4: Authorize Access](GettingStarted.AuthorizeAccess.md)\.

This section assumes that you've created an Amazon EC2 instance and can connect to it\. For instructions on how to do this, go to the [Amazon EC2 Getting Started Guide](http://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)\. 

An Amazon EC2 instance can connect to a cluster node only if you have authorized it to do so\. For more information, see [Step 4: Authorize Access](GettingStarted.AuthorizeAccess.md)\.

## Step 5\.1: Find your Node Endpoints<a name="GettingStarted.FindEndpoints"></a>

Once a cluster is in the **available** state and you've authorized access to it, you can log in to an Amazon EC2 instance and connect to a node in the cluster\. To do so, you must first determine the node endpoint\.

To find your node's endpoints, see the relevant topic\. When you find the endpoint you need, copy it to your clipboard for use in Step 5\.2\.

+ [Finding Your ElastiCache Endpoints](Endpoints.md)

+ [Finding a Memcached Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Memcached)

+ [Finding a Redis \(cluster mode disabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Redis)

+ [Finding a Redis \(cluster mode enabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.RedisCluster)

+ [Finding Endpoints \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI)

+ [Finding Endpoints \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API)

## Step 5\.2: Connect to a Memcached Node<a name="GettingStarted.ConnectToCacheNode.Memcached"></a>

Once your cluster is in the *available* state and you've authorized access to it, you can log in to an Amazon EC2 instance and connect to the cluster's node\. To do so, you must first determine the node endpoint\. Because this is a single\-node Redis \(cluster mode disabled\) cluster, this endpoint is used for both read and write operations\.

**To find the endpoint for a single\-node Redis \(cluster mode disabled\) cluster**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Redis**\.

1. From the list of Redis clusters, choose the box to the left of the single\-node Redis \(cluster mode disabled\) cluster you just created \(1 in the graphic\)\.

1. In the cluster's details section, find the **Primary Endpoint** \(2 in the graphic\)\.

1. To the right of **Primary Endpoint**, locate and highlight the endpoint \(3 in the graphic\) and copy it to your clipboard for use in Step 5\.2\.

   The form of the endpoint is in the format `cluster-name.xxxxxx.node-id.region-and-az.cache.amazonaws.com:port`, as shown here: 

   ```
   redis-01.l9gh21.0001.usw2.cache.amazonaws.com:6379
   
   ...(output omitted)...
   
   Total download size: 63 k
   Installed size: 109 k
   Is this ok [y/N]: y
   Downloading Packages:
   telnet-0.17-47.7.amzn1.x86_64.rpm                        |  63 kB     00:00  
   
   ...(output omitted)...
   
   Complete>
   ```

1. At the command prompt of your Amazon EC2 instance, type the following command, substituting the endpoint of your node and port for those shown in this example\.

   ```
   telnet mycachecluster.eaogs8.0001.usw2.cache.amazonaws.com 11211
   ```

   This will produce output similar to the following\.

   ```
   Trying 128.0.0.1...
   Connected to mycachecluster.eaogs8.0001.usw2.cache.amazonaws.com.
   Escape character is '^]'.
   >
   ```

1. Run Memcached commands\.

   You are now connected to the cluster and can run Memcached commands like the following\.

   ```
   set a 0 0 5      // Set key "a" with no expiration and 5 byte value
   hello            // Set value as "hello"
   STORED
   get a            // Get value for key "a"
   VALUE a 0 5
   hello
   END
   get b            // Get value for key "b" results in miss
   END
   >
   ```

## Step 5\.2: Connect to a Redis Cluster or Replication Group<a name="GettingStarted.ConnectToCacheNode.Redis"></a>

Now that you have the endpoint you need, you can log in to an EC2 instance and connect to the cache node\. The procedure depends on the engine that you are using:

In the following example, you use the *redis\-cli* utility to connect to a cluster that is running Redis\.

**Note**  
For more information about Redis and available Redis commands, see [Redis commands](http://redis.io/commands) webpage\.

**To connect to a Redis cluster using *redis\-cli***

1. Connect to your Amazon EC2 instance using the connection utility of your choice\. 
**Note**  
For instructions on how to connect to an Amazon EC2 instance, see the [Amazon EC2 Getting Started Guide](http://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)\. 

1. Before you can build *redis\-cli*, you will need to download and install the GNU Compiler Collection \(*gcc*\)\. At the command prompt of your EC2 instance, type the following command and type *y* at the confirmation prompt\.

   ```
   sudo yum install gcc
   ```

   This will produce output similar to the following\.

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
                          // wait 5 seconds
   get b
   (nil)                  // key has expired, nothing returned
   quit                   // Exit from redis-cli
   ```
