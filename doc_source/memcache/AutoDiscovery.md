# Automatically identify nodes in your cluster<a name="AutoDiscovery"></a>

For clusters running the Memcached engine, ElastiCache supports *Auto Discovery*—the ability for client programs to automatically identify all of the nodes in a cache cluster, and to initiate and maintain connections to all of these nodes\. 

**Note**  
Auto Discovery is added for cache clusters running on Amazon ElastiCache Memcached\.

With Auto Discovery, your application does not need to manually connect to individual cache nodes; instead, your application connects to one Memcached node and retrieves the list of nodes\. From that list your application is aware of the rest of the nodes in the cluster and can connect to any of them\. You do not need to hard code the individual cache node endpoints in your application\.

If you are using dual stack network type on your cluster, Auto Disovery will return only IPv4 or IPv6 addresses, depending on which one you select\. For more information, see [Choosing a network type](network-type.md)\.

All of the cache nodes in the cluster maintain a list of metadata about all of the other nodes\. This metadata is updated whenever nodes are added or removed from the cluster\.

**Topics**
+ [Benefits of Auto Discovery](AutoDiscovery.Benefits.md)
+ [How Auto Discovery Works](AutoDiscovery.HowAutoDiscoveryWorks.md)
+ [Using Auto Discovery](AutoDiscovery.Using.md)
+ [Connecting to Cache Nodes Manually](AutoDiscovery.Manual.md)
+ [Adding Auto Discovery To Your Client Library](AutoDiscovery.AddingToYourClientLibrary.md)
+ [ElastiCache clients with auto discovery](Clients.md)