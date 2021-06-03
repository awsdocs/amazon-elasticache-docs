# Choosing regions and availability zones<a name="RegionsAndAZs"></a>

AWS Cloud computing resources are housed in highly available data center facilities\. To provide additional scalability and reliability, these data center facilities are located in different physical locations\. These locations are categorized by *regions* and *Availability Zones*\.

AWS Regions are large and widely dispersed into separate geographic locations\. Availability Zones are distinct locations within an AWS Region that are engineered to be isolated from failures in other Availability Zones\. They provide inexpensive, low\-latency network connectivity to other Availability Zones in the same AWS Region\.

**Important**  
Each region is completely independent\. Any ElastiCache activity you initiate \(for example, creating clusters\) runs only in your current default region\.

To create or work with a cluster in a specific region, use the corresponding regional service endpoint\. For service endpoints, see [Supported regions & endpoints](#SupportedRegions)\.

![\[Image: Regions and Availability Zones\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-RegionsAndAZs.png)

*Regions and Availability Zones*

**Topics**
+ [Availability Zone considerations](#CacheNode.Memcached.AvailabilityZones)
+ [Supported regions & endpoints](#SupportedRegions)
+ [Locating your nodes](#RegionsAndAZs.AZMode)
+ [Using local zones with ElastiCache](Local_zones.md)
+ [Using Outposts](ElastiCache-Outposts.md)

## Availability Zone considerations<a name="CacheNode.Memcached.AvailabilityZones"></a>

Distributing your Memcached nodes over multiple Availability Zones within a region helps protect you from the impact of a catastrophic failure, such as a power loss within an Availability Zone\.

A Memcached cluster can have up to 300 nodes\. When you create or add nodes to your Memcached cluster, you can specify a single Availability Zone for all your nodes, allow ElastiCache to choose a single Availability Zone for all your nodes, specify the Availability Zones for each node, or allow ElastiCache to choose an Availability Zone for each node\. New nodes can be created in different Availability Zones as you add them to an existing Memcached cluster\. Once a cache node is created, its Availability Zone cannot be modified\. 

If you want a cluster in a single Availability Zone cluster to have its nodes distributed across multiple Availability Zones, ElastiCache can create new nodes in the various Availability Zones\. You can then delete some or all of the original cache nodes\. We recommend this approach\.

**To migrate Memcached nodes from a single Availability Zone to multiple availability zones**

1. Modify your cluster by creating new cache nodes in the Availability Zones where you want them\. In your request, do the following:
   + Set `AZMode` \(CLI: `--az-mode`\) to `cross-az`\.
   + Set `NumCacheNodes` \(CLI: `--num-cache-nodes`\) to the number of currently active cache nodes plus the number of new cache nodes you want to create\.
   + Set `NewAvailabilityZones` \(CLI: `--new-availability-zones`\) to a list of the zones you want the new cache nodes created in\. To let ElastiCache determine the Availability Zone for each new node, don't specify a list\.
   +  Set `ApplyImmediately` \(CLI: `--apply-immediately`\) to true\. 
**Note**  
If you are not using auto discovery, be sure to update your client application with the new cache node endpoints\.

   Before moving on to the next step, be sure the Memcached nodes are fully created and available\.

1. Modify your cluster by removing the nodes you no longer want in the original Availability Zone\. In your request, do the following:
   + Set `NumCacheNodes` \(CLI: `--num-cache-nodes`\) to the number of active cache nodes you want after this modification is applied\.
   + Set `CacheNodeIdsToRemove` \(CLI: `--nodes-to-remove`\) to a list of the cache nodes you want to remove from the cluster\.

     The number of cache node IDs listed must equal the number of currently active nodes minus the value in `NumCacheNodes`\.
   + \(Optional\) Set `ApplyImmediately` \(CLI: `--apply-immediately`\) to true\.

     If you don't set `ApplyImmediately` \(CLI: `--apply-immediately`\) to true, the node deletions will take place at your next maintenance window\.

## Supported regions & endpoints<a name="SupportedRegions"></a>

Amazon ElastiCache is available in multiple AWS Regions\. This means that you can launch ElastiCache clusters in locations that meet your requirements\. For example, you can launch in the AWS Region closest to your customers, or launch in a particular AWS Region to meet certain legal requirements\.

By default, the AWS SDKs, AWS CLI, ElastiCache API, and ElastiCache console reference the US\-West \(Oregon\) region\. As ElastiCache expands availability to new regions, new endpoints for these regions are also available to use in your HTTP requests, the AWS SDKs, AWS CLI, and the console\.

Each region is designed to be completely isolated from the other regions\. Within each region are multiple Availability Zones \(AZ\)\. By launching your nodes in different AZs you are able to achieve the greatest possible fault tolerance\. For more information on regions and Availability Zones, see [Choosing regions and availability zones](#RegionsAndAZs) at the top of this topic\.


**Regions where ElastiCache is supported**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/RegionsAndAZs.html)

Some regions support a subset of node types\. For a table of supported node types by AWS Region, see [Supported node types by AWS Region](CacheNodes.SupportedTypes.md#CacheNodes.SupportedTypesByRegion)\.

For a table of AWS products and services by region, see [Products and Services by Region](http://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

## Locating your nodes<a name="RegionsAndAZs.AZMode"></a>

Amazon ElastiCache supports locating all of a cluster's nodes in a single or multiple Availability Zones \(AZs\)\. Further, if you elect to locate your nodes in multiple AZs \(recommended\), ElastiCache enables you to either choose the AZ for each node, or allow ElastiCache to choose them for you\.

By locating the nodes in different AZs, you eliminate the chance that a failure, such as a power outage, in one AZ will cause your entire system to fail\. Testing has demonstrated that there is no significant latency difference between locating all nodes in one AZ or spreading them across multiple AZs\. 

You can specify an AZ for each node when you create a cluster or by adding nodes when you modify an existing cluster\. For more information, see the following:
+ [Creating a cluster](Clusters.Create.md)
+ [Modifying an ElastiCache cluster](Clusters.Modify.md)
+ [Adding nodes to a cluster](Clusters.AddNode.md)