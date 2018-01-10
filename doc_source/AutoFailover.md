# Replication: Multi\-AZ with Automatic Failover \(Redis\)<a name="AutoFailover"></a>

Enabling Amazon ElastiCache's Multi\-AZ with automatic failover functionality on your Redis cluster \(in the API and AWS CLI, replication group\) improves your fault tolerance in those cases where your cluster's read/write primary cluster becomes unreachable or fails for any reason\. Multi\-AZ with automatic failover is only supported on Redis clusters that support replication\.

Automatic Failover Contents

## Automatic Failover Overview<a name="AutoFailover.Overview"></a>

An ElastiCache Redis cluster that supports replication, consists of one to 15 shards, called node groups in the API and CLI\. Each shard consists of a primary node and one to five read replica nodes\. In certain situations, if your cluster is Multi\-AZ enabled, ElastiCache automatically detects the primary node's failure, selects a read replica node, and promotes it to primary\. These situations include certain types of planned maintenance, or the unlikely event of a primary node or Availability Zone failure\. This failure detection and replica promotion ensure that you can resume writing to the new primary as soon as promotion is complete\. 

ElastiCache also propagates the Domain Name Service \(DNS\) name of the promoted replica\. It does so because then if your application is writing to the primary endpoint, no endpoint change is required in your application\. However, because you read from individual endpoints, you need to change the read endpoint of the replica promoted to primary to the new replica's endpoint\.

The promotion process generally takes just a few minutes\. This process is much faster than recreating and provisioning a new primary, which is the process if you don't enable Multi\-AZ with automatic failover\.

You can enable Multi\-AZ with Automatic Failover using the ElastiCache Management Console, the AWS CLI, or the ElastiCache API\.

## Notes on Redis Multi\-AZ with Automatic Failover<a name="AutoFailover.Notes"></a>

The following points should be noted for Redis Multi\-AZ with Automatic Failover:

+ Multi\-AZ with Automatic Failover is supported on Redis version 2\.8\.6 and later\.

+ Redis Multi\-AZ with Automatic Failover is not supported on T1 node types\.

+ Redis Multi\-AZ with Automatic Failover is supported on T2 node types only if you are running Redis version 3\.2\.4 or later with cluster mode enabled\.

+ Redis replication is asynchronous\. Therefore, when a primary cluster fails over to a replica, a small amount of data might be lost due to replication lag\.

+ When choosing the replica to promote to primary, ElastiCache chooses the replica with the least replication lag \(that is, the one that is most current\)\.

+ Promoting read replicas to primary:

  + You can only promote a read replica to primary when Multi\-AZ with Automatic Failover is disabled\. To promote a read replica node to primary, you must first disable Multi\-AZ with Automatic Failover on the cluster, do the promotion, and then re\-enable Multi\-AZ with Automatic Failover\.

  + You cannot disable Multi\-AZ with Automatic Failover on Redis \(cluster mode enabled\) clusters\. Therefore, you cannot manually promote a replica to primary on any Redis \(cluster mode enabled\) cluster\.

+ ElastiCache Multi\-AZ with Automatic Failover and append\-only file \(AOF\) are mutually exclusive\. If you enable one, you cannot enable the other\.

+ When a node's failure is caused by the rare event of an entire Availability Zone failing, the replica replacing the failed primary is created only when the Availability Zone is back up\. For example, consider a replication group with the primary in AZ\-a and replicas in AZ\-b and AZ\-c\. If the primary fails, the replica with the least replication lag is promoted to primary cluster\. Then, ElastiCache creates a new replica in AZ\-a \(where the failed primary was located\) only when AZ\-a is back up and available\.

+ A customer\-initiated reboot of a primary does not trigger automatic failover\. Other reboots and failures do trigger automatic failover\.

+ Whenever the primary is rebooted, it is cleared of data when it comes back online\. When the read replicas see the cleared primary cluster, they clear their copy of the data, which causes data loss\.

+ After a read replica has been promoted, the other replicas sync with the new primary\. After the initial sync, the replicas' content is deleted and they sync the data from the new primary, causing a brief interruption during which the replicas are not accessible\. This sync process also causes a temporary load increase on the primary while syncing with the replicas\. This behavior is native to Redis and isn’t unique to ElastiCache Multi\-AZ\. For details regarding this Redis behavior, see [http://redis\.io/topics/replication](http://redis.io/topics/replication)\.

**Important**  
For Redis version 2\.8\.22 and later, external replicas are not permitted\.
For Redis versions prior to 2\.8\.22, we recommend that you do not connect an external Redis replica to an ElastiCache Redis cluster that is Multi\-AZ with Automatic Failover enabled\. This is an unsupported configuration that can create issues that prevent ElastiCache from properly performing failover and recovery\. If you need to connect an external Redis replica to an ElastiCache cluster, make sure that Multi\-AZ with Automatic Failover is disabled before you make the connection\.

## Failure Scenarios with Multi\-AZ and Automatic Failover Responses<a name="AutoFailover.Scenarios"></a>

Prior to the introduction of Multi\-AZ with Automatic Failover, ElastiCache detected and replaced a cluster's failed nodes by recreating and reprovisioning the failed node\. By enabling Multi\-AZ with Automatic Failover, a failed primary node fails over to the replica with the least replication lag\. The selected replica is automatically promoted to primary, which is much faster than creating and reprovisioning a new primary node\. This process usually takes just a few minutes until you can write to the cluster again\.

When Multi\-AZ with Automatic Failover is enabled, ElastiCache continually monitors the state of the primary node\. If the primary node fails, one of the following actions is performed depending on the nature of the failure\.

Failure Scenarios

 

### When Only the Primary Node Fails<a name="AutoFailover.Scenarios.PrimaryOnly"></a>

If only the primary node fails, the read replica with the least replication lag is promoted to primary, and a replacement read replica is created and provisioned in the same Availability Zone as the failed primary\.

![\[Image: Automatic Failover for a failed primary node\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-AutoFailover-FailedPrimary.png)

*Automatic Failover for a failed primary node*

What ElastiCache Multi\-AZ with Automatic Failover does when only the primary node fails is the following:

1. The failed primary node is taken offline\.

1. The read replica with the least replication lag is promoted to primary\.

   Writes can resume as soon as the promotion process is complete, typically just a few minutes\. If your application is writing to the primary endpoint, there is no need to change the endpoint for writes as ElastiCache propagates the DNS name of the promoted replica\.

1. A replacement read replica is launched and provisioned\.

   The replacement read replica is launched in the Availability Zone that the failed primary node was in so that the distribution of nodes is maintained\.

1. The replicas sync with the new primary node\.

You need to make the following changes to your application after the new replica is available:

+ **Primary endpoint** – Do not make any changes to your application because the DNS name of the new primary node is propagated to the primary endpoint\.

+ **Read endpoint** – Replace the read endpoint of the failed primary with the read endpoint of the new replica\.

For information about finding the endpoints of a cluster, see the following topics:

+ [Finding a Redis \(cluster mode disabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Redis)

+ [Finding the Endpoints for Replication Groups \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI.ReplGroups)

+ [Finding Endpoints for Replication Groups \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API.ReplGroups)

 

### When the Primary Node and Some Read Replicas Fail<a name="AutoFailover.Scenarios.PrimaryAndReplicas"></a>

If the primary and at least one read replica fails, the available replica with the least replication lag is promoted to primary cluster\. New read replicas are also created and provisioned in the same Availability Zones as the failed nodes and replica that was promoted to primary\.

![\[Image: Automatic failover for a failed primary node and read replica\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-AutoFailover-FailedPrimarySomeReplicas.png)

What ElastiCache Multi\-AZ does when the primary node and some read replicas fail is the following:

1. The failed primary node and failed read replicas are taken offline\.

1. The available replica with the least replication lag is promoted to primary node\.

   Writes can resume as soon as the promotion process is complete, typically just a few minutes\. If your application is writing to the primary endpoint, there is no need to change the endpoint for writes, because ElastiCache propagates the DNS name of the promoted replica\.

1. Replacement replicas are created and provisioned\.

   The replacement replicas are created in the Availability Zones of the failed nodes so that the distribution of nodes is maintained\.

1. All clusters sync with the new primary node\.

You need to make the following changes to your application after the new nodes are available:

+ **Primary endpoint** – Do not make any changes to your application because the DNS name of the new primary node is propagated to the primary endpoint\.

+ **Read endpoint** – Replace the read endpoint of the failed primary and failed replicas with the node endpoints of the new replicas\.

For information about finding the endpoints of a replication group, see the following topics:

+ [Finding a Redis \(cluster mode disabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Redis)

+ [Finding the Endpoints for Replication Groups \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI.ReplGroups)

+ [Finding Endpoints for Replication Groups \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API.ReplGroups)

 

### When the Entire Cluster Fails<a name="AutoFailover.Scenarios.AllFail"></a>

If everything fails, all the nodes are recreated and provisioned in the same Availability Zones as the original nodes\. 

In this scenario, all the data in the cluster is lost due to the failure of every node in the cluster\. This occurrence is rare\.

![\[Image: Automatic Failover for a failed cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-AutoFailover-FailedCluster.png)

What ElastiCache Multi\-AZ does when the entire cluster fails is the following:

1. The failed primary node and read replicas are taken offline\.

1. A replacement primary node is created and provisioned\.

1. Replacement replicas are created and provisioned\.

   The replacements are created in the Availability Zones of the failed nodes so that the distribution of nodes is maintained\.

   Because the entire cluster failed, data is lost and all the new nodes start cold\.

Because each of the replacement nodes will have the same endpoint as the node it is replacing, you don't need to make any endpoint changes in your application\.

For information about finding the endpoints of a replication group, see the following topics:

+ [Finding a Redis \(cluster mode disabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Redis)

+ [Finding the Endpoints for Replication Groups \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI.ReplGroups)

+ [Finding Endpoints for Replication Groups \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API.ReplGroups)

We recommend that you create the primary node and read replicas in different Availability Zones to raise your fault tolerance level\.

## Enabling Multi\-AZ with Automatic Failover<a name="AutoFailover.Enable"></a>

You can enable Multi\-AZ with Automatic Failover when you create or modify a cluster \(API or CLI, replication group\) using the ElastiCache console, AWS CLI, or the ElastiCache API\.

You can enable Multi\-AZ with Automatic Failover only on Redis \(cluster mode disabled\) clusters that have at least one available read replica\. Multi\-AZ with Automatic Failover is required on all Redis \(cluster mode enabled\) clusters, whether or not they have read replicas\. Clusters without read replicas do not provide high availability or fault tolerence\. For information about creating a cluster with replication, see [Creating a Redis Cluster with Replicas](Replication.CreatingRepGroup.md)\. For information about adding a read replica to a cluster with replication, see [Adding a Read Replica to a Redis Cluster](Replication.AddReadReplica.md)\.


+ [Enabling Multi\-AZ with Automatic Failover \(Console\)](#AutoFailover.Enable.Console)
+ [Enabling Multi\-AZ with Automatic Failover \(AWS CLI\)](#AutoFailover.Enable.CLI)
+ [Enabling Multi\-AZ with Automatic Failover \(ElastiCache API\)](#AutoFailover.Enable.API)

### Enabling Multi\-AZ with Automatic Failover \(Console\)<a name="AutoFailover.Enable.Console"></a>

You can enable Multi\-AZ with Automatic Failover using the ElastiCache console when you create a new Redis cluster or by modifying an existing Redis cluster with replication\.

Multi\-AZ with Automatic Failover is enabled by default and cannot be disabled on Redis \(cluster mode enabled\) clusters\.

#### Enabling Multi\-AZ with Automatic Failover When Creating a Cluster Using the ElastiCache Console<a name="AutoFailover.Enable.Console.NewCacheCluster"></a>

For more information on this process, see [Creating a Redis \(cluster mode disabled\) Cluster \(Console\)](Clusters.Create.CON.Redis.md)\. Be sure to have one or more replicas and enable Multi\-AZ with Automatic Failover\.

#### Enabling Multi\-AZ with Automatic Failover on an Existing Cluster \(Console\)<a name="AutoFailover.Enable.Console.ReplGrp"></a>

For more information on this process, see [Modifying a Cluster \(Console\)](Clusters.Modify.md#Clusters.Modify.CON)\.

### Enabling Multi\-AZ with Automatic Failover \(AWS CLI\)<a name="AutoFailover.Enable.CLI"></a>

The following code example uses the AWS CLI to enable Multi\-AZ with Automatic Failover for the replication group `redis12`\.

**Important**  
The replication group `redis12` must already exist and have at least one available read replica\.

For Linux, macOS, or Unix:

```
aws elasticache modify-replication-group \
    --replication-group-id redis12 \
    --automatic-failover-enabled \
    --apply-immediately
```

For Windows:

```
aws elasticache modify-replication-group ^
    --replication-group-id redis12 ^
    --automatic-failover-enabled ^
    --apply-immediately
```

The JSON output from this command should look something like this\.

```
{
    "ReplicationGroup": {
        "Status": "modifying", 
        "Description": "One shard, two nodes", 
        "NodeGroups": [
            {
                "Status": "modifying", 
                "NodeGroupMembers": [
                    {
                        "CurrentRole": "primary", 
                        "PreferredAvailabilityZone": "us-west-2b", 
                        "CacheNodeId": "0001", 
                        "ReadEndpoint": {
                            "Port": 6379, 
                            "Address": "redis12-001.v5r9dc.0001.usw2.cache.amazonaws.com"
                        }, 
                        "CacheClusterId": "redis12-001"
                    }, 
                    {
                        "CurrentRole": "replica", 
                        "PreferredAvailabilityZone": "us-west-2a", 
                        "CacheNodeId": "0001", 
                        "ReadEndpoint": {
                            "Port": 6379, 
                            "Address": "redis12-002.v5r9dc.0001.usw2.cache.amazonaws.com"
                        }, 
                        "CacheClusterId": "redis12-002"
                    }
                ], 
                "NodeGroupId": "0001", 
                "PrimaryEndpoint": {
                    "Port": 6379, 
                    "Address": "redis12.v5r9dc.ng.0001.usw2.cache.amazonaws.com"
                }
            }
        ], 
        "ReplicationGroupId": "redis12", 
        "SnapshotRetentionLimit": 1, 
        "AutomaticFailover": "enabling", 
        "SnapshotWindow": "07:00-08:00", 
        "SnapshottingClusterId": "redis12-002", 
        "MemberClusters": [
            "redis12-001", 
            "redis12-002"
        ], 
        "PendingModifiedValues": {}
    }
}
```

For more information, see these topics in the *AWS CLI Command Reference*:

+ [create\-cache\-cluster](http://docs.aws.amazon.com/cli/latest/reference/elasticache/create-cache-cluster.html)

+ [create\-replication\-group](http://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

+ [modify\-replication\-group](http://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group.html) in the *AWS CLI Command Reference\.*

### Enabling Multi\-AZ with Automatic Failover \(ElastiCache API\)<a name="AutoFailover.Enable.API"></a>

The following code example uses the ElastiCache API to enable Multi\-AZ with Automatic Failover for the replication group `redis12`\.

**Note**  
To use this example, the replication group `redis12` must already exist and have at least one available read replica\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=ModifyReplicationGroup
    &ApplyImmediately=true
    &AutoFailover=true
    &ReplicationGroupId=redis12
    &Version=2015-02-02
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20140401T192317Z
    &X-Amz-Credential=<credential>
```

For more information, see these topics in the *ElastiCache API Reference*:

+ [CreateCacheCluster](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheCluster.html)

+ [CreateReplicationGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)

+ [ModifyReplicationGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroup.html)

## Testing Multi\-AZ with Automatic Failover<a name="auto-failover-test"></a>

After you enable Multi\-AZ with Automatic Failover, you can test it using the ElastiCache console, the AWS CLI, and the ElastiCache API\.

When testing, note the following:

+ You can use this operation to test automatic failover on up to five shards \(called node groups in the ElastiCache API and AWS CLI\) in any rolling 24\-hour period\.

+ If you call this operation on shards in different clusters \(called replication groups in the API and CLI\), you can make the calls concurrently\.

+ If you call this operation multiple times on different shards in the same Redis \(cluster mode enabled\) replication group, the first node replacement must complete before a subsequent call can be made\.

+ To determine whether the node replacement is complete you can check Events using the Amazon ElastiCache console, the AWS CLI, or the ElastiCache API\. Look for the following automatic failover related events, listed here in order of occurrance:

  1. Replication group message: `Test Failover API called for node group <node-group-id>`

  1. Cache cluster message: `Failover from master node <primary-node-id> to replica node <node-id> completed`

  1. Replication group message: `Failover from master node <primary-node-id> to replica node <node-id> completed`

  1. Cache cluster message: `Recovering cache nodes <node-id>`

  1. Cache cluster message: `Finished recovery for cache nodes <node-id>`

   

  For more information, see the following:

  + [Viewing ElastiCache Events](ECEvents.Viewing.md) in the *ElastiCache User Guide*

  + [DescribeEvents](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEvents.html) in the *ElastiCache API Reference*

  + [describe\-events](http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-events.html) in the *AWS CLI Command Reference\.*


+ [Testing Automatic Failover Using the AWS Management Console](#auto-failover-test-con)
+ [Testing Automatic Failover Using the AWS CLI](#auto-failover-test-cli)
+ [Testing Automatic Failover Using the ElastiCache API](#auto-failover-test-api)

 

### Testing Automatic Failover Using the AWS Management Console<a name="auto-failover-test-con"></a>

The following procedure walks you through testing automatic failover\.

**To test automatic failover**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Redis**\.

1. From the list of Redis clusters, choose the box to the left of the cluster you want to test\. This cluster must have at least one read replica node\.

1. In the **Details** area, confirm that this cluster is Multi\-AZ enabled\. If the cluster is not Multi\-AZ enabled, either choose a different cluster or modify this cluster to enable Multi\-AZ\. For more information, see [Modifying a Cluster \(Console\)](Clusters.Modify.md#Clusters.Modify.CON)\.  
![\[Image: Details area of a Multi-AZ enabled Redis cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-AutoFailover-MultiAZ-Enabled.png)

1. For Redis \(cluster mode disabled\), choose the cluster's name\.

   For Redis \(cluster mode enabled\), do the following:

   1. Choose the cluster's name\. 

   1. On the **Shards** page, for the shard \(called node group in the API and CLI\) on which you want to test failover, choose the shard's name\. 

1. On the Nodes page, choose **Failover Primary**\.

1. Choose **Continue** to fail over the primary, or **Cancel** to cancel the operation and not fail over the primary node\.

   During the failover process, the console continues to show the node's status as *available*\. To track the progress of your failover test, choose **Events** from the console navigation pane\. On the **Events** tab, watch for events that indicate your failover has started \(`Test Failover API called`\) and completed \(`Recovery completed`\)\.

 

### Testing Automatic Failover Using the AWS CLI<a name="auto-failover-test-cli"></a>

You can test automatic failover on any Multi\-AZ with Automatic Failover enabled cluster using the AWS CLI operation `test-failover`\.

**Parameters**

+ `--replication-group-id` – Required\. The replication group \(on the console, cluster\) that is to be tested\.

+ `--node-group-id` – Required\. The name of the node group you want to test automatic failover on\. You can test a maximum of five node groups in a rolling 24\-hour period\.

The following example uses the AWS CLI to test automatic failover on the node group `redis00-0003` in the Redis \(cluster mode enabled\) cluster `redis00`\.

**Example Test Automatic Failover**  
For Linux, macOS, or Unix:  

```
aws elasticache test-failover \
   --replication-group-id redis00 \
   --node-group-id redis00-0003
```
For Windows:  

```
aws elasticache test-failover ^
   --replication-group-id redis00 ^
   --node-group-id redis00-0003
```

Output from the preceeding command looks something like this\.

```
{
    "ReplicationGroup": {
        "Status": "available", 
        "Description": "1 shard, 3 nodes (1 + 2 replicas)", 
        "NodeGroups": [
            {
                "Status": "available", 
                "NodeGroupMembers": [
                    {
                        "CurrentRole": "primary", 
                        "PreferredAvailabilityZone": "us-west-2c", 
                        "CacheNodeId": "0001", 
                        "ReadEndpoint": {
                            "Port": 6379, 
                            "Address": "redis1x3-001.7ekv3t.0001.usw2.cache.amazonaws.com"
                        }, 
                        "CacheClusterId": "redis1x3-001"
                    }, 
                    {
                        "CurrentRole": "replica", 
                        "PreferredAvailabilityZone": "us-west-2a", 
                        "CacheNodeId": "0001", 
                        "ReadEndpoint": {
                            "Port": 6379, 
                            "Address": "redis1x3-002.7ekv3t.0001.usw2.cache.amazonaws.com"
                        }, 
                        "CacheClusterId": "redis1x3-002"
                    }, 
                    {
                        "CurrentRole": "replica", 
                        "PreferredAvailabilityZone": "us-west-2b", 
                        "CacheNodeId": "0001", 
                        "ReadEndpoint": {
                            "Port": 6379, 
                            "Address": "redis1x3-003.7ekv3t.0001.usw2.cache.amazonaws.com"
                        }, 
                        "CacheClusterId": "redis1x3-003"
                    }
                ], 
                "NodeGroupId": "0001", 
                "PrimaryEndpoint": {
                    "Port": 6379, 
                    "Address": "redis1x3.7ekv3t.ng.0001.usw2.cache.amazonaws.com"
                }
            }
        ], 
        "ClusterEnabled": false, 
        "ReplicationGroupId": "redis1x3", 
        "SnapshotRetentionLimit": 1, 
        "AutomaticFailover": "enabled", 
        "SnapshotWindow": "11:30-12:30", 
        "SnapshottingClusterId": "redis1x3-002", 
        "MemberClusters": [
            "redis1x3-001", 
            "redis1x3-002", 
            "redis1x3-003"
        ], 
        "CacheNodeType": "cache.m3.medium", 
        "PendingModifiedValues": {}
    }
}
```

To track the progress of your failover, use the AWS CLI `describe-events` operation\.

For more information, see the following:

+ [test\-failover](http://docs.aws.amazon.com/cli/latest/reference/elasticache/test-failover.html) in the *AWS CLI Command Reference\.*

+ [describe\-events](http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-events.html) in the *AWS CLI Command Reference\.*

 

### Testing Automatic Failover Using the ElastiCache API<a name="auto-failover-test-api"></a>

You can test automatic failover on any cluster enabled with Multi\-AZ with Automatic Failover using the ElastiCache API operation `TestFailover`\.

**Parameters**

+ `ReplicationGroupId` – Required\. The replication group \(on the console, cluster\) that is to be tested\.

+ `NodeGroupId` – Required\. The name of the node group you want to test automatic failover on\. You can test a maximum of five node groups in a rolling 24\-hour period\.

The following example tests automatic failover on the node group `redis00-0003` in the replication group \(on the console, cluster\) `redis00`\.

**Example Testing Automatic Failover**  

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=TestFailover
    &NodeGroupId=redis00-0003
    &ReplicationGroupId=redis00
    &Version=2015-02-02
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20140401T192317Z
    &X-Amz-Credential=<credential>
```

To track the progress of your failover, use the ElastiCache API `DescribeEvents` operation\.

For more information, see the following:

+ [TestFailover](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_TestFailover.html) in the *ElastiCache API Reference * 

+ [DescribeEvents](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEvents.html) in the *ElastiCache API Reference * 

 