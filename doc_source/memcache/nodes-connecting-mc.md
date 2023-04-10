# Connecting to nodes<a name="nodes-connecting-mc"></a>

Before attempting to connect to your Memcached cluster, you must have the endpoints for the nodes\. To find the endpoints, see the following:
+ [Finding a Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Memcached)
+ [Finding Endpoints \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI)
+ [Finding Endpoints \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API)

In the following example, you use the *telnet* utility to connect to a node that is running Memcached\.

**Note**  
For more information about Memcached and available Memcached commands, see the [Memcached](http://memcached.org/) website\.

**To connect to a node using *telnet***

1. Connect to your Amazon EC2 instance by using the connection utility of your choice\. 
**Note**  
 For instructions on how to connect to an Amazon EC2 instance, see the [Amazon EC2 Getting Started Guide](https://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)\. 

1. Download and install the *telnet* utility on your Amazon EC2 instance\. At the command prompt of your Amazon EC2 instance, type the following command and type *y* at the command prompt\.

   ```
   sudo yum install telnet
   ```

   Output similar to the following appears\.

   ```
   Loaded plugins: priorities, security, update-motd, upgrade-helper
   Setting up Install Process
   Resolving Dependencies
   --> Running transaction check
   
   ...(output omitted)...
   
   Total download size: 63 k
   Installed size: 109 k
   Is this ok [y/N]: y
   Downloading Packages:
   telnet-0.17-47.7.amzn1.x86_64.rpm                        |  63 kB     00:00  
   
   ...(output omitted)...
   
   Complete!
   ```

1. At the command prompt of your Amazon EC2 instance, type the following command, substituting the endpoint of your node for the one shown in this example\.

   ```
   telnet mycachecluster.eaogs8.0001.usw2.cache.amazonaws.com 11211
   ```

   Output similar to the following appears\.

   ```
   Trying 128.0.0.1...
   Connected to mycachecluster.eaogs8.0001.usw2.cache.amazonaws.com.
   Escape character is '^]'.
   >
   ```

1. Test the connection by running Memcached commands\.

    You are now connected to a node, and you can run Memcached commands\. The following is an example\. 

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