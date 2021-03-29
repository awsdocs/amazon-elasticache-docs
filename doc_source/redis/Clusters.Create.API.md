# Creating a cluster \(ElastiCache API\)<a name="Clusters.Create.API"></a>

To create a cluster using the ElastiCache API, use the `CreateCacheCluster` action\. 

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a cluster](Clusters.Delete.md)\. 

**Topics**
+ [Creating a Redis \(Cluster Mode Disabled\) cache cluster \(ElastiCache API\)](#Clusters.Create.API.Redis)
+ [Creating a cache cluster in Redis \(Cluster Mode Enabled\) \(ElastiCache API\)](#Clusters.Create.API.RedisCluster)

## Creating a Redis \(Cluster Mode Disabled\) cache cluster \(ElastiCache API\)<a name="Clusters.Create.API.Redis"></a>

The following code creates a Redis \(cluster mode disabled\) cache cluster \(ElastiCache API\)\.

Line breaks are added for ease of reading\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=CreateCacheCluster
    &CacheClusterId=my-cluster
    &CacheNodeType=cache.r4.large
    &CacheParameterGroup=default.redis3.2
    &Engine=redis
    &EngineVersion=3.2.4
    &NumCacheNodes=1
    &SignatureVersion=4       
    &SignatureMethod=HmacSHA256
    &SnapshotArns.member.1=arn%3Aaws%3As3%3A%3A%3AmyS3Bucket%2Fdump.rdb
    &Timestamp=20150508T220302Z
    &Version=2015-02-02
    &X-Amz-Algorithm=AWS4-HMAC-SHA256
    &X-Amz-Credential=<credential>
    &X-Amz-Date=20150508T220302Z
    &X-Amz-Expires=20150508T220302Z
    &X-Amz-SignedHeaders=Host
    &X-Amz-Signature=<signature>
```

## Creating a cache cluster in Redis \(Cluster Mode Enabled\) \(ElastiCache API\)<a name="Clusters.Create.API.RedisCluster"></a>

Redis \(cluster mode enabled\) clusters \(API/CLI: replication groups\) cannot be created using the `CreateCacheCluster` operation\. To create a Redis \(cluster mode enabled\) cluster \(API/CLI: replication group\), see [Creating a replication group in Redis \(Cluster Mode Enabled\) from scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.API)\.

For more information, see the ElastiCache API reference topic [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)\.