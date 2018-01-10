# Choosing Regions and Availability Zones<a name="RegionsAndAZs"></a>

AWS cloud computing resources are housed in highly available data center facilities\. To provide additional scalability and reliability, these data center facilities are located in different physical locations\. These locations are categorized by *regions* and *Availability Zones*\.

Regions are large and widely dispersed into separate geographic locations\. Availability Zones are distinct locations within a region that are engineered to be isolated from failures in other Availability Zones and provide inexpensive, low latency network connectivity to other Availability Zones in the same region\.

**Important**  
Each region is completely independent\. Any ElastiCache activity you initiate \(for example, creating clusters\) runs only in your current default region\.

To create or work with a cluster in a specific region, use the corresponding regional service endpoint\. For service endpoints, see [Supported Regions & Endpoints](#SupportedRegions)\.

![\[Image: Regions and Availability Zones\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-RegionsAndAZs.png)

*Regions and Availability Zones*


+ [Locating Your Nodes](#RegionsAndAZs.AZMode)
+ [Supported Regions & Endpoints](#SupportedRegions)

## Locating Your Nodes<a name="RegionsAndAZs.AZMode"></a>

Amazon ElastiCache supports locating all of a cluster's nodes in a single or multiple Availability Zones \(AZs\)\. Further, if you elect to locate your nodes in multiple AZs \(recommended\), ElastiCache enables you to either choose the AZ for each node, or allow ElastiCache to choose them for you\.

By locating the nodes in different AZs, you eliminate the chance that a failure, such as a power outage, in one AZ will cause your entire system to fail\. Testing has demonstrated that there is no significant latency difference between locating all nodes in one AZ or spreading them across multiple AZs\. 

You can specify an AZ for each node when you create a cluster or by adding nodes when you modify an existing cluster\. For more information, see:

+ [Creating a Cluster](Clusters.Create.md)

+ [Creating a Redis Cluster with Replicas](Replication.CreatingRepGroup.md)

+ [Modifying an ElastiCache Cluster](Clusters.Modify.md)

+ [Adding Nodes to a Cluster](Clusters.AddNode.md)

+ [Adding a Read Replica to a Redis Cluster](Replication.AddReadReplica.md)

## Supported Regions & Endpoints<a name="SupportedRegions"></a>

Amazon ElastiCache is available in multiple regions so that you can launch ElastiCache clusters in locations that meet your requirements, such as launching in the region closest to your customers or to meet certain legal requirements\.

By default, the AWS SDKs, AWS CLI, ElastiCache API, and ElastiCache console reference the US\-West \(Oregon\) region\. As ElastiCache expands availability to new regions, new endpoints for these regions are also available to use in your HTTP requests, the AWS SDKs, AWS CLI, and the console\.

Each region is designed to be completely isolated from the other regions\. Within each region are multiple Availability Zones \(AZ\)\. By launching your nodes in different AZs you are able to achieve the greatest possible fault tolerance\. For more information on regions and Availability Zones, go to [Choosing Regions and Availability Zones](#RegionsAndAZs) at the top of this topic\.


**Regions where ElastiCache is supported**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/RegionsAndAZs.html)

Some regions support a subset of node types\. For a table of supported node types by region, see [Supported Node Types by Region](CacheNodes.SupportedTypes.md#CacheNodes.SupportedTypesByRegion)\.

For a table of AWS products and services by region, see [Products and Services by Region](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.