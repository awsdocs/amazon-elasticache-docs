# Deleting a Cluster<a name="Clusters.Delete"></a>

As long as a cluster is in the *available* state, you are being charged for it, whether or not you are actively using it\. To stop incurring charges, delete the cluster\.

## Deleting a Cluster \(Console\)<a name="Clusters.Delete.CON"></a>

The following procedure deletes a single cluster from your deployment\. To delete multiple clusters, repeat the procedure for each cluster you want to delete\. You do not need to wait for one cluster to finish deleting before starting the procedure to delete another cluster\.

**To delete a cluster**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the ElastiCache console dashboard, select the engine the cluster you want to delete is running, either Memcached or Redis\.

   A list of all clusters running the selected engine appears\.

1. To select the cluster to delete, select the cluster's name from the list of clusters\.
**Important**  
You can only delete one cluster at a time from the ElastiCache console\. Selecting multiple clusters disables the delete operation\.

1. Select the **Actions** button and then select **Delete** from the list of actions\.

1. In the **Delete Cluster** confirmation screen:

   1. If this is a Redis cluster, specify whether or not a final snapshot should be taken, and, if you want a final snapshot, the name of the snapshot\.

   1. Choose **Delete** to delete the cluster, or select **Cancel** to keep the cluster\.

   If you chose **Delete**, the status of the cluster changes to *deleting*\.

As soon as your cluster is no longer listed in the list of clusters, you stop incurring charges for it\.

## Deleting a Cache Cluster \(AWS CLI\)<a name="Clusters.Delete.CLI"></a>

The following code deletes the cache cluster `myCluster`\.

```
aws elasticache delete-cache-cluster --cache-cluster-id myCluster
```

The `delete-cache-cluster` CLI action only deletes one cache cluster\. To delete multiple cache clusters, call `delete-cache-cluster` for each cache cluster you want to delete\. You do not need to wait for one cache cluster to finish deleting before deleting another\.

For Linux, macOS, or Unix:

```
aws elasticache delete-cache-cluster \
    --cache-cluster-id myCluster \
    --region us-east-2
```

For Windows:

```
aws elasticache delete-cache-cluster ^
    --cache-cluster-id myCluster ^
    --region us-east-2
```

For more information, go to the AWS CLI for ElastiCache topic [http://docs.aws.amazon.com/cli/latest/reference/elasticache/delete-cache-cluster.html](http://docs.aws.amazon.com/cli/latest/reference/elasticache/delete-cache-cluster.html)\.

## Deleting a Cache Cluster \(ElastiCache API\)<a name="Clusters.Delete.API"></a>

The following code deletes the cluster `myCluster`\.

```
https://elasticache.us-west-2.amazonaws.com/    
    ?Action=DeleteCacheCluster
    &CacheClusterId=myCluster
    &Region us-east-2
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20150202T220302Z
    &X-Amz-Algorithm=AWS4-HMAC-SHA256
    &X-Amz-Date=20150202T220302Z
    &X-Amz-SignedHeaders=Host
    &X-Amz-Expires=20150202T220302Z
    &X-Amz-Credential=<credential>
    &X-Amz-Signature=<signature>
```

The `DeleteCacheCluster` API operation only deletes one cache cluster\. To delete multiple cache clusters, call `DeleteCacheCluster` for each cache cluster you want to delete\. You do not need to wait for one cache cluster to finish deleting before deleting another\.

For more information, go to the ElastiCache API reference topic [http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheCluster.html](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheCluster.html)\.