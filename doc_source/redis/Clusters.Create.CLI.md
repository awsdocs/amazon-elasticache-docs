# Creating a Cluster \(AWS CLI\)<a name="Clusters.Create.CLI"></a>

To create a cluster using the AWS CLI, use the `create-cache-cluster` command\.

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not actively using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a Cluster](Clusters.Delete.md)\. 

**Topics**
+ [Creating a Cache Cluster for Redis \(Cluster Mode Disabled\) \(AWS CLI\)](#Clusters.Create.CLI.Redis)
+ [Creating a Redis \(Cluster Mode Enabled\) Cluster \(AWS CLI\)](#Clusters.Create.CLI.RedisCluster)

## Creating a Cache Cluster for Redis \(Cluster Mode Disabled\) \(AWS CLI\)<a name="Clusters.Create.CLI.Redis"></a>

**Example – A Redis \(cluster mode disabled\) Cluster with no read replicas**  
The following CLI code creates a Redis \(cluster mode disabled\) cache cluster with no replicas\.  
If you want to enable in\-transit or at\-rest encryption on this cluster, add these parameters:  
+ `-transit-encryption-enabled`

  If you enable in\-transit encryption, the cluster must be created in a VPC based on the Amazon VPC service\. You must also include the parameter `--cache-subnet-group`\.
+ `--auth-token` with the customer specified string value for your AUTH token \(password\) needed to perform operations on this cluster\.
+ `--at-rest-encryption-enabled`
For Linux, macOS, or Unix:  

```
aws elasticache create-cache-cluster \
--cache-cluster-id my-cluster \
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
--cache-cluster-id my-cluster ^
--cache-node-type cache.r4.large ^
--engine redis ^
--engine-version 3.2.4 ^
--num-cache-nodes 1 ^
--cache-parameter-group default.redis3.2 ^
--snapshot-arns arn:aws:s3:myS3Bucket/snap.rdb
```

## Creating a Redis \(Cluster Mode Enabled\) Cluster \(AWS CLI\)<a name="Clusters.Create.CLI.RedisCluster"></a>

Redis \(cluster mode enabled\) clusters \(API/CLI: replication groups\) cannot be created using the `create-cache-cluster` operation\. To create a Redis \(cluster mode enabled\) cluster \(API/CLI: replication group\), see [Creating a Redis \(Cluster Mode Enabled\) Replication Group from Scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.CLI)\.

For more information, see the AWS CLI for ElastiCache reference topic [https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)\.