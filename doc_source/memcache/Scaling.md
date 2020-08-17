# Scaling ElastiCache for Memcached Clusters<a name="Scaling"></a>

The amount of data your application needs to process is seldom static\. It increases and decreases as your business grows or experiences normal fluctuations in demand\. If you self\-manage your cache, you need to provision sufficient hardware for your demand peaks, which can be expensive\. By using Amazon ElastiCache you can scale to meet current demand, paying only for what you use\. ElastiCache enables you to scale your cache to match demand\.

The following helps you find the correct topic for the scaling actions that you want to perform\.


**Scaling Memcached Clusters**  

| Action | Topic | 
| --- | --- | 
|  Scaling out  |  [Adding Nodes to a Cluster](Clusters.AddNode.md)  | 
|  Scaling in  |  [Removing Nodes from a Cluster](Clusters.DeleteNode.md)  | 
|  Changing node types  |  [Scaling Memcached Vertically](#Scaling.Memcached.Vertically)  | 

Memcached clusters are composed of 1 to 20 nodes\. Scaling a Memcached cluster out and in is as easy as adding or removing nodes from the cluster\. 

If you need more than 20 nodes in a Memcached cluster, or more than 100 nodes total in an AWS Region, fill out the ElastiCache Limit Increase Request form at [https://aws\.amazon\.com/contact\-us/elasticache\-node\-limit\-request/](https://aws.amazon.com/contact-us/elasticache-node-limit-request/)\.

Because you can partition your data across all the nodes in a Memcached cluster, scaling up to a node type with greater memory is seldom required\. However, because the Memcached engine does not persist data, if you do scale to a different node type, your new cluster starts out empty unless your application populates it\.

**Note**  
You cannot automatically migrate Intel\-based clusters to ARM\-based clusters while preserving cluster state\. 

**Topics**
+ [Scaling Memcached Horizontally](#Scaling.Memcached.Horizontally)
+ [Scaling Memcached Vertically](#Scaling.Memcached.Vertically)

## Scaling Memcached Horizontally<a name="Scaling.Memcached.Horizontally"></a>

The Memcached engine supports partitioning your data across multiple nodes\. Because of this, Memcached clusters scale horizontally easily\. A Memcached cluster can have from 1 to 20 nodes\. To horizontally scale your Memcached cluster, merely add or remove nodes\.

If you need more than 20 nodes in a Memcached cluster, or more than 100 nodes total in an AWS Region, fill out the ElastiCache Limit Increase Request form at [https://aws\.amazon\.com/contact\-us/elasticache\-node\-limit\-request/](https://aws.amazon.com/contact-us/elasticache-node-limit-request/)\.

The following topics detail how to scale your Memcached cluster out or in by adding or removing nodes\.
+ [Adding Nodes to a Cluster](Clusters.AddNode.md)
+ [Removing Nodes from a Cluster](Clusters.DeleteNode.md)

Each time you change the number of nodes in your Memcached cluster, you must re\-map at least some of your keyspace so it maps to the correct node\. For more detailed information on load balancing your Memcached cluster, see [Configuring Your ElastiCache Client for Efficient Load Balancing](BestPractices.LoadBalancing.md)\.

If you use auto discovery on your Memcached cluster, you do not need to change the endpoints in your application as you add or remove nodes\. For more information on auto discovery, see [Automatically Identify Nodes in your Memcached Cluster](AutoDiscovery.md)\. If you do not use auto discovery, each time you change the number of nodes in your Memcached cluster you must update the endpoints in your application\.

## Scaling Memcached Vertically<a name="Scaling.Memcached.Vertically"></a>

When you scale your Memcached cluster up or down, you must create a new cluster\. Memcached clusters always start out empty unless your application populates it\. 

**Important**  
If you are scaling down to a smaller node type, be sure that the smaller node type is adequate for your data and overhead\. For more information, see [Choosing Your Memcached Node Size](nodes-select-size.md#CacheNodes.SelectSize)\.

**Topics**
+ [Scaling Memcached Vertically \(Console\)](#Scaling.Memcached.Vertically.CON)
+ [Scaling Memcached Vertically \(AWS CLI\)](#Scaling.Memcached.Vertically.CLI)
+ [Scaling Memcached Vertically \(ElastiCache API\)](#Scaling.Memcached.Vertically.API)

### Scaling Memcached Vertically \(Console\)<a name="Scaling.Memcached.Vertically.CON"></a>

The following procedure walks you through scaling your cluster vertically using the ElastiCache console\.

**To scale a Memcached cluster vertically \(console\)**

1. Create a new cluster with the new node type\. For more information, see [Creating a Memcached Cluster \(Console\)](Clusters.Create.CON.Memcached.md)\.

1. In your application, update the endpoints to the new cluster's endpoints\. For more information, see [Finding a Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Memcached)\.

1. Delete the old cluster\. For more information, see [Using the AWS Management Console](Clusters.Delete.md#Clusters.Delete.CON)\.

### Scaling Memcached Vertically \(AWS CLI\)<a name="Scaling.Memcached.Vertically.CLI"></a>

The following procedure walks you through scaling your Memcached cache cluster vertically using the AWS CLI\.

**To scale a Memcached cache cluster vertically \(AWS CLI\)**

1. Create a new cache cluster with the new node type\. For more information, see [Creating a Cluster \(AWS CLI\)](Clusters.Create.CLI.md)\.

1. In your application, update the endpoints to the new cluster's endpoints\. For more information, see [Finding Endpoints \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI)\.

1. Delete the old cache cluster\. For more information, see [Using the AWS CLI](Clusters.Delete.md#Clusters.Delete.CLI)\.

### Scaling Memcached Vertically \(ElastiCache API\)<a name="Scaling.Memcached.Vertically.API"></a>

The following procedure walks you through scaling your Memcached cache cluster vertically using the ElastiCache API\.

**To scale a Memcached cache cluster vertically \(ElastiCache API\)**

1. Create a new cache cluster with the new node type\. For more information, see [Creating a Cluster \(ElastiCache API\)](Clusters.Create.API.md)\.

1. In your application, update the endpoints to the new cache cluster's endpoints\. For more information, see [Finding Endpoints \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API)\.

1. Delete the old cache cluster\. For more information, see [Using the ElastiCache API](Clusters.Delete.md#Clusters.Delete.API)\.