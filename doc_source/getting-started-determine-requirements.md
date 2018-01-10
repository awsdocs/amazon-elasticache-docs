# Determine Your Requirements \[Every time\]<a name="getting-started-determine-requirements"></a>

Before you create a cluster or replication group, you should always determine the requirements for the cluster so that when you create the cluster or replication group it will meet your business needs and not need to be redone\.


+ [Memory and Processor Requirements](#getting-started-determine-requirements-memory)
+ [Scaling Requirements](#getting-started-determine-requirements-scaling)
+ [Failover Requirements](#getting-started-determine-requirements-failover)
+ [Access Requirements](#getting-started-determine-requirements-access)
+ [Region and Availability Zone Requirements](#getting-started-determine-requirements-region)

## Memory and Processor Requirements<a name="getting-started-determine-requirements-memory"></a>

The basic building block of Amazon ElastiCache is the node\. Nodes are configured singularly or in groupings to form clusters\. When determining the node type to use for your cluster, take the cluster’s node configuration and the amount of data you have to store into consideration\.

The Memcached engine is multi\-threaded, so a node’s number of cores impacts the compute power available to the cluster\. On the other hand, the Redis engine is single threaded, so a node’s number of cores is irrelevant\.

### Memcached Cluster Configuration<a name="w3ab1b7c13b7b7"></a>

Memcached clusters are comprised of from 1 to 20 nodes\. The data in a Memcached cluster is partitioned across all the nodes in the cluster\. Your application connects with a Memcached cluster using a network address called an Endpoint\. Each node in a Memcached cluster has its own endpoint which your application can use to read from or write to a specific node\. In addition to the node endpoints, the Memcached cluster itself has an endpoint called the *Configuration Endpoint* which your application can use to read from or write to the cluster, leaving the determination of which node to read from or write to up to *Automatic Discovery*\. 

### Redis Cluster Configurations<a name="w3ab1b7c13b7b9"></a>

Amazon ElastiCache for Redis clusters provide a variety of node configurations\. There are three node configurations when running the ElastiCache for Redis engine\. Your application connects with your ElastiCache for Redis cluster using a network address called an Endpoint\. Depending upon your ElastiCache for Redis cluster configuration, you will use the endpoints differently\.

+ **Single Node**

  The simplest configuration is a single node cluster\. The node in a single\-node Redis cluster must have sufficient memory to accommodate all your in\-memory data plus overhead\. A single\-node Redis cluster does not provide high availability as there is no replication of data across multiple nodes\.

+ **Single Shard Multiple Nodes**

  A single shard configuration groups up to 6 nodes in a single cluster, a read/write Primary node and up to 5 read\-only Replica nodes\. Like the single node configuration, each node must be able to accommodate all your in\-memory data plus overhead\. Configurations with 1 or more replica nodes provide high availability by replicating your in\-memory data across all nodes in the shard\. Thus if one node fails you still have your data in one or more of the replica nodes\.

+ **Multiple Shards Multiple Nodes**

  A multiple shard configuration partitions your data across up to 20 shards in the cluster\. Each shard has a read/write Primary node and up to 5 read\-only replica nodes\. Since your in\-memory data is partitioned across multiple shards, each node in a shard only needs enough memory to accommodate its copy of the shard’s portion of your in\-memory data plus overhead\. Shard configurations with 1 or more replica nodes provide high availability by replicating your in\-memory data across all nodes in the shard\. Thus if one node fails you still have the shard’s data in one or more of the replica nodes\.

For more information, see [Choosing Your Node Size](CacheNodes.SelectSize.md) in this guide\.

## Scaling Requirements<a name="getting-started-determine-requirements-scaling"></a>

All clusters can be scaled up by creating a new cluster with the new, larger node type\. When scaling up a Memcached cluster the new cluster will start out empty\. When scaling up a Redis cluster, you can seed the new cluster with data from the old cluster so that the new cluster is populated with data when you start using it\.

Memcached and Redis multiple shard clusters can be scaled out or in\. To scale a Memcached cluster out or in you merely add or remove nodes from the cluster\. If you have enabled Automatic Discovery and your application is connecting to the cluster’s configuration endpoint, you do not need to make any changes in your application when you add or remove nodes\.

To scale a Redis multiple shard cluster, you add or remove shards from the cluster\. If your application is connecting to the cluster’s configuration endpoint, you do not need to make any changes in your application when you add or remove shards\.

For more information, see [Scaling](Scaling.md) in this guide\.

## Failover Requirements<a name="getting-started-determine-requirements-failover"></a>

Depending upon how critical access to a cluster’s data is to your business, you may want to enable automatic failover on your cluster\. When Automatic Failover is enabled, if a Redis Primary node fails for any reason, one of the shard’s read\-only replica nodes is promoted to Primary\. If Automatic Failover is not enabled and the Primary fails, ElastiCache for Redis spins up a new node to fill the Primary role\. This operation is much more time consuming than failing over to a replica node\.

Automatic failover is only available on clusters running the Redis engine with multiple nodes\.

For more information, see [Replication: Multi\-AZ with Automatic Failover \(Redis\)](AutoFailover.md) in this guide\.

## Access Requirements<a name="getting-started-determine-requirements-access"></a>

By design, Amazon ElastiCache are access from Amazon EC2 instances\. Network access to an ElastiCache cluster is limited to the user account that created the cluster\. Therefore, before you can access a cluster from an Amazon EC2 instance, you must authorize the Amazon EC2 instance to access the cluster\. The steps to do this vary, depending upon whether you launched into EC2\-VPC or EC2\-Classic\.

If you launched your cluster into EC2\-VPC you need to grant network ingress to the cluster\. If you launched your cluster into EC2\-Classic you need to grant the Amazon Elastic Compute Cloud security group associated with the instance access to your ElastiCache security group\. For detailed instructions, see [Step 4: Authorize Access](GettingStarted.AuthorizeAccess.md) in this guide\.

## Region and Availability Zone Requirements<a name="getting-started-determine-requirements-region"></a>

Amazon ElastiCache supports all AWS regions\. By locating your ElastiCache clusters in a region close to your application you can reduce latency\. If your cluster has multiple nodes, locating your nodes in different Availability Zones can reduce the impact of failures on your cluster\.

For more information, see:

+ [Choosing Regions and Availability Zones](RegionsAndAZs.md)

+ [Mitigating Failures](FaultTolerance.md)