# Minimizing Downtime in ElastiCache for Redis with Multi\-AZ<a name="AutoFailover"></a>

In certain cases, ElastiCache for Redis detects and replaces a primary node\. These cases include certain types of planned maintenance and the unlikely event of a primary node or Availability Zone failure\. 

This replacement results in some downtime for the cluster\. If you have Multi\-AZ enabled on the cluster, the downtime is minimized\. In this case, the role of primary node fails over to one of the read replicas\. There's no need to create and provision a new primary node\. This failover and replica promotion ensure that you can resume writing to the new primary as soon as promotion is complete\. 

ElastiCache also propagates the Domain Name Service \(DNS\) name of the promoted replica\. It does so because then if your application is writing to the primary endpoint, no endpoint change is required in your application\. If you are reading from individual endpoints, make sure that you change the read endpoint of the replica promoted to primary to the new replica's endpoint\.

In case of planned node replacements initiated due to maintenance updates or self\-service updates, be aware of the following:
+ For ElastiCache for Redis Cluster, the planned node replacements complete while the cluster serves incoming write requests\. 
+ For Redis Cluster mode disabled clusters with Multi\-AZ enabled that run on the 5\.0\.5 or later engine, the planned node replacements complete while the cluster serves incoming write requests\. 
+ For Redis Cluster mode disabled clusters with Multi\-AZ enabled that run on the 5\.0\.4 or earlier engine, you might notice a brief write interruption associated with DNS updates\. This interruption might take up to a few seconds\. This process is much faster than recreating and provisioning a new primary, which is the process if you don't enable Multi\-AZ\. 

You can enable Multi\-AZ using the ElastiCache Management Console, the AWS CLI, or the ElastiCache API\.

Enabling ElastiCache Multi\-AZ on your Redis cluster \(in the API and CLI, replication group\) improves your fault tolerance\. This is true particularly in cases where your cluster's read/write primary cluster becomes unreachable or fails for any reason\. Multi\-AZ is only supported on Redis clusters that have more than one node in each shard\.

**Topics**
+ [Enabling Multi\-AZ](#AutoFailover.Enable)
+ [Failure Scenarios with Multi\-AZ Responses](#AutoFailover.Scenarios)
+ [Testing Automatic Failover](#auto-failover-test)
+ [Limitations on Redis Multi\-AZ](#AutoFailover.Notes)

## Enabling Multi\-AZ<a name="AutoFailover.Enable"></a>

You can enable Multi\-AZ when you create or modify a cluster \(API or CLI, replication group\) using the ElastiCache console, AWS CLI, or the ElastiCache API\.

You can enable Multi\-AZ only on Redis \(cluster mode disabled\) clusters that have at least one available read replica\. Clusters without read replicas do not provide high availability or fault tolerance\. For information about creating a cluster with replication, see [Creating a Redis Replication Group](Replication.CreatingRepGroup.md)\. For information about adding a read replica to a cluster with replication, see [Adding a Read Replica, for Redis \(Cluster Mode Disabled\) Replication Groups](Replication.AddReadReplica.md)\.

**Topics**
+ [Enabling Multi\-AZ \(Console\)](#AutoFailover.Enable.Console)
+ [Enabling Multi\-AZ \(AWS CLI\)](#AutoFailover.Enable.CLI)
+ [Enabling Multi\-AZ \(ElastiCache API\)](#AutoFailover.Enable.API)

### Enabling Multi\-AZ \(Console\)<a name="AutoFailover.Enable.Console"></a>

You can enable Multi\-AZ using the ElastiCache console when you create a new Redis cluster or by modifying an existing Redis cluster with replication\.

Multi\-AZ is enabled by default on Redis \(cluster mode enabled\) clusters\.

**Important**  
ElastiCache will automatically enable Multi\-AZ only if the cluster contains at least one replica in a different Availability Zone from the primary in all shards\.

#### Enabling Multi\-AZ When Creating a Cluster Using the ElastiCache Console<a name="AutoFailover.Enable.Console.NewCacheCluster"></a>

For more information on this process, see [Creating a Redis \(cluster mode disabled\) Cluster \(Console\)](Clusters.Create.CON.Redis.md)\. Be sure to have one or more replicas and enable Multi\-AZ\.

#### Enabling Multi\-AZ on an Existing Cluster \(Console\)<a name="AutoFailover.Enable.Console.ReplGrp"></a>

For more information on this process, see Modifying a Cluster [Using the AWS Management Console](Clusters.Modify.md#Clusters.Modify.CON)\.

### Enabling Multi\-AZ \(AWS CLI\)<a name="AutoFailover.Enable.CLI"></a>

The following code example uses the AWS CLI to enable Multi\-AZ for the replication group `redis12`\.

**Important**  
The replication group `redis12` must already exist and have at least one available read replica\.

For Linux, macOS, or Unix:

```
aws elasticache modify-replication-group \
    --replication-group-id redis12 \
    --automatic-failover-enabled \
    --multi-az-enabled \
    --apply-immediately
```

For Windows:

```
aws elasticache modify-replication-group ^
    --replication-group-id redis12 ^
    --automatic-failover-enabled ^
    --multi-az-enabled ^
    --apply-immediately
```

The JSON output from this command should look something like the following\.

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
        "MultiAZ": "enabled", 
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
+ [create\-cache\-cluster](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-cache-cluster.html)
+ [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)
+ [modify\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group.html) in the *AWS CLI Command Reference\.*

### Enabling Multi\-AZ \(ElastiCache API\)<a name="AutoFailover.Enable.API"></a>

The following code example uses the ElastiCache API to enable Multi\-AZ for the replication group `redis12`\.

**Note**  
To use this example, the replication group `redis12` must already exist and have at least one available read replica\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=ModifyReplicationGroup
    &ApplyImmediately=true
    &AutoFailover=true
    &MultiAZEnabled=true
    &ReplicationGroupId=redis12
    &Version=2015-02-02
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20140401T192317Z
    &X-Amz-Credential=<credential>
```

For more information, see these topics in the *ElastiCache API Reference*:
+ [CreateCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheCluster.html)
+ [CreateReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)
+ [ModifyReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroup.html)

## Failure Scenarios with Multi\-AZ Responses<a name="AutoFailover.Scenarios"></a>

Before the introduction of Multi\-AZ, ElastiCache detected and replaced a cluster's failed nodes by recreating and reprovisioning the failed node\. If you enable Multi\-AZ, a failed primary node fails over to the replica with the least replication lag\. The selected replica is automatically promoted to primary, which is much faster than creating and reprovisioning a new primary node\. This process usually takes just a few seconds until you can write to the cluster again\.

When Multi\-AZ is enabled, ElastiCache continually monitors the state of the primary node\. If the primary node fails, one of the following actions is performed depending on the type of failure\.

**Topics**
+ [Failure Scenarios When Only the Primary Node Fails](#AutoFailover.Scenarios.PrimaryOnly)
+ [Failure Scenarios When the Primary Node and Some Read Replicas Fail](#AutoFailover.Scenarios.PrimaryAndReplicas)
+ [Failure Scenarios When the Entire Cluster Fails](#AutoFailover.Scenarios.AllFail)

### Failure Scenarios When Only the Primary Node Fails<a name="AutoFailover.Scenarios.PrimaryOnly"></a>

If only the primary node fails, the read replica with the least replication lag is promoted to primary\. A replacement read replica is then created and provisioned in the same Availability Zone as the failed primary\.

When only the primary node fails, ElastiCache Multi\-AZ does the following:

1. The failed primary node is taken offline\.

1. The read replica with the least replication lag is promoted to primary\.

   Writes can resume as soon as the promotion process is complete, typically just a few seconds\. If your application is writing to the primary endpoint, you don't need to change the endpoint for writes or reads\. ElastiCache propagates the DNS name of the promoted replica\.

1. A replacement read replica is launched and provisioned\.

   The replacement read replica is launched in the Availability Zone that the failed primary node was in so that the distribution of nodes is maintained\.

1. The replicas sync with the new primary node\.

After the new replica is available, be aware of these effects:
+ **Primary endpoint** – You don't need to make any changes to your application, because the DNS name of the new primary node is propagated to the primary endpoint\.
+ **Read endpoint** – The reader endpoint is automatically updated to point to the new replica nodes\.

For information about finding the endpoints of a cluster, see the following topics:
+ [Finding a Redis \(Cluster Mode Disabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Redis)
+ [Finding the Endpoints for Replication Groups \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI.ReplGroups)
+ [Finding Endpoints for Replication Groups \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API.ReplGroups)

 

### Failure Scenarios When the Primary Node and Some Read Replicas Fail<a name="AutoFailover.Scenarios.PrimaryAndReplicas"></a>

If the primary and at least one read replica fails, the available replica with the least replication lag is promoted to primary cluster\. New read replicas are also created and provisioned in the same Availability Zones as the failed nodes and replica that was promoted to primary\.

When the primary node and some read replicas fail, ElastiCache Multi\-AZ does the following:

1. The failed primary node and failed read replicas are taken offline\.

1. The available replica with the least replication lag is promoted to primary node\.

   Writes can resume as soon as the promotion process is complete, typically just a few seconds\. If your application is writing to the primary endpoint, there is no need to change the endpoint for writes\. ElastiCache propagates the DNS name of the promoted replica\.

1. Replacement replicas are created and provisioned\.

   The replacement replicas are created in the Availability Zones of the failed nodes so that the distribution of nodes is maintained\.

1. All clusters sync with the new primary node\.

Make the following changes to your application after the new nodes are available:
+ **Primary endpoint** – Don't make any changes to your application\. The DNS name of the new primary node is propagated to the primary endpoint\.
+ **Read endpoint** – The read endpoint is automatically updated to point to the new replica nodes\.

For information about finding the endpoints of a replication group, see the following topics:
+ [Finding a Redis \(Cluster Mode Disabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Redis)
+ [Finding the Endpoints for Replication Groups \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI.ReplGroups)
+ [Finding Endpoints for Replication Groups \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API.ReplGroups)

 

### Failure Scenarios When the Entire Cluster Fails<a name="AutoFailover.Scenarios.AllFail"></a>

If everything fails, all the nodes are recreated and provisioned in the same Availability Zones as the original nodes\. 

In this scenario, all the data in the cluster is lost due to the failure of every node in the cluster\. This occurrence is rare\.

When the entire cluster fails, ElastiCache Multi\-AZ does the following:

1. The failed primary node and read replicas are taken offline\.

1. A replacement primary node is created and provisioned\.

1. Replacement replicas are created and provisioned\.

   The replacements are created in the Availability Zones of the failed nodes so that the distribution of nodes is maintained\.

   Because the entire cluster failed, data is lost and all the new nodes start cold\.

Because each of the replacement nodes has the same endpoint as the node it's replacing, you don't need to make any endpoint changes in your application\.

For information about finding the endpoints of a replication group, see the following topics:
+ [Finding a Redis \(Cluster Mode Disabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Redis)
+ [Finding the Endpoints for Replication Groups \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI.ReplGroups)
+ [Finding Endpoints for Replication Groups \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API.ReplGroups)

We recommend that you create the primary node and read replicas in different Availability Zones to raise your fault tolerance level\.

## Testing Automatic Failover<a name="auto-failover-test"></a>

After you enable automatic failover, you can test it using the ElastiCache console, the AWS CLI, and the ElastiCache API\.

When testing, note the following:
+ You can use this operation to test automatic failover on up to five shards \(called node groups in the ElastiCache API and AWS CLI\) in any rolling 24\-hour period\.
+ If you call this operation on shards in different clusters \(called replication groups in the API and CLI\), you can make the calls concurrently\.
+ In some cases, you might call this operation multiple times on different shards in the same Redis \(cluster mode enabled\) replication group\. In such cases, the first node replacement must complete before a subsequent call can be made\.
+ To determine whether the node replacement is complete, check events using the Amazon ElastiCache console, the AWS CLI, or the ElastiCache API\. Look for the following events related to automatic failover, listed here in order of likely occurrence:

  1. Replication group message: `Test Failover API called for node group <node-group-id>`

  1. Cache cluster message: `Failover from master node <primary-node-id> to replica node <node-id> completed`

  1. Replication group message: `Failover from master node <primary-node-id> to replica node <node-id> completed`

  1. Cache cluster message: `Recovering cache nodes <node-id>`

  1. Cache cluster message: `Finished recovery for cache nodes <node-id>`

  For more information, see the following:
  + [Viewing ElastiCache Events](ECEvents.Viewing.md) in the *ElastiCache User Guide*
  + [DescribeEvents](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEvents.html) in the *ElastiCache API Reference*
  + [describe\-events](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-events.html) in the *AWS CLI Command Reference\.*

**Topics**
+ [Testing Automatic Failover Using the AWS Management Console](#auto-failover-test-con)
+ [Testing Automatic Failover Using the AWS CLI](#auto-failover-test-cli)
+ [Testing Automatic Failover Using the ElastiCache API](#auto-failover-test-api)

### Testing Automatic Failover Using the AWS Management Console<a name="auto-failover-test-con"></a>

Use the following procedure to test automatic failover with the console\.

**To test automatic failover**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Redis**\.

1. From the list of Redis clusters, choose the box to the left of the cluster you want to test\. This cluster must have at least one read replica node\.

1. In the **Details** area, confirm that this cluster is Multi\-AZ enabled\. If the cluster isn't Multi\-AZ enabled, either choose a different cluster or modify this cluster to enable Multi\-AZ\. For more information, see [Using the AWS Management Console](Clusters.Modify.md#Clusters.Modify.CON)\.  
![\[Image: Details area of a Multi-AZ enabled Redis cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-AutoFailover-MultiAZ-Enabled.png)

1. For Redis \(cluster mode disabled\), choose the cluster's name\.

   For Redis \(cluster mode enabled\), do the following:

   1. Choose the cluster's name\. 

   1. On the **Shards** page, for the shard \(called node group in the API and CLI\) on which you want to test failover, choose the shard's name\. 

1. On the Nodes page, choose **Failover Primary**\.

1. Choose **Continue** to fail over the primary, or **Cancel** to cancel the operation and not fail over the primary node\.

   During the failover process, the console continues to show the node's status as *available*\. To track the progress of your failover test, choose **Events** from the console navigation pane\. On the **Events** tab, watch for events that indicate your failover has started \(`Test Failover API called`\) and completed \(`Recovery completed`\)\.

 

### Testing Automatic Failover Using the AWS CLI<a name="auto-failover-test-cli"></a>

You can test automatic failover on any Multi\-AZ enabled cluster using the AWS CLI operation `test-failover`\.

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

Output from the preceding command looks something like the following\.

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
        "MultiAZ": "enabled",
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
+ [test\-failover](https://docs.aws.amazon.com/cli/latest/reference/elasticache/test-failover.html) in the *AWS CLI Command Reference\.*
+ [describe\-events](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-events.html) in the *AWS CLI Command Reference\.*

 

### Testing Automatic Failover Using the ElastiCache API<a name="auto-failover-test-api"></a>

You can test automatic failover on any cluster enabled with Multi\-AZ using the ElastiCache API operation `TestFailover`\.

**Parameters**
+ `ReplicationGroupId` – Required\. The replication group \(on the console, cluster\) to be tested\.
+ `NodeGroupId` – Required\. The name of the node group that you want to test automatic failover on\. You can test a maximum of five node groups in a rolling 24\-hour period\.

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

To track the progress of your failover, use the ElastiCache `DescribeEvents` API operation\.

For more information, see the following:
+ [TestFailover](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_TestFailover.html) in the *ElastiCache API Reference * 
+ [DescribeEvents](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEvents.html) in the *ElastiCache API Reference * 

 

## Limitations on Redis Multi\-AZ<a name="AutoFailover.Notes"></a>

Be aware of the following limitations for Redis Multi\-AZ:
+ Multi\-AZ is supported on Redis version 2\.8\.6 and later\.
+ Redis Multi\-AZ isn't supported on T1 node types\.
+ Redis replication is asynchronous\. Therefore, when a primary node fails over to a replica, a small amount of data might be lost due to replication lag\.

  When choosing the replica to promote to primary, ElastiCache for Redis chooses the replica with the least replication lag\. In other words, it chooses the replica that is most current\. Doing so helps minimize the amount of lost data\. The replica with the least replication lag can be in the same or different Availability Zone from the failed primary node\.
+ When you manually promote read replicas to primary, you can do so only when Multi\-AZ and automatic failover are disabled\. To promote a read replica to primary, take the following steps:

  1. Disable Multi\-AZ on the cluster\.

  1. Disable automatic failover on the cluster\.

  1. Promote the read replica to primary\.

  1. Re\-enable Multi\-AZ\.

  You can't disable Multi\-AZ on Redis \(cluster mode enabled\) clusters\. Therefore, you can't manually promote a replica to primary on any Redis \(cluster mode enabled\) cluster\.
+ ElastiCache for Redis Multi\-AZ and append\-only file \(AOF\) are mutually exclusive\. If you enable one, you can't enable the other\.
+ A node's failure can be caused by the rare event of an entire Availability Zone failing\. In this case, the replica replacing the failed primary is created only when the Availability Zone is back up\. For example, consider a replication group with the primary in AZ\-a and replicas in AZ\-b and AZ\-c\. If the primary fails, the replica with the least replication lag is promoted to primary cluster\. Then, ElastiCache creates a new replica in AZ\-a \(where the failed primary was located\) only when AZ\-a is back up and available\.
+ A customer\-initiated reboot of a primary doesn't trigger automatic failover\. Other reboots and failures do trigger automatic failover\.
+ When the primary is rebooted, it's cleared of data when it comes back online\. When the read replicas see the cleared primary cluster, they clear their copy of the data, which causes data loss\.
+ After a read replica has been promoted, the other replicas sync with the new primary\. After the initial sync, the replicas' content is deleted and they sync the data from the new primary\. This sync process causes a brief interruption, during which the replicas are not accessible\. The sync process also causes a temporary load increase on the primary while syncing with the replicas\. This behavior is native to Redis and isn't unique to ElastiCache Multi\-AZ\. For details about this Redis behavior, see [Replication](http://redis.io/topics/replication) on the Redis website\.

**Important**  
For Redis version 2\.8\.22 and later, you can't create external replicas\.  
For Redis versions before 2\.8\.22, we recommend that you don't connect an external Redis replica to an ElastiCache for Redis cluster that is Multi\-AZ enabled\. This unsupported configuration can create issues that prevent ElastiCache from properly performing failover and recovery\. To connect an external Redis replica to an ElastiCache cluster, make sure that Multi\-AZ isn't enabled before you make the connection\.