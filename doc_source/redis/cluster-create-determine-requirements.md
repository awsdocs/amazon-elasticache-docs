# Determine your requirements<a name="cluster-create-determine-requirements"></a>

**Topics**
+ [Memory and processor requirements](#cluster-create-determine-requirements-memory)
+ [Redis cluster configuration](#redis-cluster-configuration)
+ [Scaling requirements](#cluster-create-determine-requirements-scaling)
+ [Access requirements](#cluster-create-determine-requirements-access)
+ [Region, Availability Zone and Local Zone requirements](#cluster-create-determine-requirements-region)

**Preparation**  
Knowing the answers to the following questions helps make creating your cluster go faster:
+ Which node instance type do you need?

  For guidance on choosing an instance node type, see [Choosing your node size](nodes-select-size.md#CacheNodes.SelectSize)\.
+ Will you launch your cluster in a virtual private cloud \(VPC\) based on Amazon VPC? 
**Important**  
If you're going to launch your cluster in a VPC, make sure to create a subnet group in the same VPC before you start creating a cluster\. For more information, see [Subnets and subnet groups](SubnetGroups.md)\.  
ElastiCache is designed to be accessed from within AWS using Amazon EC2\. However, if you launch in a VPC based on Amazon VPC and your cluster is in an VPC, you can provide access from outside AWS\. For more information, see [Accessing ElastiCache resources from outside AWS](accessing-elasticache.md#access-from-outside-aws)\.
+ Do you need to customize any parameter values?

  If you do, create a custom parameter group\. For more information, see [Creating a parameter group](ParameterGroups.Creating.md)\.

   If you're running Redis, consider setting `reserved-memory` or `reserved-memory-percent`\. For more information, see [Managing Reserved Memory](redis-memory-management.md)\.
+ Do you need to create your own *security group* or *VPC security group*? 

  For more information, see [Security groups: EC2\-Classic](SecurityGroups.md) and [Security in Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html)\.
+ How do you intend to implement fault tolerance?

  For more information, see [Mitigating Failures](FaultTolerance.md)\.

**Topics**
+ [Memory and processor requirements](#cluster-create-determine-requirements-memory)
+ [Redis cluster configuration](#redis-cluster-configuration)
+ [Scaling requirements](#cluster-create-determine-requirements-scaling)
+ [Access requirements](#cluster-create-determine-requirements-access)
+ [Region, Availability Zone and Local Zone requirements](#cluster-create-determine-requirements-region)

## Memory and processor requirements<a name="cluster-create-determine-requirements-memory"></a>

The basic building block of Amazon ElastiCache is the node\. Nodes are configured singularly or in groupings to form clusters\. When determining the node type to use for your cluster, take the cluster’s node configuration and the amount of data you have to store into consideration\.

## Redis cluster configuration<a name="redis-cluster-configuration"></a>

ElastiCache for Redis clusters are comprised of from 0 to 250 shards \(also called node groups\)\. The data in a Redis cluster is partitioned across the shards in the cluster\. Your application connects with a Redis cluster using a network address called an Endpoint\. The nodes in a Redis shard fulfill one of two roles: one read/write primary and all other nodes read\-only secondaries \(also called read replicas\)\. In addition to the node endpoints, the Redis cluster itself has an endpoint called the *configuration endpoint*\. Your application can use this endpoint to read from or write to the cluster, leaving the determination of which node to read from or write to up to ElastiCache for Redis\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCacheClusters-Redis-ClustersRGs.png)

For more information, see [Managing clusters](Clusters.md)\.

## Scaling requirements<a name="cluster-create-determine-requirements-scaling"></a>

All clusters can be scaled up by creating a new cluster with the new, larger node type\. When you scale up a Redis cluster, you can seed it from a backup and avoid having the new cluster start out empty\.

For more information, see [Scaling ElastiCache for Redis clusters](Scaling.md) in this guide\.

## Access requirements<a name="cluster-create-determine-requirements-access"></a>

By design, Amazon ElastiCache clusters are accessed from Amazon EC2 instances\. Network access to an ElastiCache cluster is limited to the user account that created the cluster\. Therefore, before you can access a cluster from an Amazon EC2 instance, you must authorize the Amazon EC2 instance to access the cluster\. The steps to do this vary, depending upon whether you launched into EC2\-VPC or EC2\-Classic\.

If you launched your cluster into EC2\-VPC you need to grant network ingress to the cluster\. If you launched your cluster into EC2\-Classic you need to grant the Amazon Elastic Compute Cloud security group associated with the instance access to your ElastiCache security group\. For detailed instructions, see [Step 2: Authorize access to the cluster](GettingStarted.AuthorizeAccess.md) in this guide\.

## Region, Availability Zone and Local Zone requirements<a name="cluster-create-determine-requirements-region"></a>

Amazon ElastiCache supports all AWS regions\. By locating your ElastiCache clusters in an AWS Region close to your application you can reduce latency\. If your cluster has multiple nodes, locating your nodes in different Availability Zones or in Local Zones can reduce the impact of failures on your cluster\.

For more information, see the following:
+ [Choosing regions and availability zones](RegionsAndAZs.md)
+ [Using local zones with ElastiCache ](Local_zones.md)
+ [Mitigating Failures](FaultTolerance.md)