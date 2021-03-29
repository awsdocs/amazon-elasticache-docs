# Deleting a cluster<a name="Clusters.Delete"></a>

As long as a cluster is in the *available* state, you are being charged for it, whether or not you are actively using it\. To stop incurring charges, delete the cluster\.

**Warning**  
When you delete an ElastiCache for Redis cluster, your manual snapshots are retained\. You can also create a final snapshot before the cluster is deleted\. Automatic cache snapshots are not retained\.

## Using the AWS Management Console<a name="Clusters.Delete.CON"></a>

The following procedure deletes a single cluster from your deployment\. To delete multiple clusters, repeat the procedure for each cluster that you want to delete\. You do not need to wait for one cluster to finish deleting before starting the procedure to delete another cluster\.

**To delete a cluster**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the ElastiCache console dashboard, choose the engine the cluster that you want to delete is running\.

   A list of all clusters running that engine appears\.

1. To choose the cluster to delete, choose the cluster's name from the list of clusters\.
**Important**  
You can only delete one cluster at a time from the ElastiCache console\. Choosing multiple clusters disables the delete operation\.

1. For **Actions**, choose **Delete**\.

1. In the **Delete Cluster** confirmation screen, choose **Delete** to delete the cluster, or choose **Cancel** to keep the cluster\.

   If you chose **Delete**, the status of the cluster changes to *deleting*\.

As soon as your cluster is no longer listed in the list of clusters, you stop incurring charges for it\.

## Using the AWS CLI<a name="Clusters.Delete.CLI"></a>

The following code deletes the cache cluster `my-cluster`\.

```
aws elasticache delete-cache-cluster --cache-cluster-id my-cluster
```

The `delete-cache-cluster` CLI action only deletes one cache cluster\. To delete multiple cache clusters, call `delete-cache-cluster` for each cache cluster that you want to delete\. You do not need to wait for one cache cluster to finish deleting before deleting another\.

For Linux, macOS, or Unix:

```
aws elasticache delete-cache-cluster \
    --cache-cluster-id my-cluster \
    --region us-east-2
```

For Windows:

```
aws elasticache delete-cache-cluster ^
    --cache-cluster-id my-cluster ^
    --region us-east-2
```

For more information, see the AWS CLI for ElastiCache topic [https://docs.aws.amazon.com/cli/latest/reference/elasticache/delete-cache-cluster.html](https://docs.aws.amazon.com/cli/latest/reference/elasticache/delete-cache-cluster.html)\.

## Using the ElastiCache API<a name="Clusters.Delete.API"></a>

The following code deletes the cluster `my-cluster`\.

```
https://elasticache.us-west-2.amazonaws.com/    
    ?Action=DeleteCacheCluster
    &CacheClusterId=my-cluster
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

The `DeleteCacheCluster` API operation only deletes one cache cluster\. To delete multiple cache clusters, call `DeleteCacheCluster` for each cache cluster that you want to delete\. You do not need to wait for one cache cluster to finish deleting before deleting another\.

For more information, see the ElastiCache API reference topic [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheCluster.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheCluster.html)\.