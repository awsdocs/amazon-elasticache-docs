# Adding nodes to a cluster<a name="Clusters.AddNode"></a>

To reconfigure your Redis \(cluster mode enabled\) cluster, see [Scaling clusters in Redis \(Cluster Mode Enabled\)](scaling-redis-cluster-mode-enabled.md)

You can use the ElastiCache Management Console, the AWS CLI or ElastiCache API to add nodes to your cluster\.

## Using the AWS Management Console<a name="Clusters.AddNode.CON"></a>

If you want to add a node to a single\-node Redis \(cluster mode disabled\) cluster \(one without replication enabled\), it's a two\-step process: first add replication, and then add a replica node\.

**Topics**
+ [To add replication to a Redis cluster with no shards](#AddReplication.CON)
+ [To add nodes to a cluster \(console\)](#AddNode.CON)

The following procedure adds replication to a single\-node Redis that does not have replication enabled\. When you add replication, the existing node becomes the primary node in the replication\-enabled cluster\. After replication is added, you can add up to 5 replica nodes to the cluster\.<a name="AddReplication.CON"></a>

**To add replication to a Redis cluster with no shards**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Redis**\.

   A list of clusters running the Redis engine is displayed\.

1. Choose the name of a cluster, not the box to the left of the cluster's name, that you want to add nodes to\.

   The following is true of a Redis cluster that does not have replication enabled:
   + It is running Redis, not Clustered Redis\.
   + It has zero shards\.

     If the cluster has any shards, replication is already enabled on it and you can continue at [To add nodes to a cluster \(console\)](#AddNode.CON)\.

1. Choose **Add replication**\.

1. In **Add Replication**, enter a description for this replication\-enabled cluster\.

1. Choose **Add**\.

   As soon as the cluster's status returns to *available* you can continue at the next procedure and add replicas to the cluster\.<a name="AddNode.CON"></a>

**To add nodes to a cluster \(console\)**

The following procedure can be used to add nodes to a cluster\.

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose the engine running on the cluster that you want to add nodes to\.

   A list of clusters running the chosen engine appears\.

1. From the list of clusters, for the cluster that you want to add a node to, choose its name\.

   If your cluster is a Redis \(cluster mode enabled\) cluster, see [Scaling clusters in Redis \(Cluster Mode Enabled\)](scaling-redis-cluster-mode-enabled.md)\.

   If your cluster is a Redis \(cluster mode disabled\) cluster with zero shards, first complete the steps at [To add replication to a Redis cluster with no shards](#AddReplication.CON)\.

1. Choose **Add node**\.

1. Complete the information requested in the **Add Node** dialog box\.

1. Choose the **Apply Immediately \- Yes** button to add this node immediately, or choose **No** to add this node during the cluster's next maintenance window\.  
**Impact of New Add and Remove Requests on Pending Requests**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Clusters.AddNode.html)

   To determine what operations are pending, choose the **Description** tab and check to see how many pending creations or deletions are shown\. You cannot have both pending creations and pending deletions\.   
![\[Image: Cluster description tab\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ModifyCacheCluster-DescriptionTab-PendActions.png)

1. Choose the **Add** button\.

    After a few moments, the new nodes should appear in the nodes list with a status of **creating**\. If they don't appear, refresh your browser page\. When the node's status changes to *available* the new node is able to be used\.

## Using the AWS CLI<a name="Clusters.AddNode.CLI"></a>

If you want to add nodes to an existing Redis \(cluster mode disabled\) cluster that does not have replication enabled, you must first create the replication group specifying the existing cluster as the primary\. For more information, see [Creating a replication group using an available Redis cache cluster \(AWS CLI\)](Replication.CreatingReplGroup.ExistingCluster.md#Replication.CreatingReplGroup.ExistingCluster.CLI)\. After the replication group is *available*, you can continue with the following process\.

To add nodes to a cluster using the AWS CLI, use the AWS CLI operation `modify-cache-cluster` with the following parameters:
+ `--cache-cluster-id` The ID of the cache cluster that you want to add nodes to\.
+ `--num-cache-nodes` The `--num-cache-nodes` parameter specifies the number of nodes that you want in this cluster after the modification is applied\. To add nodes to this cluster, `--num-cache-nodes` must be greater than the current number of nodes in this cluster\. If this value is less than the current number of nodes, ElastiCache expects the parameter `cache-node-ids-to-remove` and a list of nodes to remove from the cluster\. For more information, see [Using the AWS CLI](Clusters.DeleteNode.md#Clusters.DeleteNode.CLI)\.
+ `--apply-immediately` or `--no-apply-immediately` which specifies whether to add these nodes immediately or at the next maintenance window\.

For Linux, macOS, or Unix:

```
aws elasticache modify-cache-cluster \
    --cache-cluster-id my-cluster \
    --num-cache-nodes 5 \
    --apply-immediately
```

For Windows:

```
aws elasticache modify-cache-cluster ^
    --cache-cluster-id my-cluster ^
    --num-cache-nodes 5 ^
    --apply-immediately
```

This operation produces output similar to the following \(JSON format\):

```
{
    "CacheCluster": {
        "Engine": "memcached", 
        "CacheParameterGroup": {
            "CacheNodeIdsToReboot": [], 
            "CacheParameterGroupName": "default.memcached1.4", 
            "ParameterApplyStatus": "in-sync"
        }, 
        "CacheClusterId": "my-cluster", 
        "PreferredAvailabilityZone": "us-west-2b", 
        "ConfigurationEndpoint": {
            "Port": 11211, 
            "Address": "rlh-mem000.7alc7bf-example.cfg.usw2.cache.amazonaws.com"
        }, 
        "CacheSecurityGroups": [], 
        "CacheClusterCreateTime": "2016-09-21T16:28:28.973Z", 
        "AutoMinorVersionUpgrade": true, 
        "CacheClusterStatus": "modifying", 
        "NumCacheNodes": 2, 
        "ClientDownloadLandingPage": "https://console.aws.amazon.com/elasticache/home#client-download:", 
        "SecurityGroups": [
            {
                "Status": "active", 
                "SecurityGroupId": "sg-dbe93fa2"
            }
        ], 
        "CacheSubnetGroupName": "default", 
        "EngineVersion": "1.4.24", 
        "PendingModifiedValues": {
            "NumCacheNodes": 5
        }, 
        "PreferredMaintenanceWindow": "sat:09:00-sat:10:00", 
        "CacheNodeType": "cache.m3.medium"
    }
}
```

For more information, see the AWS CLI topic [https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-cluster.html](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-cluster.html)\.

## Using the ElastiCache API<a name="Clusters.AddNode.API"></a>

If you want to add nodes to an existing Redis \(cluster mode disabled\) cluster that does not have replication enabled, you must first create the replication group specifying the existing cluster as the Primary\. For more information, see [Adding replicas to a standalone Redis \(Cluster Mode Disabled\) cluster \(ElastiCache API\)](Replication.CreatingReplGroup.ExistingCluster.md#Replication.CreatingReplGroup.ExistingCluster.API)\. After the replication group is *available*, you can continue with the following process\.

**To add nodes to a cluster \(ElastiCache API\)**
+ Call the `ModifyCacheCluster` API operation with the following parameters:
  + `CacheClusterId` The ID of the cluster that you want to add nodes to\.
  + `NumCacheNodes` The `NumCachNodes` parameter specifies the number of nodes that you want in this cluster after the modification is applied\. To add nodes to this cluster, `NumCacheNodes` must be greater than the current number of nodes in this cluster\. If this value is less than the current number of nodes, ElastiCache expects the parameter `CacheNodeIdsToRemove` with a list of nodes to remove from the cluster \(see [Using the ElastiCache API](Clusters.DeleteNode.md#Clusters.DeleteNode.API)\)\.
  + `ApplyImmediately` Specifies whether to add these nodes immediately or at the next maintenance window\.
  + `Region` Specifies the AWS Region of the cluster that you want to add nodes to\.

  The following example shows a call to add nodes to a cluster\.  
**Example**  

  ```
  https://elasticache.us-west-2.amazonaws.com/
      ?Action=ModifyCacheCluster
      &ApplyImmediately=true
      &NumCacheNodes=5
      &CacheClusterId=my-cluster
  	&Region=us-east-2
      &Version=2014-12-01
      &SignatureVersion=4
      &SignatureMethod=HmacSHA256
      &Timestamp=20141201T220302Z
      &X-Amz-Algorithm=AWS4-HMAC-SHA256
      &X-Amz-Date=20141201T220302Z
      &X-Amz-SignedHeaders=Host
      &X-Amz-Expires=20141201T220302Z
      &X-Amz-Credential=<credential>
      &X-Amz-Signature=<signature>
  ```

For more information, see ElastiCache API topic [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html)\.