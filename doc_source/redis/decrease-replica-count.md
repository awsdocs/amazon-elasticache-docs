# Decreasing the Number of Replicas in a Shard<a name="decrease-replica-count"></a>

You can decrease the number of replicas in a shard for Redis \(cluster mode enabled\), or in a replication group for Redis \(cluster mode disabled\):
+ For Redis \(cluster mode disabled\), you can decrease the number of replicas to one if Multi\-AZ is enabled, and to zero if it isn't enabled\.
+ For Redis \(cluster mode enabled\), you can decrease the number of replicas to zero\. However, you can't fail over to a replica if your primary node fails\.

You can use the AWS Management Console, the AWS CLI or the ElastiCache API to decrease the number of replicas in a node group \(shard\) or replication group\.

**Topics**
+ [Using the AWS Management Console](#decrease-replica-count-con)
+ [Using the AWS CLI](#decrease-replica-count-cli)
+ [Using the ElastiCache API](#decrease-replica-count-api)

## Using the AWS Management Console<a name="decrease-replica-count-con"></a>

The following procedure uses the console to decrease the number of replicas in a Redis \(cluster mode enabled\) replication group\.

**To decrease the number of replicas in a Redis shard**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Redis**, then choose the name of the replication group from which you want to delete replicas\.

1. Choose the box for each shard you want to remove a replica node from\.

1. Choose **Delete replicas**\.

1. Complete the **Delete Replicas from Shards** page:

   1. For **New number of replicas/shard**, enter the number of replicas that you want the selected shards to have\. This number must be greater than or equal to 1\. We recommend at least two replicas per shard as a working minimum\.

   1. Choose **Delete** to delete the replicas or **Cancel** to cancel the operation\.

**Important**  
If you don’t specify the replica nodes to be deleted, ElastiCache for Redis automatically selects replica nodes for deletion\. While doing so, ElastiCache for Redis attempts to retain the Multi\-AZ architecture for your replication group followed by retaining replicas with minimum replication lag with the master\.
You can't delete the primary or master nodes in a replication group\. If you specify a primary node for deletion, the operation fails with an error event indicating that the primary node was selected for deletion\. 

## Using the AWS CLI<a name="decrease-replica-count-cli"></a>

To decrease the number of replicas in a Redis shard, use the `decrease-replica-count` command with the following parameters:
+ `--replication-group-id` – Required\. Identifies which replication group you want to decrease the number of replicas in\.
+ `--apply-immediately` or `--no-apply-immediately` – Required\. Specifies whether to decrease the replica count immediately \(`--apply-immediately`\) or at the next maintenance window \(`--no-apply-immediately`\)\. Currently, `--no-apply-immediately` is not supported\.
+ `--new-replica-count` – Optional\. Specifies the number of replica nodes that you want\. The value of `--new-replica-count` must be a valid value less than the current number of replicas in the node groups\. For minimum permitted values, see [Decreasing the Number of Replicas in a Shard](#decrease-replica-count)\. If the value of `--new-replica-count` doesn't meet this requirement, the call fails\.
+ `--replicas-to-remove` – Optional\. Contains a list of node IDs specifying the replica nodes to remove\.
+ `--replica-configuration` – Optional\. Allows you to set the number of replicas and Availability Zones for each node group independently\. Use this parameter for Redis \(cluster mode enabled\) groups where you want to configure each node group independently\. 

  `--replica-configuration` has three optional members:
  + `NodeGroupId` – The four\-digit ID for the node group that you are configuring\. For Redis \(cluster mode disabled\) replication groups, the shard ID is always `0001`\. To find a Redis \(cluster mode enabled\) node group's \(shard's\) ID, see [Finding a Shard's ID](shard-find-id.md)\.
  + `NewReplicaCount` – An optional parameter that specifies the number of replica nodes you want\. The value of `NewReplicaCount` must be a valid value less than the current number of replicas in the node groups\. For minimum permitted values, see [Decreasing the Number of Replicas in a Shard](#decrease-replica-count)\. If the value of `NewReplicaCount` doesn't meet this requirement, the call fails\.
  + `PreferredAvailabilityZones` – A list of `PreferredAvailabilityZone` strings that specify which Availability Zones the replication group's nodes are in\. The number of `PreferredAvailabilityZone` values must equal the value of `NewReplicaCount` plus 1 to account for the primary node\. If this member of `--replica-configuration` is omitted, ElastiCache for Redis chooses the Availability Zone for each of the new replicas\.

**Important**  
You must include one and only one of the `--new-replica-count`, `--replicas-to-remove`, or `--replica-configuration` parameters\.

**Example**  
The following example uses `--new-replica-count` to decrease the number of replicas in the replication group `sample-repl-group` to one\. When the example is finished, there is one replica in each node group\. This number applies whether this is a Redis \(cluster mode disabled\) group with a single node group or a Redis \(cluster mode enabled\) group with multiple node groups\.  
For Linux, macOS, or Unix:  

```
aws elasticache decrease-replica-count
    --replication-group-id sample-repl-group \
    --new-replica-count 1 \
    --apply-immediately
```
For Windows:  

```
aws elasticache decrease-replica-count ^
    --replication-group-id sample-repl-group ^
    --new-replica-count 1 ^
    --apply-immediately
```
The following example decreases the number of replicas in the replication group `sample-repl-group` by removing two specified replicas \(`0001` and `0003`\) from the node group\.  
For Linux, macOS, or Unix:  

```
aws elasticache decrease-replica-count \
    --replication-group-id sample-repl-group \
    --replicas-to-remove 0001,0003 \
    --apply-immediately
```
For Windows:  

```
aws elasticache decrease-replica-count ^
    --replication-group-id sample-repl-group ^
    --replicas-to-remove 0001,0003 \
    --apply-immediately
```
The following example uses `--replica-configuration` to decrease the number of replicas in the replication group `sample-repl-group` to the value specified for the two specified node groups\. Given that there are multiple node groups, this is a Redis \(cluster mode enabled\) replication group\. When specifying the optional `PreferredAvailabilityZones`, the number of Availability Zones listed must equal the value of `NewReplicaCount` plus 1 more\. This approach accounts for the primary node for the group identified by `NodeGroupId`\.  
For Linux, macOS, or Unix:  

```
aws elasticache decrease-replica-count \
    --replication-group-id sample-repl-group \
    --replica-configuration \
        NodeGroupId=0001,NewReplicaCount=1,PreferredAvailabilityZones=us-east-1a,us-east-1c \
        NodeGroupId=0003,NewReplicaCount=2,PreferredAvailabilityZones=us-east-1a,us-east-1b,us-east-1c \
    --apply-immediately
```
For Windows:  

```
aws elasticache decrease-replica-count ^
    --replication-group-id sample-repl-group ^
    --replica-configuration ^
        NodeGroupId=0001,NewReplicaCount=2,PreferredAvailabilityZones=us-east-1a,us-east-1c ^
        NodeGroupId=0003,NewReplicaCount=3,PreferredAvailabilityZones=us-east-1a,us-east-1b,us-east-1c \
    --apply-immediately
```

For more information about decreasing the number of replicas using the CLI, see [decrease\-replica\-count](https://docs.aws.amazon.com/cli/latest/reference/elasticache/decrease-replica-count.html) in the *Amazon ElastiCache Command Line Reference\.*

## Using the ElastiCache API<a name="decrease-replica-count-api"></a>

To decrease the number of replicas in a Redis shard, use the `DecreaseReplicaCount` action with the following parameters:
+ `ReplicationGroupId` – Required\. Identifies which replication group you want to decrease the number of replicas in\.
+ `ApplyImmediately` – Required\. Specifies whether to decrease the replica count immediately \(`ApplyImmediately=True`\) or at the next maintenance window \(`ApplyImmediately=False`\)\. Currently, `ApplyImmediately=False` is not supported\.
+ `NewReplicaCount` – Optional\. Specifies the number of replica nodes you want\. The value of `NewReplicaCount` must be a valid value less than the current number of replicas in the node groups\. For minimum permitted values, see [Decreasing the Number of Replicas in a Shard](#decrease-replica-count)\. If the value of `--new-replica-count` doesn't meet this requirement, the call fails\.
+ `ReplicasToRemove` – Optional\. Contains a list of node IDs specifying the replica nodes to remove\.
+ `ReplicaConfiguration` – Optional\. Contains a list of node groups that allows you to set the number of replicas and Availability Zones for each node group independently\. Use this parameter for Redis \(cluster mode enabled\) groups where you want to configure each node group independently\. 

  `ReplicaConfiguraion` has three optional members:
  + `NodeGroupId` – The four\-digit ID for the node group you are configuring\. For Redis \(cluster mode disabled\) replication groups, the node group ID is always `0001`\. To find a Redis \(cluster mode enabled\) node group's \(shard's\) ID, see [Finding a Shard's ID](shard-find-id.md)\.
  + `NewReplicaCount` – The number of replicas that you want in this node group at the end of this operation\. The value must be less than the current number of replicas down to a minimum of 1 if Multi\-AZ is enabled or 0 if Multi\-AZ with Automatic Failover isn't enabled\. If this value is not less than the current number of replicas in the node group, the call fails with an exception\.
  + `PreferredAvailabilityZones` – A list of `PreferredAvailabilityZone` strings that specify which Availability Zones the replication group's nodes are in\. The number of `PreferredAvailabilityZone` values must equal the value of `NewReplicaCount` plus 1 to account for the primary node\. If this member of `ReplicaConfiguration` is omitted, ElastiCache for Redis chooses the Availability Zone for each of the new replicas\.

**Important**  
You must include one and only one of the `NewReplicaCount`, `ReplicasToRemove`, or `ReplicaConfiguration` parameters\.

**Example**  
The following example uses `NewReplicaCount` to decrease the number of replicas in the replication group `sample-repl-group` to one\. When the example is finished, there is one replica in each node group\. This number applies whether this is a Redis \(cluster mode disabled\) group with a single node group or a Redis \(cluster mode enabled\) group with multiple node groups\.  

```
https://elasticache.us-west-2.amazonaws.com/
      ?Action=DecreaseReplicaCount
      &ApplyImmediately=True
      &NewReplicaCount=1
      &ReplicationGroupId=sample-repl-group
      &Version=2015-02-02
      &SignatureVersion=4
      &SignatureMethod=HmacSHA256
      &Timestamp=20150202T192317Z
      &X-Amz-Credential=<credential>
```
The following example decreases the number of replicas in the replication group `sample-repl-group` by removing two specified replicas \(`0001` and `0003`\) from the node group\.  

```
https://elasticache.us-west-2.amazonaws.com/
      ?Action=DecreaseReplicaCount
      &ApplyImmediately=True
      &ReplicasToRemove.ReplicaToRemove.1=0001
      &ReplicasToRemove.ReplicaToRemove.2=0003
      &ReplicationGroupId=sample-repl-group
      &Version=2015-02-02
      &SignatureVersion=4
      &SignatureMethod=HmacSHA256
      &Timestamp=20150202T192317Z
      &X-Amz-Credential=<credential>
```
The following example uses `ReplicaConfiguration` to decrease the number of replicas in the replication group `sample-repl-group` to the value specified for the two specified node groups\. Given that there are multiple node groups, this is a Redis \(cluster mode enabled\) replication group\. When specifying the optional `PreferredAvailabilityZones`, the number of Availability Zones listed must equal the value of `NewReplicaCount` plus 1 more\. This approach accounts for the primary node for the group identified by `NodeGroupId`\.  

```
https://elasticache.us-west-2.amazonaws.com/
      ?Action=DecreaseReplicaCount
      &ApplyImmediately=True
      &ReplicaConfiguration.ConfigureShard.1.NodeGroupId=0001
      &ReplicaConfiguration.ConfigureShard.1.NewReplicaCount=1
      &ReplicaConfiguration.ConfigureShard.1.PreferredAvailabilityZones.PreferredAvailabilityZone.1=us-east-1a
      &ReplicaConfiguration.ConfigureShard.1.PreferredAvailabilityZones.PreferredAvailabilityZone.2=us-east-1c
      &ReplicaConfiguration.ConfigureShard.2.NodeGroupId=0003
      &ReplicaConfiguration.ConfigureShard.2.NewReplicaCount=2
      &ReplicaConfiguration.ConfigureShard.2.PreferredAvailabilityZones.PreferredAvailabilityZone.1=us-east-1a
      &ReplicaConfiguration.ConfigureShard.2.PreferredAvailabilityZones.PreferredAvailabilityZone.2=us-east-1b
      &ReplicaConfiguration.ConfigureShard.2.PreferredAvailabilityZones.PreferredAvailabilityZone.4=us-east-1c
      &ReplicationGroupId=sample-repl-group
      &Version=2015-02-02
      &SignatureVersion=4
      &SignatureMethod=HmacSHA256
      &Timestamp=20150202T192317Z
      &X-Amz-Credential=<credential>
```

For more information about decreasing the number of replicas using the API, see [DecreaseReplicaCount](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DecreaseReplicaCount.html) in the *Amazon ElastiCache API Reference\.*