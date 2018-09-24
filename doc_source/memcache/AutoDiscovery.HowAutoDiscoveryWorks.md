# How Auto Discovery Works<a name="AutoDiscovery.HowAutoDiscoveryWorks"></a>

**Topics**
+ [Connecting to Cache Nodes](#AutoDiscovery.HowAutoDiscoveryWorks.Connecting)
+ [Normal Cluster Operations](#AutoDiscovery.HowAutoDiscoveryWorks.NormalOps)
+ [Other Operations](#AutoDiscovery.HowAutoDiscoveryWorks.OtherOps)

This section describes how client applications use ElastiCache Cluster Client to manage cache node connections, and interact with data items in the cache\.

## Connecting to Cache Nodes<a name="AutoDiscovery.HowAutoDiscoveryWorks.Connecting"></a>

From the application's point of view, connecting to the cluster configuration endpoint is no different from connecting directly to an individual cache node\. The following sequence diagram shows the process of connecting to cache nodes\. 

![\[Connecting to Cache Nodes\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/autodiscovery_cluster_membership_refresh-diagram.png)


**Process of Connecting to Cache Nodes**  

|  |  | 
| --- |--- |
|  ![\[1\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/callouts/1.png) | The application resolves the configuration endpoint's DNS name\. Because the configuration endpoint maintains CNAME entries for all of the cache nodes, the DNS name resolves to one of the nodes; the client can then connect to that node\. | 
|  ![\[2\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/callouts/2.png) | The client requests the configuration information for all of the other nodes\. Since each node maintains configuration information for all of the nodes in the cluster, any node can pass configuration information to the client upon request\. | 
|  ![\[3\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/callouts/3.png) | The client receives the current list of cache node hostnames and IP addresses\. It can then connect to all of the other nodes in the cluster\. | 

**Note**  
The client program refreshes its list of cache node hostnames and IP addresses once per minute\. This polling interval can be adjusted if necessary\.

## Normal Cluster Operations<a name="AutoDiscovery.HowAutoDiscoveryWorks.NormalOps"></a>

When the application has connected to all of the cache nodes, ElastiCache Cluster Client determines which nodes should store individual data items, and which nodes should be queried for those data items later\. The following sequence diagram shows the process of normal cluster operations\.

![\[Normal Cluster Operations\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/autodiscovery_normal_cache_usage-diagram.png)


**Process of Normal Cluster Operations**  

|  |  | 
| --- |--- |
|  ![\[1\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/callouts/1.png) | The application issues a get request for a particular data item, identified by its key\. | 
|  ![\[2\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/callouts/2.png) | The client uses a hashing algorithm against the key to determine which cache node contains the data item\. | 
|  ![\[3\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/callouts/3.png) | The data item is requested from the appropriate node\. | 
|  ![\[4\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/callouts/4.png) | The data item is returned to the application\. | 

## Other Operations<a name="AutoDiscovery.HowAutoDiscoveryWorks.OtherOps"></a>

There may arise situations where there is a change in the cluster due to adding an additional node to accommodate additional demand, deleting a node to save money during periods of reduced demand, or replacing a node due to a node failure of one sort or another\.

When there is a change in the cluster that requires a metadata update to the cluster's endpoints, that change is made to all nodes at the same time\. Thus the metadata in any given node is consistent with the metadata in all of the other nodes in the cluster\.

In each of these cases, the metadata is consistent among all the nodes at all times since the metadata is updated at the same time for all nodes in the cluster\. You should always use the configuration endpoint to obtain the endpoints of the various nodes in the cluster\. By using the configuration endpoint, you ensure that you will not be obtaining endpoint data from a node that “disappears” on you\.

### Adding a Node<a name="AutoDiscovery.HowAutoDiscoveryWorks.OtherOps.AddNode"></a>

During the time that the node is being spun up, its endpoint is not included in the metadata\. As soon as the node is available, it is added to the metadata of each of the cluster’s nodes\. In this scenario, the metadata is consistent among all the nodes and you will be able to interact with the new node only after it is available\. Prior to the node being available, you will not know about it and will interact with the nodes in your cluster the same as though the new node does not exist\.

### Deleting a Node<a name="AutoDiscovery.HowAutoDiscoveryWorks.OtherOps.DelNode"></a>

When a node is removed, its endpoint is first removed from the metadata and then the node is removed from the cluster\. In this scenario the metadata in all the nodes is consistent and there is no time in which it will contain the endpoint for the node to be removed while the node is not available\. During the node removal time it is not reported in the metadata and so your application will only be interacting with the n\-1 remaining nodes, as though the node does not exist\.

### Replacing a Node<a name="AutoDiscovery.HowAutoDiscoveryWorks.OtherOps.ReplaceNode"></a>

If a node fails, ElastiCache takes down that node and spins up a replacement\. The replacement process takes a few minutes\. During this time the metadata in all the nodes still shows the endpoint for the failed node, but any attempt to interact with the node will fail\. Therefore, your logic should always include retry logic\.