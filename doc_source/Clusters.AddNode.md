# Adding Nodes to a Cluster<a name="Clusters.AddNode"></a>

Adding nodes to a cluster currently applies only if you are running Memcached or Redis \(cluster mode disabled\)\. If you are running Redis \(cluster mode disabled\), the nodes you add to the cluster are replica nodes\.

You can use the ElastiCache Management Console, the AWS CLI or ElastiCache API to add nodes to your cluster\.

Each time you change the number of nodes in your Memcached cluster, you must re\-map at least some of your keyspace so it maps to the correct node\. For more detailed information on load balancing your Memcached cluster, see [Configuring Your ElastiCache Client for Efficient Load Balancing](BestPractices.LoadBalancing.md)\.

## Adding Nodes to a Cluster \(Console\)<a name="Clusters.AddNode.CON"></a>

The process to add a node to a Memcached or Redis \(cluster mode disabled\) cluster with replication enabled is the same\. If you want to add a node to a single\-node Redis \(cluster mode disabled\) cluster \(one without replication enabled\), it's a two\-step process: first add replication, and then add a replica node\.

**Topics**

+ [To add replication to a Redis cluster with no shards](#AddReplication.CON)

+ [To add nodes to a Memcached or Redis \(cluster mode disabled\) cluster with one shard \(console\)](#AddNode.CON)

The following procedure adds replication to a single\-node Redis that does not have replication enabled\. When you add replication, the existing node becomes the primary node in the replication\-enabled cluster\. After replication is added, you can add up to 5 replica nodes to the cluster\.

**To add replication to a Redis cluster with no shards**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Redis**\.

   A list of clusters running the Redis engine is displayed\.

1. Choose the name of a cluster that you want to add to\.

   The following is true of a Redis cluster that does not have replication enabled:

   + It is running Redis, not Clustered Redis\.

   + It has zero shards\.

     If the cluster has any shards, replication is already enabled on it and you can continue at [To add nodes to a Memcached or Redis \(cluster mode disabled\) cluster with one shard \(console\)](#AddNode.CON)\.

1. Choose **Add replication**\.

1. In **Add Replication**, type a description for this replication\-enabled cluster\.

1. Choose **Add**\.

   As soon as the cluster's status returns to *available* you can continue at the next procedure and add replicas to the cluster\.

**To add nodes to a Memcached or Redis \(cluster mode disabled\) cluster with one shard \(console\)**

The following procedure can be used to add nodes to a Memcached cluster or Redis \(cluster mode disabled\) cluster which has replication enabled\. Currently you cannot add or remove nodes from a Redis \(cluster mode enabled\) cluster\.

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Memcached** or **Redis**\.

   A list of clusters running the chosen engine appears\.

1. From the list of clusters, choose the name of the cluster you want to add a node to\. This cannot be a cluster running the Clustered Redis engine or a Redis cluster with zero shards\.

   A list of the cluster's nodes appears and the **Add node** button becomes active\.

1. Choose **Add node** at the top of the page\.

1. In **Add Node**, complete the information requested in the **Add Node** \(Memcached\) or **Add Read Replica to Cluster** \(Redis\) dialog box\.

1. Choose the **Apply Immediately \- Yes** button to apply this change immediately, or choose **No** to postpone the change until your next maintenance window\.  
**Impact of New Add and Remove Requests on Pending Requests**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/Clusters.AddNode.html)

   To determine what operations are pending, choose the **Description** tab and check to see how many pending creations or deletions are shown\. You cannot have both pending creations and pending deletions\.   
![\[Image: Cluster description tab\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ModifyCacheCluster-DescriptionTab-PendActions.png)

1. Choose the **Add** button\.

    After a few moments, the new nodes should appear in the nodes list with a status of **creating**\. If they don't appear, refresh your browser page\.

## Adding Nodes to a Cache Cluster \(AWS CLI\)<a name="Clusters.AddNode.CLI"></a>

If you want to add nodes to an existing Redis \(cluster mode disabled\) replication group \(console: Cluster\) that does not have replication enabled, you must first create the replication group specifying the existing cluster as the primary\. For more information, see [Creating a Replication Group Using an Available Redis Cache Cluster \(AWS CLI\)](Replication.CreatingReplGroup.ExistingCluster.md#Replication.CreatingReplGroup.ExistingCluster.CLI)\. After the replication group is *available*, you can continue with the following process\.

To add nodes to a cluster using the AWS CLI, use the AWS CLI operation `modify-cache-cluster` with the following parameters:

+ `--cache-cluster-id` The ID of the cache cluster you want to add nodes to\.

+ `--num-cache-nodes` The `--num-cache-nodes` parameter specifies the number of nodes you want in this cluster after the modification is applied\. To add nodes to this cluster, `--num-cache-nodes` must be greater than the current number of nodes in this cluster\. If this value is less than the current number of nodes, ElastiCache expects the parameter `cache-node-ids-to-remove` and a list of nodes to remove from the cluster\. For more information, see [Removing Nodes from a Cluster \(AWS CLI\)](Clusters.DeleteNode.md#Clusters.DeleteNode.CLI)\.

+ `--apply-immediately` or `--no-apply-immediately` which specifies whether to add these nodes immediately or at the next maintenance window\.

For Linux, macOS, or Unix:

```
aws elasticache modify-cache-cluster \
    --cache-cluster-id my-cache-cluster \
    --num-cache-nodes 5 \
    --apply-immediately
```

For Windows:

```
aws elasticache modify-cache-cluster ^
    --cache-cluster-id my-cache-cluster ^
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
        "CacheClusterId": "my-cache-cluster", 
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

For more information, see the AWS CLI topic [http://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-cluster.html](http://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-cluster.html)\.

## Adding Nodes to a Cache Cluster \(ElastiCache API\)<a name="Clusters.AddNode.API"></a>

If you want to add nodes to an existing Redis \(cluster mode disabled\) replication group \(console: Cluster\) that does not have replication enabled, you must first create the replication group specifying the existing cluster as the Primary\. For more information, see [Adding Replicas to a Stand\-Alone Redis \(cluster mode disabled\) Cluster \(ElastiCache API\)](Replication.CreatingReplGroup.ExistingCluster.md#Replication.CreatingReplGroup.ExistingCluster.API)\. After the replication group is *available*, you can continue with the following process\.

**To add nodes to a cluster \(ElastiCache API\)**

+ Call the `ModifyCacheCluster` API operation with the following parameters:

  + `CacheClusterId` The ID of the cluster you want to add nodes to\.

  + `NumCacheNodes` The `NumCachNodes` parameter specifies the number of nodes you want in this cluster after the modification is applied\. To add nodes to this cluster, `NumCacheNodes` must be greater than the current number of nodes in this cluster\. If this value is less than the current number of nodes, ElastiCache expects the parameter `CacheNodeIdsToRemove` with a list of nodes to remove from the cluster \(see [Removing Nodes from a Cluster \(ElastiCache API\)](Clusters.DeleteNode.md#Clusters.DeleteNode.API)\)\.

  + `ApplyImmediately` Specifies whether to add these nodes immediately or at the next maintenance window\.

  + `Region` Specifies the AWS region of the cluster you want to add nodes to\.

  The following example shows a call to add nodes to a cluster\.  
**Example**  

  ```
  https://elasticache.us-west-2.amazonaws.com/
      ?Action=ModifyCacheCluster
      &ApplyImmediately=true
      &NumCacheNodes=5
      &CacheClusterId=myCacheCluster
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

For more information, see ElastiCache API topic [http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html)\.