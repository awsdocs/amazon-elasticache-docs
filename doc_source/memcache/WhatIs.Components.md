# ElastiCache for Memcached Components and Features<a name="WhatIs.Components"></a>

Following, you can find an overview of the major components of an Amazon ElastiCache for Memcached deployment\.

**Topics**
+ [ElastiCache Nodes](#WhatIs.Components.Nodes)
+ [ElastiCache for Memcached Clusters](#WhatIs.Components.Clusters)
+ [AWS Regions and Availability Zones](#WhatIs.RegionsAndAZs)
+ [ElastiCache for Memcached Endpoints](#WhatIs.Components.Endpoints)
+ [ElastiCache Parameter Groups](#WhatIs.Components.ParameterGroups)
+ [ElastiCache Security](#WhatIs.Components.Security)
+ [ElastiCache Security Groups](#WhatIs.Components.SecurityGroups)
+ [ElastiCache Subnet Groups](#WhatIs.Components.SubnetGroups)
+ [ElastiCache for Memcached Events](#WhatIs.Components.Events)

## ElastiCache Nodes<a name="WhatIs.Components.Nodes"></a>

A *node* is the smallest building block of an ElastiCache deployment\. A node can exist in isolation from or in some relationship to other nodes\.

A node is a fixed\-size chunk of secure, network\-attached RAM\. Each node runs an instance of Memcached\. If necessary, you can scale the nodes in a cluster up or down to a different instance type\. For more information, see [Scaling ElastiCache for Memcached Clusters](Scaling.md)\.

Every node within a cluster is the same instance type and runs the same cache engine\. Each cache node has its own Domain Name Service \(DNS\) name and port\. Multiple types of cache nodes are supported, each with varying amounts of associated memory\. For a list of supported node instance types, see [Supported Node Types](CacheNodes.SupportedTypes.md)\.

You can purchase nodes on a pay\-as\-you\-go basis, where you only pay for your use of a node\. Or you can purchase reserved nodes at a significantly reduced hourly rate\. If your usage rate is high, purchasing reserved nodes can save you money\. Suppose that your cluster is almost always in use, and you occasionally add nodes to handle use spikes\. In this case, you can purchase a number of reserved nodes to run most of the time and purchase pay\-as\-you\-go nodes for the times you occasionally need to add nodes\. For more information on reserved nodes, see [ElastiCache Reserved Nodes](CacheNodes.Reserved.md)\.

The Memcached engine supports Auto Discovery\. *Auto Discovery* is the ability for client programs to automatically identify all of the nodes in a cache cluster, and to initiate and maintain connections to all of these nodes\. With Auto Discovery, your application doesn't need to manually connect to individual nodes\. Instead, your application connects to a configuration endpoint\. The configuration endpoint DNS entry contains the CNAME entries for each of the cache node endpoints\. Thus, by connecting to the configuration endpoint, your application immediately has information about all of the nodes in the cluster and can connect to all of them\. You don't need to hard\-code the individual cache node endpoints in your application\. For more information on Auto Discovery, see [Automatically Identify Nodes in your Memcached Cluster](AutoDiscovery.md)\.

For more information on nodes, see [Managing Nodes](CacheNodes.md)\.

## ElastiCache for Memcached Clusters<a name="WhatIs.Components.Clusters"></a>

A Memcached *cluster* is a logical grouping of one or more [ElastiCache Nodes](#WhatIs.Components.Nodes)\. Data is partitioned across the nodes in a Memcached cluster\.

Many ElastiCache operations are targeted at clusters:
+ Creating a cluster
+ Modifying a cluster
+ Deleting a cluster
+ Viewing the elements in a cluster
+ Adding or removing cost allocation tags to and from a cluster

For more detailed information, see the following related topics:
+ [Managing Your ElastiCache Clusters](Clusters.md) and [Managing Nodes](CacheNodes.md)

  Information about clusters, nodes, and related operations\.
+ [AWS Service Limits: Amazon ElastiCache](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_elasticache)

  Information about ElastiCache limits, such as the maximum number of nodes or clusters\.

  If you need to exceed these limits, make your request using the [Amazon ElastiCache Cache Node request form](https://aws.amazon.com/contact-us/elasticache-node-limit-request/)\.
+ [Mitigating Failures](FaultTolerance.md)

  Information about improving the fault tolerance of your clusters\.

### Typical Cluster Configurations<a name="WhatIs.Components.Clusters.TypicalConfigurations"></a>

Memcached supports up to 100 nodes per customer for each AWS Region with each cluster having 1–20 nodes\. You partition your data across the nodes in a Memcached cluster\.

When you run the Memcached engine, clusters can be made up of 1–20 nodes\. You partition your database across the nodes\. Your application reads and writes to each node's endpoint\. For more information, see [Automatically Identify Nodes in your Memcached Cluster](AutoDiscovery.md)\.

For improved fault tolerance, locate your Memcached nodes in various Availability Zones \(AZs\) within the cluster's AWS Region\. That way, a failure in one AZ has minimal impact upon your entire cluster and application\. For more information, see [Mitigating Failures](FaultTolerance.md)\.

As demand upon your Memcached cluster changes, you can scale out or in by adding or removing nodes, which repartitions your data across the new number of nodes\. When you partition your data, we recommend using consistent hashing\. For more information about consistent hashing, see [Configuring Your ElastiCache Client for Efficient Load Balancing](BestPractices.LoadBalancing.md)\. In the following diagram, you can see examples of single node and multiple node Memcached clusters\.

![\[Image: Memcached clusters: single node and multiple node clusters\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-Cluster-Memcached.png)

## AWS Regions and Availability Zones<a name="WhatIs.RegionsAndAZs"></a>

Amazon ElastiCache for Memcached is available in multiple AWS Regions around the world\. Thus, you can launch ElastiCache clusters in the locations that meet your business requirements\. For example, you can launch in the AWS Region closest to your customers or to meet certain legal requirements\.

By default, the AWS SDKs, AWS CLI, ElastiCache API, and ElastiCache console reference the US\-West \(Oregon\) region\. As ElastiCache expands availability to new AWS Regions, new endpoints for these AWS Regions are also available to use in your HTTP requests, the AWS SDKs, AWS CLI, and ElastiCache console\.

Each AWS Region is designed to be completely isolated from the other AWS Regions\. Within each are multiple Availability Zones\. By launching your nodes in different Availability Zones, you can achieve the greatest possible fault tolerance\. For more information about AWS Regions and Availability Zones, see [Choosing Regions and Availability Zones](RegionsAndAZs.md)\.

![\[Image: Regions and Availability Zones\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-RegionsAndAZs.png)

For information on AWS Regions supported by ElastiCache and their endpoints, see [Supported Regions & Endpoints](RegionsAndAZs.md#SupportedRegions)\.

## ElastiCache for Memcached Endpoints<a name="WhatIs.Components.Endpoints"></a>

An *endpoint* is the unique address your application uses to connect to an ElastiCache node or cluster\.

Each node in a Memcached cluster has its own endpoint\. The cluster also has an endpoint called the *configuration endpoint*\. If you enable Auto Discovery and connect to the configuration endpoint, your application automatically *knows* each node endpoint, even after adding or removing nodes from the cluster\. For more information, see [Automatically Identify Nodes in your Memcached Cluster](AutoDiscovery.md)\.

For more information, see [Finding Connection Endpoints](Endpoints.md)\.

## ElastiCache Parameter Groups<a name="WhatIs.Components.ParameterGroups"></a>

Cache parameter groups are an easy way to manage runtime settings for supported engine software\. Parameters are used to control memory usage, eviction policies, item sizes, and more\. An ElastiCache parameter group is a named collection of engine\-specific parameters that you can apply to a cluster\. By doing this, you make sure that all of the nodes in that cluster are configured in exactly the same way\.

For a list of supported parameters, their default values, and which ones can be modified, see [DescribeEngineDefaultParameters](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEngineDefaultParameters.html) \([describe\-engine\-default\-parameters](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-engine-default-parameters.html)\)\.

For more detailed information on ElastiCache parameter groups, see [Configuring Engine Parameters Using Parameter Groups](ParameterGroups.md)\.

## ElastiCache Security<a name="WhatIs.Components.Security"></a>

For enhanced security, ElastiCache node access is restricted to applications running on whitelisted Amazon EC2 instances\. You can control the Amazon EC2 instances that can access your cluster by using security groups\.

By default, all new ElastiCache clusters are launched in an Amazon Virtual Private Cloud \(Amazon VPC\) environment\. You can use *subnet groups* to grant cluster access from Amazon EC2 instances running on specific subnets\. If you choose to run your cluster outside of Amazon VPC, you can create *security groups* to authorize Amazon EC2 instances running within specific Amazon EC2 security groups\.

## ElastiCache Security Groups<a name="WhatIs.Components.SecurityGroups"></a>

**Note**  
ElastiCache security groups are only applicable to clusters that are not running in an Amazon Virtual Private Cloud \(Amazon VPC\) environment\. If you are running your ElastiCache nodes in an Amazon VPC, you control access to your cache clusters with Amazon VPC security groups, which are different from ElastiCache security groups\.  
 For more information on using ElastiCache in an Amazon VPC, see [Amazon VPCs and ElastiCache Security](VPCs.md)\.

ElastiCache allows you to control access to your clusters using security groups\. A security group acts like a firewall, controlling network access to your cluster\. By default, network access to your clusters is turned off\. If you want your applications to access your cluster, you must explicitly enable access from hosts in specific Amazon EC2 security groups\. After ingress rules are configured, the same rules apply to all clusters associated with that security group\.

To allow network access to your cluster, first create a security group\. Then use the [AuthorizeCacheSecurityGroupIngress](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_AuthorizeCacheSecurityGroupIngress.html) API action or the [authorize\-cache\-security\-group\-ingress](https://docs.aws.amazon.com/cli/latest/reference/elasticache/authorize-cache-security-group-ingress.html) AWS CLI command to authorize the desired Amazon EC2 security group\. Doing this in turn specifies the Amazon EC2 instances allowed\. You can associate the security group with your cluster at the time of creation, or by using the ElastiCache management console or the [ModifyCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html) or \([modify\-cache\-cluster](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-cluster.html)\) AWS CLI for ElastiCache command\.

**Important**  
IP\-range based access control is currently not enabled for clusters\. All clients to a cluster must be within the Amazon EC2 network, and authorized via security groups as described previously\.

For more information about security groups, see [Security Groups: EC2\-Classic](SecurityGroups.md)\.

## ElastiCache Subnet Groups<a name="WhatIs.Components.SubnetGroups"></a>

A subnet group is a collection of subnets \(typically private\) that you can designate for your clusters running in an Amazon Virtual Private Cloud \(Amazon VPC\) environment\.

If you create a cluster in an Amazon VPC, then you must specify a cache subnet group\. ElastiCache uses that cache subnet group to choose a subnet and IP addresses within that subnet to associate with your cache nodes\.

For more information about cache subnet group usage in an Amazon VPC environment, see [Amazon VPCs and ElastiCache Security](VPCs.md), [Step 2: Authorize Access](GettingStarted.AuthorizeAccess.md), and [Subnets and Subnet Groups](SubnetGroups.md)\.

## ElastiCache for Memcached Events<a name="WhatIs.Components.Events"></a>

When significant events happen on a cache cluster, ElastiCache sends notification to a specific Amazon SNS topic\. Significant events can include such things as a failure to add a node, success in adding a node, the modification of a security group, and others\. By monitoring for key events, you can know the current state of your clusters and, depending upon the event, take corrective action\.

For more information on ElastiCache events, see [Monitoring ElastiCache Events](ECEvents.md)\.