# Creating a cluster \(AWS CLI\)<a name="Clusters.Create.CLI"></a>

To create a cluster using the AWS CLI, use the `create-cache-cluster` command\.

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not actively using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a cluster](Clusters.Delete.md)\. 

**Topics**
+ [Creating a Memcached Cache Cluster \(AWS CLI\)](#Clusters.Create.CLI.Memcached)

## Creating a Memcached Cache Cluster \(AWS CLI\)<a name="Clusters.Create.CLI.Memcached"></a>

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