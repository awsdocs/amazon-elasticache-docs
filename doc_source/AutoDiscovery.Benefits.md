# Benefits of Auto Discovery<a name="AutoDiscovery.Benefits"></a>

Auto Discovery offers the following benefits:

+ When you increase the number of nodes in a cache cluster, the new nodes register themselves with the configuration endpoint and with all of the other nodes\. When you remove nodes from the cache cluster, the departing nodes deregister themselves\. In both cases, all of the other nodes in the cluster are updated with the latest cache node metadata\.

+ Cache node failures are automatically detected; failed nodes are automatically replaced\.
**Note**  
Until node replacement completes, the node will continue to fail\.

+ A client program only needs to connect to the configuration endpoint\. After that, the Auto Discovery library connects to all of the other nodes in the cluster\.

+ Client programs poll the cluster once per minute \(this interval can be adjusted if necessary\)\. If there are any changes to the cluster configuration, such as new or deleted nodes, the client receives an updated list of metadata\. Then the client connects to, or disconnects from, these nodes as needed\.

Auto Discovery is enabled on all ElastiCache Memcached cache clusters\. You do not need to reboot any of your cache nodes to use this feature\.