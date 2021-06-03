# Adding nodes to a cluster<a name="Clusters.AddNode"></a>

Adding nodes to a Memcached cluster increases the number of you cluster's partitions\. When you change the number of partitions in a cluster some of your key spaces need to be remapped so that they are mapped to the right node\. Remapping key spaces temporarily increases the number of cache misses on the cluster\. For more information, see [Configuring your ElastiCache client for efficient load balancing](BestPractices.LoadBalancing.md)\.

You can use the ElastiCache Management Console, the AWS CLI or ElastiCache API to add nodes to your cluster\.

## Using the AWS Management Console<a name="Clusters.AddNode.CON"></a>

**Topics**
+ [To add nodes to a cluster \(console\)](#AddNode.CON)<a name="AddNode.CON"></a>

**To add nodes to a cluster \(console\)**

The following procedure can be used to add nodes to a cluster\.

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose the engine running on the cluster that you want to add nodes to\.

   A list of clusters running the chosen engine appears\.

1. From the list of clusters, for the cluster that you want to add a node to, choose its name\.

1. Choose **Add node**\.

1. Complete the information requested in the **Add Node** dialog box\.

1. Choose the **Apply Immediately \- Yes** button to add this node immediately, or choose **No** to add this node during the cluster's next maintenance window\.  
**Impact of New Add and Remove Requests on Pending Requests**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/Clusters.AddNode.html)

   To determine what operations are pending, choose the **Description** tab and check to see how many pending creations or deletions are shown\. You cannot have both pending creations and pending deletions\.   
![\[Image: Cluster description tab\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ModifyCacheCluster-DescriptionTab-PendActions.png)

1. Choose the **Add** button\.

    After a few moments, the new nodes should appear in the nodes list with a status of **creating**\. If they don't appear, refresh your browser page\. When the node's status changes to *available* the new node is able to be used\.

## Using the AWS CLI<a name="Clusters.AddNode.CLI"></a>

To add nodes to a cluster using the AWS CLI, use the AWS CLI operation `modify-cache-cluster` with the following parameters:
+ `--cache-cluster-id` The ID of the cache cluster that you want to add nodes to\.
+ `--num-cache-nodes` The `--num-cache-nodes` parameter specifies the number of nodes that you want in this cluster after the modification is applied\. To add nodes to this cluster, `--num-cache-nodes` must be greater than the current number of nodes in this cluster\. If this value is less than the current number of nodes, ElastiCache expects the parameter `cache-node-ids-to-remove` and a list of nodes to remove from the cluster\. For more information, see [Using the AWS CLI](Clusters.DeleteNode.md#Clusters.DeleteNode.CLI)\.
+ `--apply-immediately` or `--no-apply-immediately` which specifies whether to add these nodes immediately or at the next maintenance window\.

For Linux, OS X, or Unix:

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
      &X-Amz-Algorithm=&AWS;4-HMAC-SHA256
      &X-Amz-Date=20141201T220302Z
      &X-Amz-SignedHeaders=Host
      &X-Amz-Expires=20141201T220302Z
      &X-Amz-Credential=<credential>
      &X-Amz-Signature=<signature>
  ```

For more information, see ElastiCache API topic [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html)\.