# Determine Your Requirements<a name="cluster-create-determine-requirements"></a>

**Topics**
+ [Memory and Processor Requirements](#cluster-create-determine-requirements-memory)
+ [Memcached Cluster Configuration](#memcached-cluster-configuration)
+ [Scaling Requirements](#cluster-create-determine-requirements-scaling)
+ [Access Requirements](#cluster-create-determine-requirements-access)
+ [Region and Availability Zone Requirements](#cluster-create-determine-requirements-region)

**Preparation**  
Knowing the answers to the following questions helps make creating your cluster go faster:
+ Which node instance type do you need?

  For guidance on choosing an instance node type, see [Choosing Your Memcached Node Size](nodes-select-size.md#CacheNodes.SelectSize)\.
+ Will you launch your cluster in a virtual private cloud \(VPC\) based on Amazon VPC? 
**Important**  
If you're going to launch your cluster in a VPC, make sure to create a subnet group in the same VPC before you start creating a cluster\. For more information, see [Subnets and Subnet Groups](SubnetGroups.md)\.  
ElastiCache is designed to be accessed from within AWS using Amazon EC2\. However, if you launch in a VPC based on Amazon VPC and your cluster is in an VPC, you can provide access from outside AWS\. For more information, see [Accessing ElastiCache Resources from Outside AWS](accessing-elasticache.md#access-from-outside-aws)\.
+ Do you need to customize any parameter values?

  If you do, create a custom parameter group\. For more information, see [Creating a Parameter Group](ParameterGroups.Creating.md)\.
+ Do you need to create your own *security group* or *VPC security group*? 

  For more information, see [Security Groups: EC2\-Classic](SecurityGroups.md) and [Security in Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html)\.
+ How do you intend to implement fault tolerance?

  For more information, see [Mitigating Failures](FaultTolerance.md)\.

**Topics**
+ [Memory and Processor Requirements](#cluster-create-determine-requirements-memory)
+ [Memcached Cluster Configuration](#memcached-cluster-configuration)
+ [Scaling Requirements](#cluster-create-determine-requirements-scaling)
+ [Access Requirements](#cluster-create-determine-requirements-access)
+ [Region and Availability Zone Requirements](#cluster-create-determine-requirements-region)

## Memory and Processor Requirements<a name="cluster-create-determine-requirements-memory"></a>

The basic building block of Amazon ElastiCache is the node\. Nodes are configured singularly or in groupings to form clusters\. When determining the node type to use for your cluster, take the cluster’s node configuration and the amount of data you have to store into consideration\.

The Memcached engine is multi\-threaded, so a node’s number of cores impacts the compute power available to the cluster\.

## Memcached Cluster Configuration<a name="memcached-cluster-configuration"></a>

ElastiCache for Memcached clusters are comprised of from 1 to 20 nodes\. The data in a Memcached cluster is partitioned across the nodes in the cluster\. Your application connects with a Memcached cluster using a network address called an Endpoint\. Each node in a Memcached cluster has its own endpoint which your application uses to read from or write to the specific node\. In addition to the node endpoints, the Memcached cluster itself has an endpoint called the *configuration endpoint*\. Your application can use this endpoint to read from or write to the cluster, leaving the determination of which node to read from or write to up to [Automatically Identify Nodes in your Memcached Cluster](AutoDiscovery.md)\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-Cluster-Memcached.png)

For more information, see [Managing Your ElastiCache Clusters](Clusters.md)\.

## Scaling Requirements<a name="cluster-create-determine-requirements-scaling"></a>

All clusters can be scaled up by creating a new cluster with the new, larger node type\. When you scale up a Memcached cluster\. the new cluster starts out empty\.

Amazon ElastiCache for Memcached clusters can be scaled out or in\. To scale a Memcached cluster out or in you merely add or remove nodes from the cluster\. If you have enabled Automatic Discovery and your application is connecting to the cluster’s configuration endpoint, you do not need to make any changes in your application when you add or remove nodes\.

For more information, see [Scaling ElastiCache for Memcached Clusters](Scaling.md) in this guide\.

## Access Requirements<a name="cluster-create-determine-requirements-access"></a>

By design, Amazon ElastiCache clusters are accessed from Amazon EC2 instances\. Network access to an ElastiCache cluster is limited to the user account that created the cluster\. Therefore, before you can access a cluster from an Amazon EC2 instance, you must authorize the Amazon EC2 instance to access the cluster\. The steps to do this vary, depending upon whether you launched into EC2\-VPC or EC2\-Classic\.

If you launched your cluster into EC2\-VPC you need to grant network ingress to the cluster\. If you launched your cluster into EC2\-Classic you need to grant the Amazon Elastic Compute Cloud security group associated with the instance access to your ElastiCache security group\. For detailed instructions, see [Step 2: Authorize Access](GettingStarted.AuthorizeAccess.md) in this guide\.

## Region and Availability Zone Requirements<a name="cluster-create-determine-requirements-region"></a>

Amazon ElastiCache supports all AWS regions\. By locating your ElastiCache clusters in an AWS Region close to your application you can reduce latency\. If your cluster has multiple nodes, locating your nodes in different Availability Zones can reduce the impact of failures on your cluster\.

For more information, see the following:
+ [Choosing Regions and Availability Zones](RegionsAndAZs.md)
+ [Mitigating Failures](FaultTolerance.md)