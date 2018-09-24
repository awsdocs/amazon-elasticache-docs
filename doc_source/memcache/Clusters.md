# Managing Your ElastiCache Clusters<a name="Clusters"></a>

A *cluster* is a collection of one or more cache nodes, all of which run an instance of the Memcached cache engine software\. When you create a cluster, you specify the engine and version that all of the nodes will use\.

The following diagram illustrates a typical Memcached cluster\. Memcached clusters contain from 1 to 20 nodes across which you horizontally partition your data\.

![\[Image: Typical Memcached Cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-Cluster-Memcached.png)

*Typical Memcached Cluster*

Most ElastiCache operations are performed at the cluster level\. You can set up a cluster with a specific number of nodes and a parameter group that controls the properties for each node\. All nodes within a cluster are designed to be of the same node type and have the same parameter and security group settings\. 

Every cluster must have a cluster identifier\. The cluster identifier is a customer\-supplied name for the cluster\. This identifier specifies a particular cluster when interacting with the ElastiCache API and AWS CLI commands\. The cluster identifier must be unique for that customer in an AWS Region\.

ElastiCache supports multiple engine versions\. Unless you have specific reasons, we recommend always using the your engine's latest version\.

ElastiCache clusters are designed to be accessed via an Amazon EC2 instance\. If you launch your cluster in an Amazon VPC, you can access if from outside AWS\. For more information, see:
+ [Step 2: Authorize Access](GettingStarted.AuthorizeAccess.md)
+ [Accessing ElastiCache Resources from Outside AWS](accessing-elasticache.md#access-from-outside-aws)

## Supported Memcached Versions<a name="Clusters.MemcachedVersions"></a>
+ [Memcached Version 1\.4\.34](supported-engine-versions.md#memcached-version-1-4-34)
+ [Memcached Version 1\.4\.33](supported-engine-versions.md#memcached-version-1-4-33)
+ [Memcached Version 1\.4\.24](supported-engine-versions.md#memcached-version-1-4-24)
+ [Memcached Version 1\.4\.14 ](supported-engine-versions.md#memcached-version-1-4-14)
+ [Memcached Version 1\.4\.5](supported-engine-versions.md#memcached-version-1-4-5)

## Other ElastiCache Cluster Operations<a name="Clusters.OtherOperations"></a>

Additional operations involving clusters: 
+ [Finding Connection Endpoints](Endpoints.md)
+ [Accessing ElastiCache Resources from Outside AWS](accessing-elasticache.md#access-from-outside-aws)