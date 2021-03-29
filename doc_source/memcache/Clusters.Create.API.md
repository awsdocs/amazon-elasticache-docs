# Creating a cluster \(ElastiCache API\)<a name="Clusters.Create.API"></a>

To create a cluster using the ElastiCache API, use the `CreateCacheCluster` action\. 

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a cluster](Clusters.Delete.md)\. 

**Topics**
+ [Creating a Memcached cache cluster \(ElastiCache API\)](#Clusters.Create.API.Memcached)

## Creating a Memcached cache cluster \(ElastiCache API\)<a name="Clusters.Create.API.Memcached"></a>

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
    &X-Amz-Algorithm=AWS4-HMAC-SHA256
    &X-Amz-Credential=<credential>
    &X-Amz-Date=20150508T220302Z
    &X-Amz-Expires=20150508T220302Z
    &X-Amz-SignedHeaders=Host
    &X-Amz-Signature=<signature>
```