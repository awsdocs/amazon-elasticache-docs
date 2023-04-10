# Managing clusters<a name="Clusters"></a>

A *cluster* is a collection of one or more cache nodes, all of which run an instance of the Memcached cache engine software\. When you create a cluster, you specify the engine and version for all of the nodes to use\.

The following diagram illustrates a typical Memcached cluster\. Memcached clusters contain from 1 to 40 nodes across which you horizontally partition your data\.

To request a limit increase, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) and choose the limit type **Nodes per cluster per instance type**\. 

A typical Memcached cluster looks as follows\.

![\[Image: Typical Memcached Cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-Cluster-Memcached.png)

Most ElastiCache operations are performed at the cluster level\. You can set up a cluster with a specific number of nodes and a parameter group that controls the properties for each node\. All nodes within a cluster are designed to be of the same node type and have the same parameter and security group settings\. 

Every cluster must have a cluster identifier\. The cluster identifier is a customer\-supplied name for the cluster\. This identifier specifies a particular cluster when interacting with the ElastiCache API and AWS CLI commands\. The cluster identifier must be unique for that customer in an AWS Region\.

ElastiCache supports multiple engine versions\. Unless you have specific reasons, we recommend using the latest version\.

ElastiCache clusters are designed to be accessed using an Amazon EC2 instance\. If you launch your cluster in a virtual private cloud \(VPC\) based on the Amazon VPC service, you can access it from outside AWS\. For more information, see [Accessing ElastiCache resources from outside AWS](accessing-elasticache.md#access-from-outside-aws)\.

For a list of supported Memcached versions, see [Supported ElastiCache for Memcached Versions](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/supported-engine-versions-mc.html)\.