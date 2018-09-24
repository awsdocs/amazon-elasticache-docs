# Using Auto Discovery<a name="AutoDiscovery.Using"></a>

To begin using Auto Discovery, follow these steps:
+ [Step 1: Obtain the Configuration Endpoint](#AutoDiscovery.Using.ConfigEndpoint)
+ [Step 2: Download the ElastiCache Cluster Client](#AutoDiscovery.Using.ClusterClient)
+ [Step 3: Modify Your Application Program](#AutoDiscovery.Using.ModifyApp)

## Step 1: Obtain the Configuration Endpoint<a name="AutoDiscovery.Using.ConfigEndpoint"></a>

To connect to a cluster, client programs must know the cluster configuration endpoint\. See the topic [Finding a Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Memcached)

You can also use the `aws elasticache describe-cache-clusters` command with the `--show-cache-node-info` parameter:

Whatever method you use to find the cluster's endpoints, the configuration endpoint will always have **\.cfg** in its address\.

**Example Finding endpoints using the AWS CLI for ElastiCache**  
For Linux, macOS, or Unix:  

```
aws elasticache describe-cache-clusters \
    --cache-cluster-id mycluster \
    --show-cache-node-info
```
For Windows:  

```
aws elasticache describe-cache-clusters ^
    --cache-cluster-id mycluster ^
    --show-cache-node-info
```
This operation produces output similar to the following \(JSON format\):  

```
{
    "CacheClusters": [
        {
            "Engine": "memcached", 
            "CacheNodes": [
                {
                    "CacheNodeId": "0001", 
                    "Endpoint": {
                        "Port": 11211, 
                        "Address": "mycluster.fnjyzo.cfg.0001.use1.cache.amazonaws.com"
                    }, 
                    "CacheNodeStatus": "available", 
                    "ParameterGroupStatus": "in-sync", 
                    "CacheNodeCreateTime": "2016-10-12T21:39:28.001Z", 
                    "CustomerAvailabilityZone": "us-east-1e"
                }, 
                {
                    "CacheNodeId": "0002", 
                    "Endpoint": {
                        "Port": 11211, 
                        "Address": "mycluster.fnjyzo.cfg.0002.use1.cache.amazonaws.com"
                    }, 
                    "CacheNodeStatus": "available", 
                    "ParameterGroupStatus": "in-sync", 
                    "CacheNodeCreateTime": "2016-10-12T21:39:28.001Z", 
                    "CustomerAvailabilityZone": "us-east-1a"
                }
            ], 
            "CacheParameterGroup": {
                "CacheNodeIdsToReboot": [], 
                "CacheParameterGroupName": "default.memcached1.4", 
                "ParameterApplyStatus": "in-sync"
            }, 
            "CacheClusterId": "mycluster", 
            "PreferredAvailabilityZone": "Multiple", 
            "ConfigurationEndpoint": {
                "Port": 11211, 
                "Address": "mycluster.fnjyzo.cfg.use1.cache.amazonaws.com"
            }, 
            "CacheSecurityGroups": [], 
            "CacheClusterCreateTime": "2016-10-12T21:39:28.001Z", 
            "AutoMinorVersionUpgrade": true, 
            "CacheClusterStatus": "available", 
            "NumCacheNodes": 2, 
            "ClientDownloadLandingPage": "https://console.aws.amazon.com/elasticache/home#client-download:", 
            "CacheSubnetGroupName": "default", 
            "EngineVersion": "1.4.24", 
            "PendingModifiedValues": {}, 
            "PreferredMaintenanceWindow": "sat:06:00-sat:07:00", 
            "CacheNodeType": "cache.r3.large"
        }
    ]
}
```

## Step 2: Download the ElastiCache Cluster Client<a name="AutoDiscovery.Using.ClusterClient"></a>

To take advantage of Auto Discovery, client programs must use the *ElastiCache Cluster Client*\. The ElastiCache Cluster Client is available for Java, PHP, and \.NET and contains all of the necessary logic for discovering and connecting to all of your cache nodes\.

**To download the ElastiCache Cluster Client**

1. Sign in to the AWS Management Console and open the ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the ElastiCache console, choose **ElastiCache Cluster Client** then choose **Download**\.

The source code for the ElastiCache Cluster Client for Java is available at [https://github\.com/amazonwebservices/aws\-elasticache\-cluster\-client\-memcached\-for\-java](https://github.com/amazonwebservices/aws-elasticache-cluster-client-memcached-for-java)\. This library is based on the popular Spymemcached client\. The ElastiCache Cluster Client is released under the Amazon Software License [https://aws\.amazon\.com/asl](https://aws.amazon.com/asl)\. You are free to modify the source code as you see fit\. You can even incorporate the code into other open source Memcached libraries, or into your own client code\.

**Note**  
To use the ElastiCache Cluster Client for PHP, you will first need to install it on your Amazon EC2 instance\. For more information, see [Installing the ElastiCache Cluster Client for PHP](Appendix.PHPAutoDiscoverySetup.md)\.  
To use the ElastiCache Cluster Client for \.NET, you will first need to install it on your Amazon EC2 instance\. For more information, see [Installing the ElastiCache Cluster Client for \.NET](Appendix.DotNETAutoDiscoverySetup.md)\.

## Step 3: Modify Your Application Program<a name="AutoDiscovery.Using.ModifyApp"></a>

Modify your application program so that it uses Auto Discovery\. The following sections show how to use the ElastiCache Cluster Client for Java, PHP, and \.NET\. 

**Important**  
When specifying the cluster's configuration endpoint, be sure that the endpoint has "\.cfg" in its address as shown here\. Do not use a CNAME or an endpoint without "\.cfg" in it\.   

```
"mycluster.fnjyzo.cfg.use1.cache.amazonaws.com";
```
 Failure to explicitly specify the cluster's configuration endpoint results in configuring to a specific node\.

**Topics**
+ [Using the ElastiCache Cluster Client for Java](#AutoDiscovery.Using.ModifyApp.Java)
+ [Using the ElastiCache Cluster Client for PHP](#AutoDiscovery.Using.ModifyApp.PHP)
+ [Using the ElastiCache Cluster Client for \.NET](#AutoDiscovery.Using.ModifyApp.DotNET)

### Using the ElastiCache Cluster Client for Java<a name="AutoDiscovery.Using.ModifyApp.Java"></a>

The program below demonstrates how to use the ElastiCache Cluster Client to connect to a cluster configuration endpoint and add a data item to the cache\. Using Auto Discovery, the program connects to all of the nodes in the cluster without any further intervention\.

```
package com.amazon.elasticache;

import java.io.IOException;
import java.net.InetSocketAddress;

// Import the AWS-provided library with Auto Discovery support 
import net.spy.memcached.MemcachedClient;  

public class AutoDiscoveryDemo {

    public static void main(String[] args) throws IOException {
            
        String configEndpoint = "mycluster.fnjyzo.cfg.use1.cache.amazonaws.com";
        Integer clusterPort = 11211;

        MemcachedClient client = new MemcachedClient(
                                 new InetSocketAddress(configEndpoint, 
                                                       clusterPort));       
        // The client will connect to the other cache nodes automatically.

        // Store a data item for an hour.  
        // The client will decide which cache host will store this item. 
        client.set("theKey", 3600, "This is the data value");
    }
}
```

### Using the ElastiCache Cluster Client for PHP<a name="AutoDiscovery.Using.ModifyApp.PHP"></a>

The program below demonstrates how to use the ElastiCache Cluster Client to connect to a cluster configuration endpoint and add a data item to the cache\. Using Auto Discovery, the program will connect to all of the nodes in the cluster without any further intervention\.

To use the ElastiCache Cluster Client for PHP, you will first need to install it on your Amazon EC2 instance\. For more information, see [Installing the ElastiCache Cluster Client for PHP](Appendix.PHPAutoDiscoverySetup.md)

```
<?php
	
 /**
  * Sample PHP code to show how to integrate with the Amazon ElastiCache
  * Auto Discovery feature.
  */

  /* Configuration endpoint to use to initialize memcached client. 
   * This is only an example. 	*/
  $server_endpoint = "mycluster.fnjyzo.cfg.use1.cache.amazonaws.com";
  
  /* Port for connecting to the ElastiCache cluster. 
   * This is only an example 	*/
  $server_port = 11211;

 /**
  * The following will initialize a Memcached client to utilize the Auto Discovery feature.
  * 
  * By configuring the client with the Dynamic client mode with single endpoint, the
  * client will periodically use the configuration endpoint to retrieve the current cache
  * cluster configuration. This allows scaling the cache cluster up or down in number of nodes
  * without requiring any changes to the PHP application. 
  *
  * By default the Memcached instances are destroyed at the end of the request. 
  * To create an instance that persists between requests, 
  *    use persistent_id to specify a unique ID for the instance. 
  * All instances created with the same persistent_id will share the same connection. 
  * See [http://php\.net/manual/en/memcached\.construct\.php](http://php.net/manual/en/memcached.construct.php) for more information.
  */
  $dynamic_client = new Memcached('persistent-id');
  $dynamic_client->setOption(Memcached::OPT_CLIENT_MODE, Memcached::DYNAMIC_CLIENT_MODE);
  $dynamic_client->addServer($server_endpoint, $server_port);
  
  /**
  * Store the data for 60 seconds in the cluster. 
  * The client will decide which cache host will store this item.
  */  
  $dynamic_client->set('key', 'value', 60);  


 /**
  * Configuring the client with Static client mode disables the usage of Auto Discovery
  * and the client operates as it did before the introduction of Auto Discovery. 
  * The user can then add a list of server endpoints.
  */
  $static_client = new Memcached('persistent-id');
  $static_client->setOption(Memcached::OPT_CLIENT_MODE, Memcached::STATIC_CLIENT_MODE);
  $static_client->addServer($server_endpoint, $server_port);

 /**
  * Store the data without expiration. 
  * The client will decide which cache host will store this item.
  */  
  $static_client->set('key', 'value');  
  ?>
```

### Using the ElastiCache Cluster Client for \.NET<a name="AutoDiscovery.Using.ModifyApp.DotNET"></a>

\.NET client for ElastiCache is open source at [https://github\.com/awslabs/elasticache\-cluster\-config\-net](https://github.com/awslabs/elasticache-cluster-config-net)\.

 \.NET applications typically get their configurations from their config file\. The following is a sample application config file\.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <configSections>
        <section 
            name="clusterclient" 
            type="Amazon.ElastiCacheCluster.ClusterConfigSettings, Amazon.ElastiCacheCluster" />
    </configSections>

    <clusterclient>
        <!-- the hostname and port values are from step 1 above -->
        <endpoint hostname="mycluster.fnjyzo.cfg.use1.cache.amazonaws.com" port="11211" />
    </clusterclient>
</configuration>
```

The C\# program below demonstrates how to use the ElastiCache Cluster Client to connect to a cluster configuration endpoint and add a data item to the cache\. Using Auto Discovery, the program will connect to all of the nodes in the cluster without any further intervention\.

```
// *****************
// Sample C# code to show how to integrate with the Amazon ElastiCcache Auto Discovery feature.

using System;

using Amazon.ElastiCacheCluster;

using Enyim.Caching;
using Enyim.Caching.Memcached;

public class DotNetAutoDiscoveryDemo  {

    public static void Main(String[] args)  {
    
        // instantiate a new client.
        ElastiCacheClusterConfig config = new ElastiCacheClusterConfig();
        MemcachedClient memClient = new MemcachedClient(config);
        
        // Store the data for 3600 seconds (1hour) in the cluster. 
        // The client will decide which cache host will store this item.
        memClient.Store(StoreMode.Set, 3600, "This is the data value.");
        
    }  // end Main
    
}  // end class DotNetAutoDiscoverDemo
```