# Using the ElastiCache Cluster Client for \.NET<a name="AutoDiscovery.Using.ModifyApp.DotNET"></a>

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