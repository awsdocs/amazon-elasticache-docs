# What is Amazon ElastiCache for Redis?<a name="WhatIs"></a>

Welcome to the *Amazon ElastiCache for Redis User Guide*\. Amazon ElastiCache is a web service that makes it easy to set up, manage, and scale a distributed in\-memory data store or cache environment in the cloud\. It provides a high\-performance, scalable, and cost\-effective caching solution\. At the same time, it helps remove the complexity associated with deploying and managing a distributed cache environment\.

**Note**  
Amazon ElastiCache works with both the Redis and Memcached engines\. Use the guide for the engine that you're interested in\. If you're unsure which engine you want to use, see [Comparing Memcached and Redis](SelectEngine.md) in this guide\.

## Overview of ElastiCache for Redis<a name="WhatIs.Overview"></a>

Existing applications that use Redis can use ElastiCache with almost no modification\. Your applications simply need information about the host names and port numbers of the ElastiCache nodes that you have deployed\. 

ElastiCache for Redis has multiple features that help make the service more reliable for critical production deployments:
+ Automatic detection of and recovery from cache node failures\.
+ Multi\-AZ for a failed primary cluster to a read replica, in Redis clusters that support replication\.
+ Redis \(cluster mode enabled\) supports partitioning your data across up to 500 shards\.
+ For Redis version 3\.2 and later, all versions support encryption in transit and encryption at rest encryption with authentication\. This support helps you build HIPAA\-compliant applications\. 
+ Flexible Availability Zone placement of nodes and clusters for increased fault tolerance\.
+ Integration with other AWS services such as Amazon EC2, Amazon CloudWatch, AWS CloudTrail, and Amazon SNS\. This integration helps provide a managed in\-memory caching solution that is high\-performance and highly secure\.
+ ElastiCache for Redis manages backups, software patching, automatic failure detection, and recovery\.
+ You can have automated backups performed when you need them, or manually create your own backup snapshot\. You can use these backups to restore a cluster\. The ElastiCache for Redis restore process works reliably and efficiently\.
+ You can get high availability with a primary instance and a synchronous secondary instance that you can fail over to when problems occur\. You can also use read replicas to increase read scaling\. 
+ You can control access to your ElastiCache for Redis clusters by using AWS Identity and Access Management to define users and permissions\. You can also help protect your clusters by putting them in a virtual private cloud \(VPC\)\. 
+ By using the Global Datastore for Redis feature, you can work with fully managed, fast, reliable, and secure replication across AWS Regions\. Using this feature, you can create cross\-Region read replica clusters for ElastiCache for Redis to enable low\-latency reads and disaster recovery across AWS Regions\. 
+ Data tiering provides a price\-performance option for Redis workloads by utilizing lower\-cost solid state drives \(SSDs\) in each cluster node in addition to storing data in memory\. It is ideal for workloads that access up to 20 percent of their overall dataset regularly, and for applications that can tolerate additional latency when accessing data on SSD\. For more information, see [Data tiering](data-tiering.md)\.

## Clusters<a name="WhatIs.Clusters"></a>

The basic building block of ElastiCache for Redis is the cluster\. A cluster is a collection of one or more cache nodes, all of which run an instance of the Redis cache engine software\. When you create a cluster, you specify the engine and version for all of the nodes to use\. Your ElastiCache for Redis instances are designed to be accessed through an Amazon EC2 instance\. You can create and modify a cluster by using the AWS CLI, the ElastiCache for Redis API, or the AWS Management Console\.

Each ElastiCache for Redis cluster runs a Redis engine version\. Each Redis engine version has its own supported features\. Additionally, each Redis engine version has a set of parameters in a parameter group that control the behavior of the clusters that it manages\.

The computation and memory capacity of a cluster is determined by its instance, or node, class\. You can select the node type that best meets your needs\. If your needs change over time, you can change node types\. For information, see [Supported node types](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/CacheNodes.SupportedTypes.html)\.

You can also leverage data\-tiering when considering your node type needs\. Data tiering is a feature where some least frequently used data is stored on disk to mitigate against memory limitations on applications that can tolerate additional latency when data on SSD \(solid state drives\) is accessed\.

**Note**  
For pricing information on ElastiCache instance classes, see [Amazon ElastiCache pricing](https://aws.amazon.com/elasticache/pricing/)\.

Cluster node storage comes in two types: Standard and memory\-optimized\. They differ in performance characteristics and price, allowing you to tailor your storage performance and cost to your needs\. Each instance has minimum and maximum storage requirements depending on the storage type\. It's important to have sufficient storage so that your clusters have room to grow\. Also, sufficient storage makes sure that features have room to write content or log entries\.

You can run a cluster on a virtual private cloud \(VPC\) using the Amazon Virtual Private Cloud \(Amazon VPC\) service\. When you use a VPC, you have control over your virtual networking environment\. You can choose your own IP address range, create subnets, and configure routing and access control lists\. ElastiCache manages backups, software patching, automatic failure detection, and recovery\. There's no additional cost to run your cluster in a VPC\. For more information on using Amazon VPC with ElastiCache for Redis, see [Amazon VPCs and ElastiCache security](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/VPCs.html)\.

## AWS Regions and Availability Zones<a name="WhatIs.AZs"></a>

Amazon cloud computing resources are housed in highly available data center facilities in different areas of the world \(for example, North America, Europe, or Asia\)\. Each data center location is called an AWS Region\.

Each AWS Region contains multiple distinct locations called Availability Zones, or AZs\. Each Availability Zone is engineered to be isolated from failures in other Availability Zones\. Each is engineered to provide inexpensive, low\-latency network connectivity to other Availability Zones in the same AWS Region\. By launching instances in separate Availability Zones, you can protect your applications from the failure of a single location\. For more information, see [Choosing regions and availability zones](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/RegionsAndAZs.html)\. You can create your cluster in several Availability Zones, an option called a Multi\-AZ deployment\. When you choose this option, Amazon automatically provisions and maintains a secondary standby node instance in a different Availability Zone\. Your primary node instance is asynchronously replicated across Availability Zones to the secondary instance\. This approach helps provide data redundancy and failover support, eliminate I/O freezes, and minimize latency spikes during system backups\. For more information, see [Minimizing downtime in ElastiCache for Redis with Multi\-AZ](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/AutoFailover.html)\.

## Security<a name="WhatIs.Security"></a>

A security group controls the access to a cluster\. It does so by allowing access to IP address ranges or Amazon EC2 instances that you specify\. For more information about security groups, see [Security in ElastiCache for Redis](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/redis-security.html)\.

## Monitoring an ElastiCache for Redis cluster<a name="WhatIs.Monitoring"></a>

There are several ways that you can track the performance and health of a ElastiCache for Redis cluster\. You can use the CloudWatch service to monitor the performance and health of a cluster\. CloudWatch performance charts are shown in the ElastiCache for Redis console\. You can also subscribe to ElastiCache for Redis events to be notified about changes to a cluster, snapshot, parameter group, or security group\. For more information, see [Monitoring Use with CloudWatch metrics](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/CacheMetrics.html)\.