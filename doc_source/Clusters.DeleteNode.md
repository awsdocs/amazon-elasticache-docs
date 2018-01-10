# Removing Nodes from a Cluster<a name="Clusters.DeleteNode"></a>

Removing nodes from a cluster applies only if you are not running the Clustered Redis engine\. 

Each time you change the number of nodes in a Memcached cluster, you must re\-map at least some of your keyspace so it maps to the correct node\. For more detailed information on load balancing a Memcached cluster, see [Configuring Your ElastiCache Client for Efficient Load Balancing](BestPractices.LoadBalancing.md)\.


+ [Removing Nodes from a Cluster \(Console\)](#Clusters.DeleteNode.CON)
+ [Removing Nodes from a Cluster \(AWS CLI\)](#Clusters.DeleteNode.CLI)
+ [Removing Nodes from a Cluster \(ElastiCache API\)](#Clusters.DeleteNode.API)

## Removing Nodes from a Cluster \(Console\)<a name="Clusters.DeleteNode.CON"></a>

**To remove nodes from a cluster \(console\)**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the dropdown in the upper right corner, choose the region of the cluster you want to remove nodes from\.

1. In the navigation pane, choose **Memcached** or **Redis**\.

   A list of clusters running the chosen engine appears\.

1. From the list of clusters, choose the cluster name from which you want to remove a node\.

   A list of the cluster's nodes appears\.

1. Choose the box to the left of the node ID for the node you want to delete\. Using the ElastiCache console, you can only delete one node at a time, so choosing multiple nodes will disable the **Delete node** button\.

   The *Delete Node* dialog appears\.

1. To delete the node, complete the **Delete Node** dialog box and choose **Delete Node**\. To not delete the node, choose the **Cancel**\.


**Impact of New Add and Remove Requests on Pending Requests**  

| Scenarios | Pending Operation | New Request | Results | 
| --- | --- | --- | --- | 
|  Scenario 1 |  Delete | Delete |  The new delete request, pending or immediate, replaces the pending delete request\. For example, if nodes 0001, 0003, and 0007 are pending deletion and a new request to delete nodes 0002 and 0004 is issued, only nodes 0002 and 0004 will be deleted\. Nodes 0001, 0003, and 0007 will not be deleted\. | 
|  Scenario 2 |  Delete |  Create |  The new create request, pending or immediate, replaces the pending delete request\. For example, if nodes 0001, 0003, and 0007 are pending deletion and a new request to create a node is issued, a new node will be created and nodes 0001, 0003, and 0007 will not be deleted\. | 
|  Scenario 3 |  Create |  Delete |  The new delete request, pending or immediate, replaces the pending create request\. For example, if there is a pending request to create two nodes and a new request is issued to delete node 0003, no new nodes will be created and node 0003 will be deleted\. | 
|  Scenario 4 |  Create |  Create |  The new create request is added to the pending create request\. For example, if there is a pending request to create two nodes and a new request is issued to create three nodes, the new requests is added to the pending request and five nodes will be created\. If the new create request is set to **Apply Immediately \- Yes**, all create requests are performed immediately\. If the new create request is set to **Apply Immediately \- No**, all create requests are pending\. | 

To determine what operations are pending, choose the **Description** tab and check to see how many pending creations or deletions are shown\. You cannot have both pending creations and pending deletions\. 

![\[Image: Cluster description tab\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ModifyCacheCluster-DescriptionTab-PendActions.png)

## Removing Nodes from a Cluster \(AWS CLI\)<a name="Clusters.DeleteNode.CLI"></a>

1. Use the command `describe-cache-cluster` to display a list of nodes for a cluster, as in the following example, and note the identifiers of the nodes you want to remove\. 

   For Linux, macOS, or Unix:

   ```
   aws elasticache describe-cache-clusters \
       --cache-cluster-id my-memcached-cluster \
       --show-cache-node-info
   ```

   For Windows:

   ```
   aws elasticache describe-cache-clusters ^
       --cache-cluster-id my-memcached-cluster ^
       --show-cache-node-info
   ```

   This operation produces output similar to the following \(JSON format\):

   ```
   {
       "CacheClusters": [
           {
               "Engine": "memcached", 
               "CacheNodes": [
                   {
                       "CacheNodeId": "0001", 
                       "Endpoint": {
                           "Port": 11211, 
                           "Address": "my-memcached-cluster.7ef-example.0001.usw2.cache.amazonaws.com"
                       }, 
                       "CacheNodeStatus": "available", 
                       "ParameterGroupStatus": "in-sync", 
                       "CacheNodeCreateTime": "2016-09-21T16:28:28.973Z", 
                       "CustomerAvailabilityZone": "us-west-2b"
                   }, 
                   {
                       "CacheNodeId": "0002", 
                       "Endpoint": {
                           "Port": 11211, 
                           "Address": "my-memcached-cluster.7ef-example.0002.usw2.cache.amazonaws.com"
                       }, 
                       "CacheNodeStatus": "available", 
                       "ParameterGroupStatus": "in-sync", 
                       "CacheNodeCreateTime": "2016-09-21T16:28:28.973Z", 
                       "CustomerAvailabilityZone": "us-west-2b"
                   }, 
                   {
                       "CacheNodeId": "0003", 
                       "Endpoint": {
                           "Port": 11211, 
                           "Address": "my-memcached-cluster.7ef-example.0003.usw2.cache.amazonaws.com"
                       }, 
                       "CacheNodeStatus": "available", 
                       "ParameterGroupStatus": "in-sync", 
                       "CacheNodeCreateTime": "2016-09-21T16:28:28.973Z", 
                       "CustomerAvailabilityZone": "us-west-2b"
                   }
               ], 
               "CacheParameterGroup": {
                   "CacheNodeIdsToReboot": [], 
                   "CacheParameterGroupName": "default.memcached1.4", 
                   "ParameterApplyStatus": "in-sync"
               }, 
               "CacheClusterId": "my-memcached-cluster", 
               "PreferredAvailabilityZone": "us-west-2b", 
               "ConfigurationEndpoint": {
                   "Port": 11211, 
                   "Address": "my-memcached-cluster.7ef-example.cfg.usw2.cache.amazonaws.com"
               }, 
               "CacheSecurityGroups": [], 
               "CacheClusterCreateTime": "2016-09-21T16:28:28.973Z", 
               "AutoMinorVersionUpgrade": true, 
               "CacheClusterStatus": "available", 
               "NumCacheNodes": 3, 
               "ClientDownloadLandingPage": "https://console.aws.amazon.com/elasticache/home#client-download:", 
               "SecurityGroups": [
                   {
                       "Status": "active", 
                       "SecurityGroupId": "sg-dbe93fa2"
                   }
               ], 
               "CacheSubnetGroupName": "default", 
               "EngineVersion": "1.4.24", 
               "PendingModifiedValues": {}, 
               "PreferredMaintenanceWindow": "sat:09:00-sat:10:00", 
               "CacheNodeType": "cache.m3.medium"
           }
       ]
   }
   ```

1. Use the `modify-cache-cluster` CLI operation with a list of the nodes to remove, as in the following example\.

   To remove nodes from a cluster using the command\-line interface, use the command `modify-cache-cluster` with the following parameters:

   + `--cache-cluster-id` The ID of the cache cluster you want to remove nodes from\.

   + `--num-cache-nodes` The `--num-cache-nodes` parameter specifies the number of nodes you want in this cluster after the modification is applied\.

   + `--cache-node-ids-to-remove` A list of node IDs you want removed from this cluster\.

   + `--apply-immediately` or `--no-apply-immediately` Specifies whether to remove these nodes immediately or at the next maintenance window\.

   + `--region` Specifies the region of the cluster you want to remove nodes from\.

   The following example immediately removes node 0001 from the cluster my\-memcached\-cluster\.

   For Linux, macOS, or Unix:

   ```
   aws elasticache modify-cache-cluster \
       --cache-cluster-id my-memcached-cluster \
       --num-cache-nodes 2 \
       --cache-node-ids-to-remove 0001 \
       --region us-east-2 \
       --apply-immediately
   ```

   For Windows:

   ```
   aws elasticache modify-cache-cluster ^
       --cache-cluster-id my-memcached-cluster ^
       --num-cache-nodes 2 ^
       --cache-node-ids-to-remove 0001 ^
       --region us-east-2 ^
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
           "CacheClusterId": "my-memcached-cluster", 
           "PreferredAvailabilityZone": "us-east-2b", 
           "ConfigurationEndpoint": {
               "Port": 11211, 
               "Address": "rlh-mem000.7ef-example.cfg.usw2.cache.amazonaws.com"
           }, 
           "CacheSecurityGroups": [], 
           "CacheClusterCreateTime": "2016-09-21T16:28:28.973Z", 9dcv5r
           "AutoMinorVersionUpgrade": true, 
           "CacheClusterStatus": "modifying", 
           "NumCacheNodes": 3, 
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
               "NumCacheNodes": 2, 
               "CacheNodeIdsToRemove": [
                   "0001"
               ]
           }, 
           "PreferredMaintenanceWindow": "sat:09:00-sat:10:00", 
           "CacheNodeType": "cache.m3.medium"
       }
   }
   ```

For more information, see the AWS CLI topics [http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-cluster.html](http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-cluster.html) and [http://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-cluster.html](http://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-cluster.html)\.

## Removing Nodes from a Cluster \(ElastiCache API\)<a name="Clusters.DeleteNode.API"></a>

To remove nodes using the ElastiCache API, call the `ModifyCacheCluster` API operation with the cache cluster ID and a list of nodes to remove, as shown:

+ `CacheClusterId` The ID of the cache cluster you want to remove nodes from\.

+ `NumCacheNodes` The `NumCacheNodes` parameter specifies the number of nodes you want in this cluster after the modification is applied\.

+ `CacheNodeIdsToRemove.member.n` The list of node IDs to remove from the cluster\.

  + `CacheNodeIdsToRemove.member.1=0004`

  + `CacheNodeIdsToRemove.member.1=0005`

+ `ApplyImmediately` Specifies whether to remove these nodes immediately or at the next maintenance window\.

+ `Region` Specifies the region of the cluster you want to remove a node from\.

The following example immediately removes nodes 0004 and 0005 from the cluster myCacheCluster\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=ModifyCacheCluster
    &CacheClusterId=myCacheCluster
    &ApplyImmediately=true
    &CacheNodeIdsToRemove.member.1=0004
    &CacheNodeIdsToRemove.member.2=0005
    &NumCacheNodes=3   
    &Region us-east-2
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