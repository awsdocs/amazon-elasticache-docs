# Scaling ElastiCache for Memcached clusters<a name="Scaling"></a>

The amount of data your application needs to process is seldom static\. It increases and decreases as your business grows or experiences normal fluctuations in demand\. If you self\-manage your cache, you need to provision sufficient hardware for your demand peaks, which can be expensive\. By using Amazon ElastiCache you can scale to meet current demand, paying only for what you use\. ElastiCache enables you to scale your cache to match demand\.

The following helps you find the correct topic for the scaling actions that you want to perform\.


**Scaling Memcached Clusters**  

| Action | Topic | 
| --- | --- | 
|  Scaling out  |  [Adding nodes to a cluster](Clusters.AddNode.md)  | 
|  Scaling in  |  [Removing nodes from a cluster](Clusters.DeleteNode.md)  | 
|  Changing node types  |  [Scaling Memcached vertically](#Scaling.Memcached.Vertically)  | 

Memcached clusters are composed of 1 to 40 nodes\. Scaling a Memcached cluster out and in is as easy as adding or removing nodes from the cluster\. 

If you need more than 40 nodes in a Memcached cluster, or more than 300 nodes total in an AWS Region, fill out the ElastiCache Limit Increase Request form at [http://aws\.amazon\.com/contact\-us/elasticache\-node\-limit\-request/](http://aws.amazon.com/contact-us/elasticache-node-limit-request/)\.

Because you can partition your data across all the nodes in a Memcached cluster, scaling up to a node type with greater memory is seldom required\. However, because the Memcached engine does not persist data, if you do scale to a different node type, your new cluster starts out empty unless your application populates it\.

**Topics**
+ [Scaling Memcached Horizontally](#Scaling.Memcached.Horizontally)
+ [Scaling Memcached vertically](#Scaling.Memcached.Vertically)

## Scaling Memcached Horizontally<a name="Scaling.Memcached.Horizontally"></a>

The Memcached engine supports partitioning your data across multiple nodes\. Because of this, Memcached clusters scale horizontally easily\. A Memcached cluster can have from 1 to 40 nodes\. To horizontally scale your Memcached cluster, merely add or remove nodes\.

If you need more than 40 nodes in a Memcached cluster, or more than 300 nodes total in an AWS Region, fill out the ElastiCache Limit Increase Request form at [http://aws\.amazon\.com/contact\-us/elasticache\-node\-limit\-request/](http://aws.amazon.com/contact-us/elasticache-node-limit-request/)\.

The following topics detail how to scale your Memcached cluster out or in by adding or removing nodes\.
+ [Adding nodes to a cluster](Clusters.AddNode.md)
+ [Removing nodes from a cluster](Clusters.DeleteNode.md)

Each time you change the number of nodes in your Memcached cluster, you must re\-map at least some of your keyspace so it maps to the correct node\. For more detailed information on load balancing your Memcached cluster, see [Configuring your ElastiCache client for efficient load balancing](BestPractices.LoadBalancing.md)\.

If you use auto discovery on your Memcached cluster, you do not need to change the endpoints in your application as you add or remove nodes\. For more information on auto discovery, see [Automatically identify nodes in your cluster](AutoDiscovery.md)\. If you do not use auto discovery, each time you change the number of nodes in your Memcached cluster you must update the endpoints in your application\.

## Scaling Memcached vertically<a name="Scaling.Memcached.Vertically"></a>

When you scale your Memcached cluster up or down, you must create a new cluster\. Memcached clusters always start out empty unless your application populates it\. 

**Important**  
If you are scaling down to a smaller node type, be sure that the smaller node type is adequate for your data and overhead\. For more information, see [Choosing your Memcached node size](nodes-select-size.md#CacheNodes.SelectSize)\.

**Topics**
+ [Scaling Memcached vertically \(Console\)](#Scaling.Memcached.Vertically.CON)
+ [Scaling Memcached vertically \(AWS CLI\)](#Scaling.Memcached.Vertically.CLI)
+ [Scaling Memcached vertically \(ElastiCache API\)](#Scaling.Memcached.Vertically.API)

### Scaling Memcached vertically \(Console\)<a name="Scaling.Memcached.Vertically.CON"></a>

The following procedure walks you through scaling your cluster vertically using the ElastiCache console\.

**To scale a Memcached cluster vertically \(console\)**

1. Create a new cluster with the new node type\. For more information, see [Creating a Memcached cluster \(console\)](Clusters.Create.md#Clusters.Create.CON.Memcached)\.

1. In your application, update the endpoints to the new cluster's endpoints\. For more information, see [Finding a Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Memcached)\.

1. Delete the old cluster\. For more information, see [Using the AWS Management Console](Clusters.Delete.md#Clusters.Delete.CON)\.

### Scaling Memcached vertically \(AWS CLI\)<a name="Scaling.Memcached.Vertically.CLI"></a>

The following procedure walks you through scaling your Memcached cache cluster vertically using the AWS CLI\.

**To scale a Memcached cache cluster vertically \(AWS CLI\)**

1. Create a new cache cluster with the new node type\. For more information, see [Creating a cluster \(AWS CLI\)](Clusters.Create.md#Clusters.Create.CLI)\.

1. In your application, update the endpoints to the new cluster's endpoints\. For more information, see [Finding Endpoints \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI)\.

1. Delete the old cache cluster\. For more information, see [Using the AWS CLI](Clusters.Delete.md#Clusters.Delete.CLI)\.

### Scaling Memcached vertically \(ElastiCache API\)<a name="Scaling.Memcached.Vertically.API"></a>

The following procedure walks you through scaling your Memcached cache cluster vertically using the ElastiCache API\.

**To scale a Memcached cache cluster vertically \(ElastiCache API\)**

1. Create a new cache cluster with the new node type\. For more information, see [Creating a cluster \(ElastiCache API\)](Clusters.Create.md#Clusters.Create.API)\.

1. In your application, update the endpoints to the new cache cluster's endpoints\. For more information, see [Finding Endpoints \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API)\.

1. Delete the old cache cluster\. For more information, see [Using the ElastiCache API](Clusters.Delete.md#Clusters.Delete.API)\.