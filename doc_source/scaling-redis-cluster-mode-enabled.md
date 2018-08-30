# Scaling for Amazon ElastiCache for Redis—Redis \(cluster mode enabled\)<a name="scaling-redis-cluster-mode-enabled"></a>

As demand on your clusters changes, you might decide to improve performance or reduce costs by changing the number of shards in your Redis \(cluster mode enabled\) cluster\. We recommend using online horizontal scaling to do so, because it allows your cluster to continue serving requests during the scaling process\.

Conditions under which you might decide to rescale your cluster include the following:

+ **Memory pressure:**

  If the nodes in your cluster are under memory pressure, you might decide to scale out so that you have more resources to better store data and serve requests\.

  You can determine whether your nodes are under memory pressure by monitoring the following metrics: *FreeableMemory*, *SwapUsage*, and *BytesUseForCache*\.

+ **CPU or network bottleneck:**

  If latency/throughput issues are plaguing your cluster, you might need to scale out to resolve the issues\.

  You can monitor your latency and throughput levels by monitoring the following metrics: *CPUUtilization*, *NetworkBytesIn*, *NetworkBytesOut*, *CurrConnections*, and *NewConnections*\.

+ **Your cluster is over\-scaled:**

  Current demand on your cluster is such that scaling in doesn't hurt performance and reduces your costs\.

  You can monitor your cluster's use to determine whether or not you can safely scale in using the following metrics: *FreeableMemory*, *SwapUseage*, *BytesUseForCache*, *CPUUtilization*, *NetworkBytesIn*, *NetworkBytesOut*, *CurrConnections*, and *NewConnections*\.

**Performance Impact of Scaling**  
When you scale using the offline process, your cluster is offline for a significant portion of the process and thus unable to serve requests\. When you scale using the online method, because scaling is a compute\-intensive operation, there is some degradation in performance, nevertheless, your cluster continues to serve requests throughout the scaling operation\. How much degradation you experience depends upon your normal CPU utilization and your data\.

There are two ways to scale your Redis \(cluster mode enabled\) cluster; offline and online\. Whichever you choose, you can do the following:

+ Change the number of node groups \(shards\) in the replication group by adding or removing node groups\.

+ Configure the slots in your new cluster differently than they were in the old cluster\. Offline method only\.

+ Change the node type of the cluster's nodes\. If you are changing to a smaller node type, be sure that the new node size has sufficient memory for your data and Redis overhead\. Offline method only\.

  For more information, see [Choosing Your Node Size](CacheNodes.SelectSize.md)\.


+ [Offline Resharding and Cluster Reconfiguration for ElastiCache for Redis—Redis \(cluster mode enabled\)](#redis-cluster-resharding-offline)
+ [Online Resharding and Shard Rebalancing for ElastiCache for Redis—Redis \(cluster mode enabled\)](#redis-cluster-resharding-online)

## Offline Resharding and Cluster Reconfiguration for ElastiCache for Redis—Redis \(cluster mode enabled\)<a name="redis-cluster-resharding-offline"></a>

The main advantage you get from offline shard reconfiguration is that you can do more than merely add or remove shards from your replication group\. When you reshard offline, in addition to changing the number of shards in your replication group, you can do the following:

+ Change the node type of your replication group\.

+ Specify the Availability Zone for each node in the replication group\.

+ Upgrade to a newer engine version\.

+ Specify the number of replica nodes in each shard independently\.

+ Specify the keyspace for each shard\.

The main disadvantage of offline shard reconfiguration is that your cluster is offline beginning with the restore portion of the process and continuing until you update the endpoints in your application\. The length of time that your cluster is offline varies with the amount of data in your cluster\.

**To reconfigure your shards Redis \(cluster mode enabled\) cluster offline**

1. Create a manual backup of your existing Redis cluster\. For more information, see [Making Manual Backups](backups-manual.md)\.

1. Create a new cluster by restoring from the backup\. For more information, see [Restoring From a Backup with Optional Cluster Resizing](backups-restoring.md)\.

1. Update the endpoints in your application to the new cluster's endpoints\. For more information, see [Finding Your ElastiCache Endpoints](Endpoints.md)\.

## Online Resharding and Shard Rebalancing for ElastiCache for Redis—Redis \(cluster mode enabled\)<a name="redis-cluster-resharding-online"></a>

By using online resharding and shard rebalancing with Amazon ElastiCache for Redis, you can scale your ElastiCache for Redis \(cluster mode enabled\) dynamically with no downtime\. This approach means that your cluster can continue to serve requests even while scaling or rebalancing is in process\.

You can do the following:

+ **Scale out** – Increase read and write capacity by adding shards \(node groups\) to your Redis \(cluster mode enabled\) cluster \(replication group\)\.

  If you add one or more shards to your replication group, the number of nodes in each new shard is the same as the number of nodes in the smallest of the existing shards\.

+ **Scale in** – Reduce read and write capacity, and thereby costs, by removing shards from your Redis \(cluster mode enabled\) cluster\.

+ **Rebalance** – Move the keyspaces among the shards in your ElastiCache for Redis \(cluster mode enabled\) cluster so they are as equally distributed among the shards as possible\.

You can't do the following:

+ **Scale up/down:** Change your node type\. To do this, you must use the offline process\.

+ **Upgrade your engine:** Change your engine version to a newer version\. To do this, you must use the offline process\.

+ **Configure shards independently:**

  + You can't specify the number of nodes in each shard independently\. To do this, you must use the offline process\.

  + You can't specify the keyspace for shards independently\. To do this, you must use the offline process\.

Currently, the following limitations apply to ElastiCache for Redis online resharding and rebalancing:

+ These processes require Redis engine version 3\.2\.10 or newer\. For information on upgrading your engine version, see [Upgrading Engine Versions](VersionManagement.md)\.

+ There are limitations with slots or keyspaces and large items:

  If any of the keys in a shard contain a large item, that key isn't migrated to a new shard when scaling out or rebalancing\. This functionality can result in unbalanced shards\.

  If any of the keys in a shard contain a large item \(items greater than 256 MB after serialization\), that shard isn't deleted when scaling in\. This functionality can result in some shards not being deleted\.

+ When scaling out, the number of nodes in any new shards equals the number of nodes in the smallest existing shard\.

+ When scaling out, any tags that are common to all existing shards are copied to the new shards\.

For more information, see [Best Practices: Online Resharding](best-practices-online-resharding.md)\.

You can horizontally scale or rebalance your ElastiCache for Redis \(cluster mode enabled\) clusters using the AWS Management Console, the AWS CLI, and the ElastiCache API\.

### Adding Shards with Online Resharding<a name="redis-cluster-resharding-online-add"></a>

You can add shards to your Redis \(cluster mode enabled\) cluster using the AWS Management Console, AWS CLI, or ElastiCache API\. When you add shards to a Redis \(cluster mode enabled\) cluster, any tags on the existing shards are copied over to the new shards\.


+ [Adding Shards \(Console\)](#redis-cluster-resharding-online-add-console)
+ [Adding Shards \(AWS CLI\)](#redis-cluster-resharding-online-add-cli)
+ [Adding Shards \(ElastiCache API\)](#redis-cluster-resharding-online-add-api)

#### Adding Shards \(Console\)<a name="redis-cluster-resharding-online-add-console"></a>

You can use the AWS Management Console to add one or more shards to your Redis \(cluster mode enabled\) cluster\. The following procedure describes the process\.

**To add shards to your Redis \(cluster mode enabled\) cluster**

1. Open the ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Redis**\.

1. Locate and choose the name of the Redis \(cluster mode enabled\) cluster that you want to add shards to\.
**Tip**  
Redis \(cluster mode enabled\) clusters have a value of 1 or greater in the **Shards** column\.

1. Choose **Add shard**\.

   1. For **Number of shards to be added**, choose the number of shards you want added to this cluster\.

   1. For **Availability zone\(s\)**, choose either **No preference** or **Specify availability zones**\.

   1. If you chose **Specify availability zones**, for each node in each shard, select the node's Availability Zone from the list of Availability Zones\.

   1. Choose **Add**\.

#### Adding Shards \(AWS CLI\)<a name="redis-cluster-resharding-online-add-cli"></a>

The following process describes how to reconfigure the shards in your Redis \(cluster mode enabled\) cluster by adding shards using the AWS CLI\.

Use the following parameters with `modify-replication-group-shard-configuration`\.

**Parameters**

+ `--apply-immediately` – Required\. Specifies the shard reconfiguration operation is to be started immediately\.

+ `--replication-group-id` – Required\. Specifies which replication group \(cluster\) the shard reconfiguration operation is to be performed on\.

+ `--node-group-count` – Required\. Specifies the number of shards \(node groups\) to exist when the operation is completed\. When adding shards, the value of `--node-group-count` must be greater than the current number of shards\.

  Optionally, you can specify the Availability Zone for each node in the replication group using `--resharding-configuration`\.

+ `--resharding-configuration` – Optional\. A list of preferred Availability Zones for each node in each shard in the replication group\. Use this parameter only if the value of `--node-group-count` is greater than the current number of shards\. If this parameter is omitted when adding shards, Amazon ElastiCache selects the Availability Zones for the new nodes\.

The following example reconfigures the keyspaces over four shards in the Redis \(cluster mode enabled\) cluster `my-cluster`\. The example also specifies the Availability Zone for each node in each shard\. The operation begins immediately\.

**Example \- Adding Shards**  
For Linux, macOS, or Unix:  

```
aws elasticache modify-replication-group-shard-configuration \
    --replication-group-id my-cluster \
    --node-group-count 4 \
    --resharding-configuration \
        "PreferredAvailabilityZones=us-east-2a,us-east-2c" \
        "PreferredAvailabilityZones=us-east-2b,us-east-2a" \
        "PreferredAvailabilityZones=us-east-2c,us-east-2d" \
        "PreferredAvailabilityZones=us-east-2d,us-east-2c" \
    --apply-immediately
```
For Windows:  

```
aws elasticache modify-replication-group-shard-configuration ^
    --replication-group-id my-cluster ^
    --node-group-count 4 ^
    --resharding-configuration ^
        "PreferredAvailabilityZones=us-east-2a,us-east-2c" ^
        "PreferredAvailabilityZones=us-east-2b,us-east-2a" ^
        "PreferredAvailabilityZones=us-east-2c,us-east-2d" ^
        "PreferredAvailabilityZones=us-east-2d,us-east-2c" ^
    --apply-immediately
```

For more information, see [modify\-replication\-group\-shard\-configuration](http://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group-shard-configuration.html) in the AWS CLI documentation\.

#### Adding Shards \(ElastiCache API\)<a name="redis-cluster-resharding-online-add-api"></a>

You can use the ElastiCache API to reconfigure the shards in your Redis \(cluster mode enabled\) cluster online by using the `ModifyReplicationGroupShardConfiguration` operation\.

Use the following parameters with `ModifyReplicationGroupShardConfiguration`\.

**Parameters**

+ `ApplyImmediately=true` – Required\. Specifies the shard reconfiguration operation is to be started immediately\.

+ `ReplicationGroupId` – Required\. Specifies which replication group \(cluster\) the shard reconfiguration operation is to be performed on\.

+ `NodeGroupCount` – Required\. Specifies the number of shards \(node groups\) to exist when the operation is completed\. When adding shards, the value of `NodeGroupCount` must be greater than the current number of shards\.

  Optionally, you can specify the Availability Zone for each node in the replication group using `ReshardingConfiguration`\.

+ `ReshardingConfiguration` – Optional\. A list of preferred Availability Zones for each node in each shard in the replication group\. Use this parameter only if the value of `NodeGroupCount` is greater than the current number of shards\. If this parameter is omitted when adding shards, Amazon ElastiCache selects the Availability Zones for the new nodes\.

The following process describes how to reconfigure the shards in your Redis \(cluster mode enabled\) cluster by adding shards using the ElastiCache API\.

**Example \- Adding Shards**  
The following example adds node groups to the Redis \(cluster mode enabled\) cluster `my-cluster`, so there are a total of four node groups when the operation completes\. The example also specifies the Availability Zone for each node in each shard\. The operation begins immediately\.  

```
https://elasticache.us-east-2.amazonaws.com/
    ?Action=ModifyReplicationGroupShardConfiguration
    &ApplyImmediately=true
    &NodeGroupCount=4
    &ReplicationGroupId=my-cluster
    &ReshardingConfiguration.ReshardingConfiguration.1.PreferredAvailabilityZones.AvailabilityZone.1=us-east-2a 
    &ReshardingConfiguration.ReshardingConfiguration.1.PreferredAvailabilityZones.AvailabilityZone.2=us-east-2c 
    &ReshardingConfiguration.ReshardingConfiguration.2.PreferredAvailabilityZones.AvailabilityZone.1=us-east-2b 
    &ReshardingConfiguration.ReshardingConfiguration.2.PreferredAvailabilityZones.AvailabilityZone.2=us-east-2a 
    &ReshardingConfiguration.ReshardingConfiguration.3.PreferredAvailabilityZones.AvailabilityZone.1=us-east-2c 
    &ReshardingConfiguration.ReshardingConfiguration.3.PreferredAvailabilityZones.AvailabilityZone.2=us-east-2d 
    &ReshardingConfiguration.ReshardingConfiguration.4.PreferredAvailabilityZones.AvailabilityZone.1=us-east-2d 
    &ReshardingConfiguration.ReshardingConfiguration.4.PreferredAvailabilityZones.AvailabilityZone.2=us-east-2c 
    &Version=2015-02-02
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20171002T192317Z
    &X-Amz-Credential=<credential>
```

For more information, see [ModifyReplicationGroupShardConfiguration](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference//AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroupShardConfiguration.html) in the ElastiCache API Reference\.

### Removing Shards with Online Resharding<a name="redis-cluster-resharding-online-remove"></a>

You can remove shards from your Redis \(cluster mode enabled\) cluster using the AWS Management Console, AWS CLI, or ElastiCache API\.


+ [Removing Shards \(Console\)](#redis-cluster-resharding-online-remove-console)
+ [Removing Shards \(AWS CLI\)](#redis-cluster-resharding-online-remove-cli)
+ [Removing Shards \(ElastiCache API\)](#redis-cluster-resharding-online-remove-api)

#### Removing Shards \(Console\)<a name="redis-cluster-resharding-online-remove-console"></a>

The following process describes how to reconfigure the shards in your Redis \(cluster mode enabled\) cluster by removing shards using the AWS Management Console\.

Before removing node groups \(shards\) from your replication group, ElastiCache makes sure that all your data fits in the remaining shards\. If the data fits, the specified shards are deleted from the replication group as requested\. If the data doesn't fit in the remaining node groups, the process is terminated\. Terminating the process leaves your replication group with the same node group configuration as before the request was made\.

You can use the AWS Management Console to remove one or more shards from your Redis \(cluster mode enabled\) cluster\. You cannot remove all the shards in a replication group\. Instead, you must delete the replication group\. For more information, see [Deleting a Cluster with Replicas](Replication.DeletingRepGroup.md)\. The following procedure describes the process for deleting one or more shards\.

**To remove shards from your Redis \(cluster mode enabled\) cluster**

1. Open the ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Redis**\.

1. Locate and choose the name of the Redis \(cluster mode enabled\) cluster you want to remove shards from\.
**Tip**  
Redis \(cluster mode enabled\) clusters have a value of 1 or greater in the **Shards** column\.

1. From the list of shards, choose the box by the name of each shard that you want to delete\.

1. Choose **Delete shard**\.

#### Removing Shards \(AWS CLI\)<a name="redis-cluster-resharding-online-remove-cli"></a>

The following process describes how to reconfigure the shards in your Redis \(cluster mode enabled\) cluster by removing shards using the AWS CLI\.

**Important**  
Before removing node groups \(shards\) from your replication group, ElastiCache makes sure that all your data fits in the remaining shards\. If the data fits, the specified shards \(`--node-groups-to-remove`\) are deleted from the replication group as requested and their keyspaces mapped into the remaining shards\. If the data doesn't fit in the remaining node groups, the process is terminated\. Terminating the process leaves your replication group with the same node group configuration as before the request was made\.

You can use the AWS CLI to remove one or more shards from your Redis \(cluster mode enabled\) cluster\. You cannot remove all the shards in a replication group\. Instead, you must delete the replication group\. For more information, see [Deleting a Cluster with Replicas](Replication.DeletingRepGroup.md)\.

Use the following parameters with `modify-replication-group-shard-configuration`\.

**Parameters**

+ `--apply-immediately` – Required\. Specifies the shard reconfiguration operation is to be started immediately\.

+ `--replication-group-id` – Required\. Specifies which replication group \(cluster\) the shard reconfiguration operation is to be performed on\.

+ `--node-group-count` – Required\. Specifies the number of shards \(node groups\) to exist when the operation is completed\. When removing shards, the value of `--node-group-count` must be less than the current number of shards\.

+ `--node-groups-to-remove` – Required when `--node-group-count` is less than the current number of node groups \(shards\)\. A list of shard \(node group\) IDs to remove from the replication group\.

The following procedure describes the process for deleting one or more shards\.

**Example \- Removing Shards**  
The following example removes two node groups from the Redis \(cluster mode enabled\) cluster `my-cluster`, so there are a total of two node groups when the operation completes\. The keyspaces from the removed shards are distributed evenly over the remaining shards\.  
For Linux, macOS, or Unix:  

```
aws elasticache modify-replication-group-shard-configuration \
    --replication-group-id my-cluster \
    --node-group-count 2 \
    --node-groups-to-remove "0002" "0003" \
    --apply-immediately
```
For Windows:  

```
aws elasticache modify-replication-group-shard-configuration ^
    --replication-group-id my-cluster ^
    --node-group-count 2 ^
    --node-groups-to-remove "0002" "0003" ^
    --apply-immediately
```

For more information, see [modify\-replication\-group\-shard\-configuration](http://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group-shard-configuration.html) in the AWS CLI documentation\.

#### Removing Shards \(ElastiCache API\)<a name="redis-cluster-resharding-online-remove-api"></a>

You can use the ElastiCache API to reconfigure the shards in your Redis \(cluster mode enabled\) cluster online by using the `ModifyReplicationGroupShardConfiguration` operation\.

The following process describes how to reconfigure the shards in your Redis \(cluster mode enabled\) cluster by removing shards using the ElastiCache API\.

**Important**  
Before removing node groups \(shards\) from your replication group, ElastiCache makes sure that all your data fits in the remaining shards\. If the data fits, the specified shards \(`NodeGroupsToRemove`\) are deleted from the replication group as requested and their keyspaces mapped into the remaining shards\. If the data doesn't fit in the remaining node groups, the process is terminated\. Terminating the process leaves your replication group with the same node group configuration as before the request was made\.

You can use the ElastiCache API to remove one or more shards from your Redis \(cluster mode enabled\) cluster\. You cannot remove all the shards in a replication group\. Instead, you must delete the replication group\. For more information, see [Deleting a Cluster with Replicas](Replication.DeletingRepGroup.md)\.

Use the following parameters with `ModifyReplicationGroupShardConfiguration`\.

**Parameters**

+ `ApplyImmediately=true` – Required\. Specifies the shard reconfiguration operation is to be started immediately\.

+ `ReplicationGroupId` – Required\. Specifies which replication group \(cluster\) the shard reconfiguration operation is to be performed on\.

+ `NodeGroupCount` – Required\. Specifies the number of shards \(node groups\) to exist when the operation is completed\. When removing shards, the value of `NodeGroupCount` must be less than the current number of shards\.

+ `NodeGroupsToRemove` – Required when `--node-group-count` is less than the current number of node groups \(shards\)\. A list of shard \(node group\) IDs to remove from the replication group\.

The following procedure describes the process for deleting one or more shards\.

**Example \- Removing Shards**  
The following example removes two node groups from the Redis \(cluster mode enabled\) cluster `my-cluster`, so there are a total of two node groups when the operation completes\. The keyspaces from the removed shards are distributed evenly over the remaining shards\.  

```
https://elasticache.us-east-2.amazonaws.com/
    ?Action=ModifyReplicationGroupShardConfiguration
    &ApplyImmediately=true
    &NodeGroupCount=2
    &ReplicationGroupId=my-cluster
    &NodeGroupsToRemove.member.1=0002
    &NodeGroupsToRemove.member.2=0003
    &Version=2015-02-02
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20171002T192317Z
    &X-Amz-Credential=<credential>
```

For more information, see [ModifyReplicationGroupShardConfiguration](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference//AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroupShardConfiguration.html) in the ElastiCache API Reference\.

### Online Shard Rebalancing<a name="redis-cluster-resharding-online-rebalance"></a>

You can rebalance shards in your Redis \(cluster mode enabled\) cluster using the AWS Management Console, AWS CLI, or ElastiCache API\.


+ [Online Shard Rebalancing \(Console\)](#redis-cluster-resharding-online-rebalance-console)
+ [Online Shard Rebalancing \(AWS CLI\)](#redis-cluster-resharding-online-rebalance-cli)
+ [Online Shard Rebalancing \(ElastiCache API\)](#redis-cluster-resharding-online-rebalance-api)

#### Online Shard Rebalancing \(Console\)<a name="redis-cluster-resharding-online-rebalance-console"></a>

The following process describes how to reconfigure the shards in your Redis \(cluster mode enabled\) cluster by rebalancing shards using the AWS Management Console\.

**To rebalance the keyspaces among the shards on your Redis \(cluster mode enabled\) cluster**

1. Open the ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Redis**\.

1. Choose the name of the Redis \(cluster mode enabled\) cluster that you want to rebalance\.
**Tip**  
Redis \(cluster mode enabled\) clusters have a value of 1 or greater in the **Shards** column\.

1. Choose **Rebalance**\.

1. When prompted, choose **Rebalance**\. You might see a message similar to this one: *Slots in the replication group are uniformly distributed\. Nothing to do\. \(Service: AmazonElastiCache; Status Code: 400; Error Code: InvalidReplicationGroupState; Request ID: 2246cebd\-9721\-11e7\-8d5b\-e1b0f086c8cf\)*\. If you do, choose **Cancel**\.

#### Online Shard Rebalancing \(AWS CLI\)<a name="redis-cluster-resharding-online-rebalance-cli"></a>

Use the following parameters with `modify-replication-group-shard-configuration`\.

**Parameters**

+ `--apply-immediately` – Required\. Specifies the shard reconfiguration operation is to be started immediately\.

+ `--replication-group-id` – Required\. Specifies which replication group \(cluster\) the shard reconfiguration operation is to be performed on\.

+ `--node-group-count` – Required\. To rebalance the keyspaces across all shards in the cluster, this value must be the same as the current number of shards\.

The following process describes how to reconfigure the shards in your Redis \(cluster mode enabled\) cluster by rebalancing shards using the AWS CLI\.

**Example \- Rebalancing the Shards in a Cluster**  
The following example rebalances the slots in the Redis \(cluster mode enabled\) cluster `my-cluster` so that the slots are distributed as equally as possible\. The value of `--node-group-count` \(`4`\) is the number of shards currently in the cluster\.  
For Linux, macOS, or Unix:  

```
aws elasticache modify-replication-group-shard-configuration \
    --replication-group-id my-cluster \
    --node-group-count 4 \
    --apply-immediately
```
For Windows:  

```
aws elasticache modify-replication-group-shard-configuration ^
    --replication-group-id my-cluster ^
    --node-group-count 4 ^
    --apply-immediately
```

For more information, see [modify\-replication\-group\-shard\-configuration](http://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group-shard-configuration.html) in the AWS CLI documentation\.

#### Online Shard Rebalancing \(ElastiCache API\)<a name="redis-cluster-resharding-online-rebalance-api"></a>

You can use the ElastiCache API to reconfigure the shards in your Redis \(cluster mode enabled\) cluster online by using the `ModifyReplicationGroupShardConfiguration` operation\.

Use the following parameters with `ModifyReplicationGroupShardConfiguration`\.

**Parameters**

+ `ApplyImmediately=true` – Required\. Specifies the shard reconfiguration operation is to be started immediately\.

+ `ReplicationGroupId` – Required\. Specifies which replication group \(cluster\) the shard reconfiguration operation is to be performed on\.

+ `NodeGroupCount` – Required\. To rebalance the keyspaces across all shards in the cluster, this value must be the same as the current number of shards\.

The following process describes how to reconfigure the shards in your Redis \(cluster mode enabled\) cluster by rebalancing the shards using the ElastiCache API\.

**Example \- Rebalancing a Cluster**  
The following example rebalances the slots in the Redis \(cluster mode enabled\) cluster `my-cluster` so that the slots are distributed as equally as possible\. The value of `NodeGroupCount` \(`4`\) is the number of shards currently in the cluster\.  

```
https://elasticache.us-east-2.amazonaws.com/
    ?Action=ModifyReplicationGroupShardConfiguration
    &ApplyImmediately=true
    &NodeGroupCount=4
    &ReplicationGroupId=my-cluster
    &Version=2015-02-02
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20171002T192317Z
    &X-Amz-Credential=<credential>
```

For more information, see [ModifyReplicationGroupShardConfiguration](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference//AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroupShardConfiguration.html) in the ElastiCache API Reference\.