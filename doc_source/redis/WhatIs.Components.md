# ElastiCache for Redis Components and Features<a name="WhatIs.Components"></a>

Following, you can find an overview of the major components of an Amazon ElastiCache deployment\.

**Topics**
+ [ElastiCache Nodes](#WhatIs.Components.Nodes)
+ [ElastiCache for Redis Shards](#WhatIs.Components.Shards)
+ [ElastiCache for Redis Clusters](#WhatIs.Components.Clusters)
+ [ElastiCache for Redis Replication](#WhatIs.Components.ReplicationGroups)
+ [AWS Regions and Availability Zones](#WhatIs.RegionsAndAZs)
+ [ElastiCache for Redis Endpoints](#WhatIs.Components.Endpoints)
+ [ElastiCache Parameter Groups](#WhatIs.Components.ParameterGroups)
+ [ElastiCache for Redis Security](#WhatIs.Components.Security)
+ [ElastiCache Security Groups](#WhatIs.Components.SecurityGroups)
+ [ElastiCache Subnet Groups](#WhatIs.Components.SubnetGroups)
+ [ElastiCache for Redis Backups](#WhatIs.Components.Snapshots)
+ [ElastiCache Events](#WhatIs.Components.Events)

## ElastiCache Nodes<a name="WhatIs.Components.Nodes"></a>

A *node* is the smallest building block of an ElastiCache deployment\. A node can exist in isolation from or in some relationship to other nodes\.

A node is a fixed\-size chunk of secure, network\-attached RAM\. Each node runs an instance of the engine and version that was chosen when you created your cluster\. If necessary, you can scale the nodes in a cluster up or down to a different instance type\. For more information, see [Scaling ElastiCache for Redis Clusters](Scaling.md)\.

Every node within a cluster is the same instance type and runs the same cache engine\. Each cache node has its own Domain Name Service \(DNS\) name and port\. Multiple types of cache nodes are supported, each with varying amounts of associated memory\. For a list of supported node instance types, see [Supported Node Types](CacheNodes.SupportedTypes.md)\.

You can purchase nodes on a pay\-as\-you\-go basis, where you only pay for your use of a node\. Or you can purchase reserved nodes at a significantly reduced hourly rate\. If your usage rate is high, purchasing reserved nodes can save you money\. Suppose that your cluster is almost always in use, and you occasionally add nodes to handle use spikes\. In this case, you can purchase a number of reserved nodes to run most of the time and purchase pay\-as\-you\-go nodes for the times you occasionally need to add nodes\. For more information on reserved nodes, see [ElastiCache Reserved Nodes](CacheNodes.Reserved.md)\.

For more information on nodes, see [Managing Nodes](CacheNodes.md)\.

## ElastiCache for Redis Shards<a name="WhatIs.Components.Shards"></a>

A Redis *shard* \(called a *node group* in the API and CLI\) is a grouping of one to six related nodes\. A Redis \(cluster mode disabled\) cluster always has one shard\. A Redis \(cluster mode enabled\) cluster can have 1–90 shards\.

A *multiple node shard* implements replication by have one read/write primary node and 1–5 replica nodes\. For more information, see [High Availability Using Replication Groups](Replication.md)\.

For more information on shards, see [Shards](Shards.md)\.

## ElastiCache for Redis Clusters<a name="WhatIs.Components.Clusters"></a>

A Redis *cluster* is a logical grouping of one or more [ElastiCache for Redis Shards](#WhatIs.Components.Shards)\. Data is partitioned across the shards in a Redis \(cluster mode enabled\) cluster\.

Many ElastiCache operations are targeted at clusters:
+ Creating a cluster
+ Modifying a cluster
+ Taking snapshots of a cluster \(all versions of Redis\)
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

  Information about improving the fault tolerance of your clusters and replication groups\.

### Typical Cluster Configurations<a name="WhatIs.Components.Clusters.TypicalConfigurations"></a>

A Redis cluster contains 1–90 shards \(in the API, called node groups\), each of which is a partition of your data\. Redis \(cluster mode disabled\) always has just one shard\.

Following are typical cluster configurations\.

#### Redis Clusters<a name="WhatIs.Components.Clusters.TypicalConfigurations.Redis"></a>

A Redis \(cluster mode enabled\) cluster contains 1–90 shards \(in the API and CLI, called *node groups*\)\. Redis \(cluster mode disabled\) clusters always contain just one shard \(in the API and CLI, one node group\)\. A Redis shard contains one to six nodes\. If there is more than one node in a shard, the shard supports replication with one node being the read/write primary node and the others read\-only replica nodes\.

For improved fault tolerance, we recommend having at least two nodes in a Redis cluster and enabling Multi\-AZ with automatic failover\. For more information, see [Mitigating Failures](FaultTolerance.md)\.

As demand upon your Redis \(cluster mode disabled\) cluster changes, you can scale up or down by moving your cluster to a different node instance type\. If your application is read intensive, we recommend adding read\-only replicas Redis \(cluster mode disabled\) cluster so you can spread the reads across a more appropriate number of nodes\.

ElastiCache supports changing a Redis \(cluster mode disabled\) cluster's node type to a larger node type dynamically\. For information on scaling up or down, see [Scaling Single\-Node Redis \(cluster mode disabled\) Clusters](Scaling.RedisStandalone.md) or [Scaling Redis \(cluster mode disabled\) Clusters with Replica Nodes](Scaling.RedisReplGrps.md)\.

## ElastiCache for Redis Replication<a name="WhatIs.Components.ReplicationGroups"></a>

Before you continue reading here, see [ElastiCache for Redis Terminology](WhatIs.Terms.md) to better understand the differences in terminology between the ElastiCache console and the ElastiCache API and AWS CLI\.

Replication is implemented by grouping from two to six nodes in a shard \(in the API and CLI, called a node group\)\. One of these nodes is the read/write primary node\. All the other nodes are read\-only replica nodes\.

Each replica node maintains a copy of the data from the primary node\. Replica nodes use asynchronous replication mechanisms to keep synchronized with the primary node\. Applications can read from any node in the cluster but can write only to primary nodes\. Read replicas enhance scalability by spreading reads across multiple endpoints\. Read replicas also improve fault tolerance by maintaining multiple copies of the data\. Locating read replicas in multiple Availability Zones further improves fault tolerance\. For more information on fault tolerance, see [Mitigating Failures](FaultTolerance.md)\.

Redis \(cluster mode disabled\) clusters support one shard \(in the API and CLI, called a *node group*\)\. Redis \(cluster mode enabled\) clusters support 1–90 shards \(in the API and CLI, called node groups\)\.

Replication from the API and CLI perspective uses different terminology to maintain compatibility with previous versions, but the results are the same\. The following table shows the API and CLI terms for implementing replication\.

**Comparing Replication: Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\)**

In the following table, you can find a comparison of the features of Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\) replication groups\.


****  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/WhatIs.Components.html)

All of the shards \(in the API and CLI, node groups\) and nodes must reside in the same AWS Region\. However, you can provision the individual nodes in multiple Availability Zones within that AWS Region\. 

Read replicas guard against potential data loss because your data is replicated over two or more nodes—the primary and one or more read replicas\. For greater reliability and faster recovery, we recommend that you create one or more read replicas in different Availability Zones, and enable Multi\-AZ with automatic failover instead of using AOF\. AOF is disabled when Multi\-AZ with automatic failover is enabled\. For more information, see [Minimizing Downtime: Multi\-AZ with Automatic Failover](AutoFailover.md)\.

**Replication: Limits and Exclusions**
+ AOF is not supported on node type `cache.t1.micro` and cache\.t2\. For nodes of these types, the `appendonly` parameter value is ignored\.
+ Multi\-AZ with automatic failover is not supported on node types T1\.

For more information on AOF and Multi\-AZ, see [Mitigating Failures](FaultTolerance.md)\.

## AWS Regions and Availability Zones<a name="WhatIs.RegionsAndAZs"></a>

Amazon ElastiCache is available in multiple AWS Regions around the world\. Thus, you can launch ElastiCache clusters in the locations that meet your business requirements\. For example, you can launch in the AWS Region closest to your customers or to meet certain legal requirements\.

By default, the AWS SDKs, AWS CLI, ElastiCache API, and ElastiCache console reference the US\-West \(Oregon\) region\. As ElastiCache expands availability to new AWS Regions, new endpoints for these AWS Regions are also available to use in your HTTP requests, the AWS SDKs, AWS CLI, and ElastiCache console\.

Each AWS Region is designed to be completely isolated from the other AWS Regions\. Within each are multiple Availability Zones\. By launching your nodes in different Availability Zones, you can achieve the greatest possible fault tolerance\. For more information about AWS Regions and Availability Zones, see [Choosing Regions and Availability Zones](RegionsAndAZs.md)\. In the following diagram, you can see a high\-level view of how AWS Regions and Availability Zones work\.

![\[Image: AWS Regions and Availability Zones\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-RegionsAndAZs.png)

For information on AWS Regions supported by ElastiCache and their endpoints, see [Supported Regions & Endpoints](RegionsAndAZs.md#SupportedRegions)\.

## ElastiCache for Redis Endpoints<a name="WhatIs.Components.Endpoints"></a>

An endpoint is the unique address your application uses to connect to an ElastiCache node or cluster\.

### Single Node Redis \(cluster mode disabled\) Endpoints<a name="WhatIs.Components.Endpoints.Redis"></a>

The endpoint for a single node Redis cluster is used to connect to the cluster for both reads and writes\.

### Multi\-Node Redis \(cluster mode disabled\) Endpoints<a name="WhatIs.Components.Endpoints.Redis.Multi"></a>

A multiple node Redis \(cluster mode disabled\) cluster has two types of endpoints\. The primary endpoint always connects to the primary node in the cluster, even if the specific node in the primary role changes\. Use the primary endpoint for all writes to the cluster\.

The read endpoint in a Redis \(cluster mode disabled\) cluster always points to a specific node\. Whenever you add or remove a read replica, you must update the associated node endpoint in your application\.

### Redis \(cluster mode enabled\) Endpoints<a name="WhatIs.Components.Endpoints.Redis.Cluster"></a>

A Redis \(cluster mode enabled\) cluster has a single configuration endpoint\. By connecting to the configuration endpoint, your application is able to discover the primary and read endpoints for each shard in the cluster\.

For more information, see [Finding Connection Endpoints](Endpoints.md)\.

## ElastiCache Parameter Groups<a name="WhatIs.Components.ParameterGroups"></a>

Cache parameter groups are an easy way to manage runtime settings for supported engine software\. Parameters are used to control memory usage, eviction policies, item sizes, and more\. An ElastiCache parameter group is a named collection of engine\-specific parameters that you can apply to a cluster\. By doing this, you make sure that all of the nodes in that cluster are configured in exactly the same way\.

For a list of supported parameters, their default values, and which ones can be modified, see [DescribeEngineDefaultParameters](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEngineDefaultParameters.html) \(CLI: [describe\-engine\-default\-parameters](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-engine-default-parameters.html)\)\.

For more detailed information on ElastiCache parameter groups, see [Configuring Engine Parameters Using Parameter Groups](ParameterGroups.md)\.

## ElastiCache for Redis Security<a name="WhatIs.Components.Security"></a>

For enhanced security, ElastiCache for Redis node access is restricted to applications running on whitelisted Amazon EC2 instances\. You can control the Amazon EC2 instances that can access your cluster by using subnet groups or security groups\.

By default, all new ElastiCache for Redis clusters are launched in an Amazon Virtual Private Cloud \(Amazon VPC\) environment\. You can use *subnet groups* to grant cluster access from Amazon EC2 instances running on specific subnets\. If you choose to run your cluster outside of Amazon VPC, you can create *security groups* to authorize Amazon EC2 instances running within specific Amazon EC2 security groups\.

In addition to restricting access to your nodes, ElastiCache for Redis supports TLS and in\-place encryption for nodes running specified versions of the ElastiCache for Redis engine\. For more information, see the following:
+ [Data Security in Amazon ElastiCache](encryption.md)
+ [HIPAA Compliance](elasticache-compliance.md#elasticache-compliance-hipaa)
+ [Authenticating Users with Redis AUTH](auth.md)

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

For more information about cache subnet group usage in an Amazon VPC environment, see the following:
+ [Amazon VPCs and ElastiCache Security](VPCs.md)
+ [Step 2: Authorize Access](GettingStarted.AuthorizeAccess.md)
+ [Subnets and Subnet Groups](SubnetGroups.md)

## ElastiCache for Redis Backups<a name="WhatIs.Components.Snapshots"></a>

A backup is a point\-in\-time copy of a Redis cluster\. Backups can be used to restore an existing cluster or to seed a new cluster\. Backups consist of all the data in a cluster plus some metadata\.

Depending upon the version of Redis running on your cluster, the backup process requires differing amounts of reserved memory to succeed\. For more information, see the following: 
+ [ElastiCache for Redis Backup and Restore](backups.md)
+ [How Synchronization and Backup are Implemented](Replication.Redis.Versions.md)
+ [Performance Impact of Backups](backups.md#backups-performance)
+ [Ensuring You Have Sufficient Memory to Create a Redis Snapshot](BestPractices.BGSAVE.md)

## ElastiCache Events<a name="WhatIs.Components.Events"></a>

When significant events happen on a cache cluster, ElastiCache sends notification to a specific Amazon SNS topic\. Significant events can include such things as a failure to add a node, success in adding a node, the modification of a security group, and others\. By monitoring for key events, you can know the current state of your clusters and, depending upon the event, take corrective action\.

For more information on ElastiCache events, see [Monitoring ElastiCache Events](ECEvents.md)\.