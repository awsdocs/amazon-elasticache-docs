# Creating a Cache Cluster \(AWS CLI\)<a name="Clusters.Create.CLI"></a>

To create a cluster using the AWS CLI, use the `create-cache-cluster` command\. The following example creates a single node Redis \(cluster mode enabled\) cluster named *my\-redis\-cluster* and seeds it with the snapshot file `snap.rdb` that has been copied to Amazon S3\.

If you want a Redis cluster that supports replication, see [Creating a Redis \(cluster mode disabled\) Cluster with Replicas from Scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.CLI)\.

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not actively using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a Cluster](Clusters.Delete.md)\. 


+ [Creating a Memcached Cache Cluster \(AWS CLI\)](#Clusters.Create.CLI.Memcached)
+ [Creating a Redis \(cluster mode disabled\) Cache Cluster \(AWS CLI\)](#Clusters.Create.CLI.Redis)
+ [Creating a Redis \(cluster mode enabled\) Cluster \(AWS CLI\)](#Clusters.Create.CLI.RedisCluster)

## Creating a Memcached Cache Cluster \(AWS CLI\)<a name="Clusters.Create.CLI.Memcached"></a>

The following CLI code creates a Memcached cache cluster\.

For Linux, macOS, or Unix:

```
aws elasticache create-cache-cluster \
    --cache-cluster-id my-memcached-cluster \
    --cache-node-type cache.r4.large \
    --engine memcached \
    --engine-version 1.4.24 \
    --cache-parameter-group default.memcached1.4 \
    --num-cache-nodes 3
```

For Windows:

```
aws elasticache create-cache-cluster ^
    --cache-cluster-id my-memcached-cluster ^
    --cache-node-type cache.r4.large ^
    --engine memcached ^
    --engine-version 1.4.24 ^
    --cache-parameter-group default.memcached1.4 ^
    --num-cache-nodes 3
```

## Creating a Redis \(cluster mode disabled\) Cache Cluster \(AWS CLI\)<a name="Clusters.Create.CLI.Redis"></a>

**Example – A Redis \(cluster mode disabled\) Cluster with no read replicas**  
The following CLI code creates a Redis \(cluster mode disabled\) cache cluster with no replicas\.  
For Linux, macOS, or Unix:  

```
aws elasticache create-cache-cluster \
    --cache-cluster-id my-redis3-cluster \
    --cache-node-type cache.r4.large \
    --engine redis \
    --engine-version 3.2.4 \
    --num-cache-nodes 1 \
    --cache-parameter-group default.redis3.2 \
    --snapshot-arns arn:aws:s3:myS3Bucket/snap.rdb
```
For Windows:  

```
aws elasticache create-cache-cluster ^
    --cache-cluster-id my-redis3-cluster ^
    --cache-node-type cache.r4.large ^
    --engine redis ^
    --engine-version 3.2.4 ^
    --num-cache-nodes 1 ^
    --cache-parameter-group default.redis3.2 ^
    --snapshot-arns arn:aws:s3:myS3Bucket/snap.rdb
```

## Creating a Redis \(cluster mode enabled\) Cluster \(AWS CLI\)<a name="Clusters.Create.CLI.RedisCluster"></a>

Redis \(cluster mode enabled\) clusters \(API/CLI: replication groups\) cannot be created using the `create-cache-cluster` operation\. To create a Redis \(cluster mode enabled\) cluster \(API/CLI: replication group\), see [Creating a Redis \(cluster mode enabled\) Cluster with Replicas from Scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.CLI)\.

For more information, go to the AWS CLI for ElastiCache reference topic [http://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html](http://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)\.