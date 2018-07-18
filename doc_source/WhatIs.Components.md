# ElastiCache Components and Features<a name="WhatIs.Components"></a>

In the topics in this section, you can find an overview of the major components of an Amazon ElastiCache deployment\.


+ [ElastiCache Nodes](#WhatIs.Components.Nodes)
+ [ElastiCache Shards \(Redis\)](#WhatIs.Components.Shards)
+ [ElastiCache Clusters](#WhatIs.Components.Clusters)
+ [ElastiCache Replication \(Redis\)](#WhatIs.Components.ReplicationGroups)
+ [Regions and Availability Zones](#WhatIs.RegionsAndAZs)
+ [ElastiCache Endpoints](#WhatIs.Components.Endpoints)
+ [ElastiCache Parameter Groups](#WhatIs.Components.ParameterGroups)
+ [ElastiCache Security](#WhatIs.Components.Security)
+ [ElastiCache Security Groups](#WhatIs.Components.SecurityGroups)
+ [ElastiCache Subnet Groups](#WhatIs.Components.SubnetGroups)
+ [ElastiCache Backups/Snapshots \(Redis\)](#WhatIs.Components.Snapshots)
+ [ElastiCache Events](#WhatIs.Components.Events)

## ElastiCache Nodes<a name="WhatIs.Components.Nodes"></a>

A *node* is the smallest building block of an ElastiCache deployment\. A node can exist in isolation from or in some relationship to other nodes\.

A node is a fixed\-size chunk of secure, network\-attached RAM\. Each node runs an instance of either Memcached or Redis, depending on which was chosen when you created your cluster\. If necessary, you can scale the nodes in a cluster up or down to a different instance type\. For more information, see [Scaling](Scaling.md)\.

Every node within a cluster is the same instance type and runs the same cache engine\. Each cache node has its own Domain Name Service \(DNS\) name and port\. Multiple types of cache nodes are supported, each with varying amounts of associated memory\. For a list of supported node instance types, see [Supported Node Types](CacheNodes.SupportedTypes.md)\.

You can purchase nodes on a pay\-as\-you\-go basis, where you only pay for your use of a node, or, you can purchase reserved nodes at a significantly reduced hourly rate\. If your usage rate is high, purchasing reserved nodes could save you money\. If your cluster is almost always in use and you occasionally add nodes to handle use spikes, you can purchase a number of reserved nodes to run most of the time, and purchase pay\-as\-you\-go nodes for the times you occasionally need to add nodes\. For more information on reserved nodes, see [ElastiCache Reserved Nodes](CacheNodes.Reserved.md)\.

The Memcached engine supports Auto Discovery—the ability for client programs to automatically identify all of the nodes in a cache cluster, and to initiate and maintain connections to all of these nodes\. With Auto Discovery, your application does not need to manually connect to individual nodes; instead, your application connects to a configuration endpoint\. The configuration endpoint DNS entry contains the CNAME entries for each of the cache node endpoints; thus, by connecting to the configuration endpoint, you application immediately *knows* about all of the nodes in the cluster and can connect to all of them\. You do not need to hard code the individual cache node endpoints in your application\. For more information on Auto Discovery, see [Node Auto Discovery \(Memcached\)](AutoDiscovery.md)\.

For more information on nodes, see [ElastiCache Nodes](CacheNodes.md)\.

## ElastiCache Shards \(Redis\)<a name="WhatIs.Components.Shards"></a>

A Redis *shard* \(called a *node group* in the API and CLI\) is a grouping of 1 to 6 related nodes\. A Redis \(cluster mode disabled\) cluster always has one shard\. A Redis \(cluster mode enabled\) cluster can have from 1 to 15 shards\.

A *multiple node shard* implements replication by have one read/write primary node and 1 to 5 replica nodes\. For more information, see [ElastiCache Replication \(Redis\)](Replication.md)\.

![\[Image: Redis shard configurations\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCacheClusters-CSN-RedisShards.png)

*Redis shard configurations*

For more information on shards, see [Shards \(Redis\)](Shards.md)\.

## ElastiCache Clusters<a name="WhatIs.Components.Clusters"></a>

A Redis *cluster* is a logical grouping of one or more [ElastiCache Shards \(Redis\)](#WhatIs.Components.Shards)\. Data is partitioned across the shards in a Redis \(cluster mode enabled\) cluster\.

A Memcached *cluster* is a logical grouping of one or more [ElastiCache Nodes](#WhatIs.Components.Nodes)\. Data is partitioned across the nodes in a Memcached cluster\.

Many ElastiCache operations are targeted at clusters:

+ Creating a cluster

+ Modifying a cluster

+ Taking snapshots of a cluster \(all versions of Redis\)

+ Deleting a cluster

+ Viewing the elements in a cluster

+ Adding or removing cost allocation tags to and from a cluster

For more detailed information, see the following related topics:

+ [ElastiCache Clusters](Clusters.md) and [ElastiCache Nodes](CacheNodes.md)

  Information about clusters, nodes, and related operations\.

+ [AWS Service Limits: Amazon ElastiCache](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_elasticache)

  Information about ElastiCache limits, such as the maximum number of nodes or clusters\.

  If you need to exceed these limits, make your request using the [Amazon ElastiCache Cache Node request form](https://aws.amazon.com/contact-us/elasticache-node-limit-request/)\.

+ [Mitigating Failures](FaultTolerance.md)

  Information about improving the fault tolerance of your clusters and replication groups\.

### Typical Cluster Configurations<a name="WhatIs.Components.Clusters.TypicalConfigurations"></a>

Depending on the engine you choose, possible cluster configurations differ\.

Memcached supports up to 100 nodes per customer per region with each cluster having 1 to 20 nodes\. You can partition your data across the nodes in a Memcached cluster\.

A Redis cluster contains 1 to 15 shards \(in the API, called node groups\), each of which is a partition of your data\. Redis \(cluster mode disabled\) always has just one shard\.

Following are typical cluster configurations for the Memcached and Redis engines\.

#### Memcached Clusters<a name="WhatIs.Components.Clusters.TypicalConfigurations.Memcached"></a>

When you run the Memcached engine, clusters can be made up of 1 to 20 nodes\. You can partition your database across the nodes\. Your application reads and writes to each node's endpoint\. For more information, see [Node Auto Discovery \(Memcached\)](AutoDiscovery.md)\.

For improved fault tolerance, locate your Memcached nodes in various Availability Zones \(AZs\) within the cluster's region\. That way, a failure in one AZ will have minimal impact upon your entire cluster and application\. For more information, see [Mitigating Failures](FaultTolerance.md)\.

As demand upon your Memcached cluster changes, you can scale out or in by adding or removing nodes and repartitioning your data across the new number of nodes\. When you partition your data, we recommend using consistent hashing\. For more information about consistent hashing, see [Configuring Your ElastiCache Client for Efficient Load Balancing](BestPractices.LoadBalancing.md)\.

![\[Image: Memcached clusters: single node and multiple node clusters\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-Cluster-Memcached.png)

*Memcached clusters: single node and multiple node clusters*

#### Redis Clusters<a name="WhatIs.Components.Clusters.TypicalConfigurations.Redis"></a>

A Redis \(cluster mode enabled\) cluster contains 1 to 15 shards \(in the API and CLI, called *node groups*\)\. Redis \(cluster mode disabled\) clusters always contain just one shard \(in the API and CLI, one node group\)\. A Redis shard contains 1 to 6 nodes\. If there is more than one node in a shard, the shard supports replication with one node being the read/write primary node and the others read\-only replica nodes\.

For improved fault tolerance, we recommend having at least two nodes in a Redis cluster and enabling Multi\-AZ with automatic failover\. For more information, see [Mitigating Failures](FaultTolerance.md)\.

As demand upon your Redis \(cluster mode disabled\) cluster changes, you can scale up or down by moving your cluster to a different node instance type\. If your application is read intensive, we recommend adding read\-only replicas Redis \(cluster mode disabled\) cluster so you can spread the reads across a more appropriate number of nodes\.

ElastiCache supports changing a Redis \(cluster mode disabled\) cluster's node type to a larger node type dynamically\. For information on scaling up or down, see [Scaling Single\-Node Redis \(cluster mode disabled\) Clusters](Scaling.RedisStandalone.md) or [Scaling Redis \(cluster mode disabled\) Clusters with Replica Nodes](Scaling.RedisReplGrps.md)\.

## ElastiCache Replication \(Redis\)<a name="WhatIs.Components.ReplicationGroups"></a>

Before you continue reading here, see [ElastiCache for Redis Terminology](WhatIs.Terms.md) to better understand the differences in terminology between the ElastiCache console and the ElastiCache API and AWS CLI\.

Replication is implemented by grouping from 2 to 6 nodes in a shard \(in the API and CLI, called a node group\)\. One of these nodes is the read/write primary node\. All the other nodes are read\-only replica nodes\.

Each replica node maintains a copy of the data from the primary node\. Replica nodes use asynchronous replication mechanisms to keep synchronized with the primary node\. Applications can read from any node in the cluster but can write only to primary nodes\. Read replicas enhance scalability by spreading reads across multiple endpoints\. Read replicas also improve fault tolerance by maintaining multiple copies of the data\. Locating read replicas in multiple Availability Zones further improves fault tolerance\. For more information on fault tolerance, see [Mitigating Failures](FaultTolerance.md)\.

Redis \(cluster mode disabled\) clusters support one shard \(in the API and CLI, called a *node group*\)\. Redis \(cluster mode enabled\) clusters support from 1 to 15 shards \(in the API and CLI, called node groups\)\.

The following graphic illustrates replication for Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\) clusters using the console's view and terminology\.

![\[Image: Redis replication (console view), single shard and multiple shards.\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCacheClusters-CSN-RedisRepl.png)

*Redis replication \(console view\), single shard and multiple shards*

Replication from the API and CLI perspective uses different terminology to maintain compatibility with previous versions, but the results are the same\. The following table shows the API and CLI terms for implementing replication\.

**Comparing Replication: Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\)**

The following table compares various features of Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\) replication groups\.


****  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/WhatIs.Components.html)

All of the shards \(in the API and CLI, node groups\) and nodes must reside in the same region\. However, you can provision the individual nodes in multiple Availability Zones within that region\. 

Read replicas guard against potential data loss because your data is replicated over two or more nodes— the primary and one or more read replicas\. For greater reliability and faster recovery, we recommend that you create one or more read replicas in different Availability Zones, and enable Multi\-AZ with automatic failover instead of using AOF\. AOF is disabled when Multi\-AZ with automatic failover is enabled\. For more information, see [Replication: Multi\-AZ with Automatic Failover \(Redis\)](AutoFailover.md)\.

**Replication: Limits and Exclusions**

+ AOF is not supported on node types `cache.t1.micro` and `cache.t2.*`\.

+ Multi\-AZ with automatic failover is only supported on Redis versions 2\.6\.8 and later\.

+ Multi\-AZ with automatic failover is not supported on node types T1 and T2\.

For more information on AOF and Multi\-AZ, see [Mitigating Failures](FaultTolerance.md)\.

## Regions and Availability Zones<a name="WhatIs.RegionsAndAZs"></a>

Amazon ElastiCache is available in multiple regions around the world\. Thus, you can launch ElastiCache clusters in the locations that meet your business requirements\. For example, you can launch in the region closest to your customers or to meet certain legal requirements\.

By default, the AWS SDKs, AWS CLI, ElastiCache API, and ElastiCache console reference the US\-West \(Oregon\) region\. As ElastiCache expands availability to new regions, new endpoints for these regions are also available to use in your HTTP requests, the AWS SDKs, AWS CLI, and ElastiCache console\.

Each region is designed to be completely isolated from the other regions\. Within each region are multiple Availability Zones\. By launching your nodes in different Availability Zones, you can achieve the greatest possible fault tolerance\. For more information about regions and Availability Zones, see [Choosing Regions and Availability Zones](RegionsAndAZs.md)\.

![\[Image: Regions and Availability Zones\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-RegionsAndAZs.png)

*Regions and Availability Zones*

For information on regions supported by ElastiCache and their endpoints, see [Supported Regions & Endpoints](RegionsAndAZs.md#SupportedRegions)\.

## ElastiCache Endpoints<a name="WhatIs.Components.Endpoints"></a>

An endpoint is the unique address your application uses to connect to an ElastiCache node or cluster\.

### Memcached Endpoints<a name="w3ab1b3c43c39b5"></a>

Each node in a Memcached cluster has its own endpoint\. The cluster also has an endpoint called the *configuration endpoint*\. If you enable Auto Discovery and connect to the configuration endpoint, your application will automatically *know* each node endpoint, even after adding or removing nodes from the cluster\. For more information, see [Node Auto Discovery \(Memcached\)](AutoDiscovery.md)\.

### Single Node Redis Cluster Endpoints<a name="WhatIs.Components.Endpoints.Redis"></a>

The endpoint for a single node Redis cluster is used to connect to the cluster for both reads and writes\.

### Multi\-Node Redis Cluster Endpoints<a name="WhatIs.Components.Endpoints.Redis.Cluster"></a>

A multiple node Redis \(cluster mode disabled\) cluster has two types of endpoints\. The primary endpoint always connects to the primary node in the cluster, even if the specific node in the primary role changes\. Use the primary endpoint for all writes to the cluster\.

The read endpoint in a Redis \(cluster mode disabled\) cluster always points to a specific node\. Whenever you add or remove a read replica, you must update the associated node endpoint in your application\.

A Redis \(cluster mode enabled\) cluster has a single configuration endpoint\. By connecting to the configuration endpoint your application is able to discover the primary and read endpoints for each shard in the cluster\.

For more information, see [Finding Your ElastiCache Endpoints](Endpoints.md)\.

## ElastiCache Parameter Groups<a name="WhatIs.Components.ParameterGroups"></a>

Cache parameter groups are an easy way to manage runtime settings for supported engine software\. Memcached and Redis have many parameters to control memory usage, eviction policies, item sizes, and more\. An ElastiCache parameter group is a named collection of Memcached\- or Redis\-specific parameters that you can apply to a cluster, thereby guaranteeing that all of the nodes in that cluster are configured in exactly the same way\.

For a list of supported parameters, their default values, and which ones can be modified, see [DescribeEngineDefaultParameters](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEngineDefaultParameters.html) \([describe\-engine\-default\-parameters](http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-engine-default-parameters.html)\)\.

For more detailed information on ElastiCache parameter groups, see [Parameters and Parameter Groups](ParameterGroups.md)\.

## ElastiCache Security<a name="WhatIs.Components.Security"></a>

For enhanced security, ElastiCache node access is restricted to applications running on whitelisted Amazon EC2 instances\. You can control the Amazon EC2 instances that can access your cluster by using subnet groups or security groups\.

By default, all new ElastiCache clusters are launched in an Amazon Virtual Private Cloud \(Amazon VPC\) environment\. You can use *subnet groups* to grant cluster access from Amazon EC2 instances running on specific subnets\. If you choose to run your cluster outside of Amazon VPC, you can create *security groups* to authorize Amazon EC2 instances running within specific Amazon EC2 security groups\.

## ElastiCache Security Groups<a name="WhatIs.Components.SecurityGroups"></a>

**Note**  
ElastiCache security groups are only applicable to clusters that are not running in an Amazon Virtual Private Cloud \(Amazon VPC\) environment\. If you are running your ElastiCache nodes in an Amazon VPC, you control access to your cache clusters with Amazon VPC security groups, which are different from ElastiCache security groups\.  
 For more information on using ElastiCache in an Amazon VPC, see [Amazon Virtual Private Cloud \(Amazon VPC\) with ElastiCache](AmazonVPC.md)\.

ElastiCache allows you to control access to your clusters using security groups\. A security group acts like a firewall, controlling network access to your cluster\. By default, network access to your clusters is turned off\. If you want your applications to access your cluster, you must explicitly enable access from hosts in specific Amazon EC2 security groups\. After ingress rules are configured, the same rules apply to all clusters associated with that security group\.

To allow network access to your cluster, create a security group and use the [AuthorizeCacheSecurityGroupIngress](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_AuthorizeCacheSecurityGroupIngress.html) API or the [authorize\-cache\-security\-group\-ingress](http://docs.aws.amazon.com/cli/latest/reference/elasticache/authorize-cache-security-group-ingress.html) AWS CLI command to authorize the desired Amazon EC2 security group \(which in turn specifies the Amazon EC2 instances allowed\)\. The security group can be associated with your cluster at the time of creation, or by using the ElastiCache management console or the [ModifyCacheCluster](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html) or \([modify\-cache\-cluster](http://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-cluster.html)\) AWS CLI for ElastiCache command\.

**Important**  
IP\-range based access control is currently not enabled for clusters\. All clients to a cluster must be within the Amazon EC2 network, and authorized via security groups as described previously\.

For more information about security groups, see [Security Groups \[EC2\-Classic\]](SecurityGroups.md)\.

## ElastiCache Subnet Groups<a name="WhatIs.Components.SubnetGroups"></a>

A subnet group is a collection of subnets \(typically private\) that you can designate for your clusters running in an Amazon Virtual Private Cloud \(Amazon VPC\) environment\.

If you create a cluster in an Amazon VPC, then you must specify a cache subnet group\. ElastiCache uses that cache subnet group to choose a subnet and IP addresses within that subnet to associate with your cache nodes\.

For more information about cache subnet group usage in an Amazon VPC environment, see [Amazon Virtual Private Cloud \(Amazon VPC\) with ElastiCache](AmazonVPC.md), [Step 4: Authorize Access](GettingStarted.AuthorizeAccess.md), and [Subnets and Subnet Groups](SubnetGroups.md)\.

## ElastiCache Backups/Snapshots \(Redis\)<a name="WhatIs.Components.Snapshots"></a>

A backup is a point\-in\-time copy of a Redis cluster\. Backups can be used to restore an existing cluster or to seed a new cluster\. Backups consist of all the data in a cluster plus some metadata\. Backups are not supported by the Memcached engine\.

Depending upon the version of Redis running on your cluster, the backup process requires differing amounts of reserved memory to succeed\. For more information, see: 

+ [ElastiCache Backup and Restore \(Redis\)](backups.md)

+ [How Synchronization and Backup are Implemented](Replication.Redis.Versions.md)

+ [Performance Impact of Backups](backups.md#backups-performance)

+ [Ensuring You Have Sufficient Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)

## ElastiCache Events<a name="WhatIs.Components.Events"></a>

When significant events happen on a cache cluster, such as a failure to add a node, success in adding a node, the modification of a security group and others, ElastiCache sends notification to a specific Amazon SNS topic\. By monitoring for key events you can know the current state of your clusters and, depending upon the event, be able to take corrective action\.

For more information on ElastiCache events, see [Monitoring ElastiCache Events](ECEvents.md)\.
