# Removing nodes from a cluster<a name="Clusters.DeleteNode"></a>

You can delete a node from a cluster using the AWS Management Console, the AWS CLI, or the ElastiCache API\.

## Using the AWS Management Console<a name="Clusters.DeleteNode.CON"></a>

**To remove nodes from a cluster \(console\)**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the list in the upper\-right corner, choose the AWS Region of the cluster that you want to remove nodes from\.

1. In the navigation pane, choose the engine running on the cluster that you want to remove a node\.

   A list of clusters running the chosen engine appears\.

1. From the list of clusters, choose the cluster name from which you want to remove a node\.

   A list of the cluster's nodes appears\.

1. Choose the box to the left of the node ID for the node that you want to remove\. Using the ElastiCache console, you can only delete one node at a time, so choosing multiple nodes means that you can't use the **Delete node** button\.

   The *Delete Node* page appears\.

1. To delete the node, complete the **Delete Node** page and choose **Delete Node**\. To keep the node, choose **Cancel**\.
**Important**  
If deleting the node results in the cluster no longer being Multi\-AZ compliant, make sure to first clear the **Multi\-AZ** check box and then delete the node\. If you clear the **Multi\-AZ** check box, you can choose to enable **Auto failover**\.


**Impact of New Add and Remove Requests on Pending Requests**  

| Scenarios | Pending Operation | New Request | Results | 
| --- | --- | --- | --- | 
|  Scenario 1 |  Delete | Delete |  The new delete request, pending or immediate, replaces the pending delete request\. For example, if nodes 0001, 0003, and 0007 are pending deletion and a new request to delete nodes 0002 and 0004 is issued, only nodes 0002 and 0004 will be deleted\. Nodes 0001, 0003, and 0007 will not be deleted\. | 
|  Scenario 2 |  Delete |  Create |  The new create request, pending or immediate, replaces the pending delete request\. For example, if nodes 0001, 0003, and 0007 are pending deletion and a new request to create a node is issued, a new node will be created and nodes 0001, 0003, and 0007 will not be deleted\. | 
|  Scenario 3 |  Create |  Delete |  The new delete request, pending or immediate, replaces the pending create request\. For example, if there is a pending request to create two nodes and a new request is issued to delete node 0003, no new nodes will be created and node 0003 will be deleted\. | 
|  Scenario 4 |  Create |  Create |  The new create request is added to the pending create request\. For example, if there is a pending request to create two nodes and a new request is issued to create three nodes, the new requests is added to the pending request and five nodes will be created\. If the new create request is set to **Apply Immediately \- Yes**, all create requests are performed immediately\. If the new create request is set to **Apply Immediately \- No**, all create requests are pending\. | 

To determine what operations are pending, choose the **Description** tab and check to see how many pending creations or deletions are shown\. You cannot have both pending creations and pending deletions\. 

![\[Image: Cluster description tab\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ModifyCacheCluster-DescriptionTab-PendActions.png)

## Using the AWS CLI<a name="Clusters.DeleteNode.CLI"></a>

1. Identify the IDs of the nodes that you want to remove\. For more information, see [Viewing a cluster's details](Clusters.ViewDetails.md)\.

1. Use the `decrease-replica-count` CLI operation with a list of the nodes to remove, as in the following example\.

   To remove nodes from a cluster using the command\-line interface, use the command `decrease-replica-count` with the following parameters:
   + `--replication-group-id` The ID of the replication group that you want to remove nodes from\.
   + `--new-replica-count` The `--new-replica-count` parameter specifies the number of nodes that you want in this cluster after the modification is applied\.
   + `--replicas-to-remove` A list of node IDs that you want removed from this cluster\.
   + `--apply-immediately` or `--no-apply-immediately` Specifies whether to remove these nodes immediately or at the next maintenance window\.
   + `--region` Specifies the AWS Region of the cluster that you want to remove nodes from\.
**Note**  
You can pass only one of `--replicas-to-remove` or `--new-replica-count` parameters when calling this operation\.

   For Linux, macOS, or Unix:

   ```
   aws elasticache decrease-replica-count \
       --replication-group-id my-replication-group \
       --new-replica-count 2 \   
       --region us-east-2 \
       --apply-immediately
   ```

   For Windows:

   ```
   aws elasticache decrease-replica-count ^
       --replication-group-id my-replication-group ^
       --new-replica-count 3 ^   
       --region us-east-2 ^
       --apply-immediately
   ```

   This operation produces output similar to the following \(JSON format\):

   ```
   {
       "ReplicationGroup": {
           "ReplicationGroupId": "node-test",
           "Description": "node-test"
          },
           "Status": "modifying",
           "PendingModifiedValues": {},
           "MemberClusters": [
               "node-test-001",
               "node-test-002",
               "node-test-003",
               "node-test-004",
               "node-test-005",
               "node-test-006"
           ],
           "NodeGroups": [
               {
                   "NodeGroupId": "0001",
                   "Status": "modifying",
                   "PrimaryEndpoint": {
                       "Address": "node-test.zzzzzz.ng.0001.usw2.cache.amazonaws.com",
                       "Port": 6379
                   },
                   "ReaderEndpoint": {
                       "Address": "node-test-ro.zzzzzz.ng.0001.usw2.cache.amazonaws.com",
                       "Port": 6379
                   },
                   "NodeGroupMembers": [
                       {
                           "CacheClusterId": "node-test-001",
                           "CacheNodeId": "0001",
                           "ReadEndpoint": {
                               "Address": "node-test-001.zzzzzz.0001.usw2.cache.amazonaws.com",
                               "Port": 6379
                           },
                           "PreferredAvailabilityZone": "us-west-2a",
                           "CurrentRole": "primary"
                       },
                       {
                           "CacheClusterId": "node-test-002",
                           "CacheNodeId": "0001",
                           "ReadEndpoint": {
                               "Address": "node-test-002.zzzzzz.0001.usw2.cache.amazonaws.com",
                               "Port": 6379
                           },
                           "PreferredAvailabilityZone": "us-west-2c",
                           "CurrentRole": "replica"
                       },
                       {
                           "CacheClusterId": "node-test-003",
                           "CacheNodeId": "0001",
                           "ReadEndpoint": {
                               "Address": "node-test-003.zzzzzz.0001.usw2.cache.amazonaws.com",
                               "Port": 6379
                           },
                           "PreferredAvailabilityZone": "us-west-2b",
                           "CurrentRole": "replica"
                       },
                       {
                           "CacheClusterId": "node-test-004",
                           "CacheNodeId": "0001",
                           "ReadEndpoint": {
                               "Address": "node-test-004.zzzzzz.0001.usw2.cache.amazonaws.com",
                               "Port": 6379
                           },
                           "PreferredAvailabilityZone": "us-west-2c",
                           "CurrentRole": "replica"
                       },
                       {
                           "CacheClusterId": "node-test-005",
                           "CacheNodeId": "0001",
                           "ReadEndpoint": {
                               "Address": "node-test-005.zzzzzz.0001.usw2.cache.amazonaws.com",
                               "Port": 6379
                           },
                           "PreferredAvailabilityZone": "us-west-2b",
                           "CurrentRole": "replica"
                       },
                       {
                           "CacheClusterId": "node-test-006",
                           "CacheNodeId": "0001",
                           "ReadEndpoint": {
                               "Address": "node-test-006.zzzzzz.0001.usw2.cache.amazonaws.com",
                               "Port": 6379
                           },
                           "PreferredAvailabilityZone": "us-west-2b",
                           "CurrentRole": "replica"
                       }
                   ]
               }
           ],
           "SnapshottingClusterId": "node-test-002",
           "AutomaticFailover": "enabled",
           "MultiAZ": "enabled",
           "SnapshotRetentionLimit": 1,
           "SnapshotWindow": "07:30-08:30",
           "ClusterEnabled": false,
           "CacheNodeType": "cache.r5.large",
           "TransitEncryptionEnabled": false,
           "AtRestEncryptionEnabled": false,
           "ARN": "arn:aws:elasticache:us-west-2:123456789012:replicationgroup:node-test"
       }
   }
   ```

Alternatively, you could call `decrease-replica-count` and instead of passing in the `--new-replica-count` parameter, you could pass the `--replicas-to-remove` parameter, as shown following:

For Linux, macOS, or Unix:

```
aws elasticache decrease-replica-count \
    --replication-group-id my-replication-group \
    --replicas-to-remove node-test-003 \   
    --region us-east-2 \
    --apply-immediately
```

For Windows:

```
aws elasticache decrease-replica-count ^
    --replication-group-id my-replication-group ^
    --replicas-to-remove node-test-003 ^   
    --region us-east-2 ^
    --apply-immediately
```

For more information, see the AWS CLI topics [https://docs.aws.amazon.com/cli/latest/reference/elasticache/decrease-replica-count.html](https://docs.aws.amazon.com/cli/latest/reference/elasticache/decrease-replica-count.html)\.

## Using the ElastiCache API<a name="Clusters.DeleteNode.API"></a>

To remove nodes using the ElastiCache API, call the `DecreaseReplicaCount` API operation with the replication group Id and a list of nodes to remove, as shown:
+ `ReplicationGroupId` The ID of the replication group that you want to remove nodes from\.
+ `ReplicasToRemove` The `ReplicasToRemove` parameter specifies the number of nodes that you want in this cluster after the modification is applied\.
+ `ApplyImmediately` Specifies whether to remove these nodes immediately or at the next maintenance window\.
+ `Region` Specifies the AWS Region of the cluster that you want to remove a node from\.

The following example immediately removes nodes 0004 and 0005 from the cluster my\-cluster\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=DecreaseReplicaCount
    &CacheClusterId=my-cluster
    &ApplyImmediately=true
    &ReplicasToRemove=node-test-003    
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

For more information, see ElastiCache API topic [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DecreaseReplicaCount.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DecreaseReplicaCount.html)\.