# Creating a cluster<a name="Clusters.Create"></a>

The following examples show how to create a cluster using the AWS Management Console, AWS CLI and ElastiCache API\.

## Creating a Memcached cluster \(console\)<a name="Clusters.Create.CON.Memcached"></a>

When you use the Memcached engine, Amazon ElastiCache supports horizontally partitioning your data over multiple nodes\. Memcached enables auto discovery so you don't need to keep track of the endpoints for each node\. Memcached tracks each node's endpoint, updating the endpoint list as nodes are added and removed\. All your application needs to interact with the cluster is the configuration endpoint\. For more information on auto discovery, see [Automatically identify nodes in your cluster](AutoDiscovery.md)\.

To create a Memcachd cluster, follow the steps at [Create a cluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/GettingStarted.CreateCluster.html)

As soon as your cluster's status is *available*, you can grant Amazon EC2 access to it, connect to it, and begin using it\. For more information, see [Step 3: Authorize access to the cluster](GettingStarted.AuthorizeAccess.md) and [Step 4: Connect to the cluster's node](GettingStarted.ConnectToCacheNode.md)\.

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not actively using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a cluster](Clusters.Delete.md)\. 

## Creating a cluster \(AWS CLI\)<a name="Clusters.Create.CLI"></a>

To create a cluster using the AWS CLI, use the `create-cache-cluster` command\.

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not actively using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a cluster](Clusters.Delete.md)\. 

### Creating a Memcached Cache Cluster \(AWS CLI\)<a name="Clusters.Create.CLI.Memcached"></a>

The following CLI code creates a Memcached cache cluster with 3 nodes\.

For Linux, macOS, or Unix:

```
aws elasticache create-cache-cluster \
--cache-cluster-id my-cluster \
--cache-node-type cache.r4.large \
--engine memcached \
--engine-version 1.4.24 \
--cache-parameter-group default.memcached1.4 \
--num-cache-nodes 3
```

For Windows:

```
aws elasticache create-cache-cluster ^
--cache-cluster-id my-cluster ^
--cache-node-type cache.r4.large ^
--engine memcached ^
--engine-version 1.4.24 ^
--cache-parameter-group default.memcached1.4 ^
--num-cache-nodes 3
```

## Creating a cluster \(ElastiCache API\)<a name="Clusters.Create.API"></a>

To create a cluster using the ElastiCache API, use the `CreateCacheCluster` action\. 

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a cluster](Clusters.Delete.md)\. 

**Topics**

### Creating a Memcached cache cluster \(ElastiCache API\)<a name="Clusters.Create.API.Memcached"></a>

The following code creates a Memcached cluster with 3 nodes \(ElastiCache API\)\.

Line breaks are added for ease of reading\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=CreateCacheCluster
    &CacheClusterId=my-cluster
    &CacheNodeType=cache.r4.large
    &Engine=memcached
    &NumCacheNodes=3
    &SignatureVersion=4       
    &SignatureMethod=HmacSHA256
    &Timestamp=20150508T220302Z
    &Version=2015-02-02
    &X-Amz-Algorithm=&AWS;4-HMAC-SHA256
    &X-Amz-Credential=<credential>
    &X-Amz-Date=20150508T220302Z
    &X-Amz-Expires=20150508T220302Z
    &X-Amz-SignedHeaders=Host
    &X-Amz-Signature=<signature>
```