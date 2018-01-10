# Creating a Cache Cluster \(ElastiCache API\)<a name="Clusters.Create.API"></a>

To create a cluster using the ElastiCache API, use the `CreateCacheCluster` action\. The following example creates a single node Redis cluster named *my\-redis\-cluster* and seeds it with the snapshot file `dump.rdb` that has been copied to Amazon S3\.

If you want a Redis cluster that supports replication, see [Creating a Redis \(cluster mode disabled\) Cluster with Replicas from Scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.API)\.

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a Cluster](Clusters.Delete.md)\. 


+ [Creating a Memcached Cache Cluster \(ElastiCache API\)](#Clusters.Create.API.Memcached)
+ [Creating a Redis \(cluster mode disabled\) Cache Cluster \(ElastiCache API\)](#Clusters.Create.API.Redis)
+ [Creating a Redis \(cluster mode enabled\) Cache Cluster \(ElastiCache API\)](#Clusters.Create.API.RedisCluster)

## Creating a Memcached Cache Cluster \(ElastiCache API\)<a name="Clusters.Create.API.Memcached"></a>

The following code creates a Memcached cluster with 4 nodes \(ElastiCache API\)\.

Line breaks are added for ease of reading\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=CreateCacheCluster
    &CacheClusterId=myMemcachedCluster
    &CacheNodeType=cache.r4.large
    &Engine=memcached
    &NumCacheNodes=4
    &SignatureVersion=4       
    &SignatureMethod=HmacSHA256
    &Timestamp=20150508T220302Z
    &Version=2015-02-02
    &X-Amz-Algorithm=AWS4-HMAC-SHA256
    &X-Amz-Credential=<credential>
    &X-Amz-Date=20150508T220302Z
    &X-Amz-Expires=20150508T220302Z
    &X-Amz-SignedHeaders=Host
    &X-Amz-Signature=<signature>
```

## Creating a Redis \(cluster mode disabled\) Cache Cluster \(ElastiCache API\)<a name="Clusters.Create.API.Redis"></a>

The following code creates a Redis \(cluster mode disabled\) cache cluster \(ElastiCache API\)\.

Line breaks are added for ease of reading\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=CreateCacheCluster
    &CacheClusterId=my-redis2-cluster
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

## Creating a Redis \(cluster mode enabled\) Cache Cluster \(ElastiCache API\)<a name="Clusters.Create.API.RedisCluster"></a>

Redis \(cluster mode enabled\) clusters \(API/CLI: replication groups\) cannot be created using the `CreateCacheCluster` operation\. To create a Redis \(cluster mode enabled\) cluster \(API/CLI: replication group\), see [Creating a Redis \(cluster mode enabled\) Cluster with Replicas from Scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.API)\.

For more information, go to the ElastiCache API reference topic [http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)\.