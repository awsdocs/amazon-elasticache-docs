# Using the ElastiCache Cluster Client for Java<a name="AutoDiscovery.Using.ModifyApp.Java"></a>

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