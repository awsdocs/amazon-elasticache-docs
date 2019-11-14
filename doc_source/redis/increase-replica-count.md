# Increasing the Number of Replicas in a Shard<a name="increase-replica-count"></a>

You can increase the number of replicas in a Redis \(cluster mode enabled\) shard or Redis \(cluster mode disabled\) replication group up to a maximum of five\. You can do so using the AWS Management Console, the AWS CLI, or the ElastiCache API\.

**Topics**
+ [Using the AWS Management Console](#increase-replica-count-con)
+ [Using the AWS CLI](#increase-replica-count-cli)
+ [Using the ElastiCache API](#increase-replica-count-api)

## Using the AWS Management Console<a name="increase-replica-count-con"></a>

The following procedure uses the console to increase the number of replicas in a Redis \(cluster mode enabled\) replication group\.

**To increase the number of replicas in Redis shards**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Redis**, and then choose the name of the replication group that you want to add replicas to\.

1. Choose the box for each shard that you want to add replicas to\.

1. Choose **Add replicas**\.

1. Complete the **Add Replicas to Shards** page:
   + For **New number of replicas/shard**, enter the number of replicas that you want all of your selected shards to have\. This value must be greater than or equal to **Current Number of Replicas per shard** and less than or equal to five\. We recommend at least two replicas as a working minimum\.
   + For **Availability Zones**, choose either **No preference** to have ElastiCache chose an Availability Zone for each new replica, or **Specify Availability Zones** to choose an Availability Zone for each new replica\.

     If you choose **Specify Availability Zones**, for each new replica specify an Availability Zone using the list\.

1. Choose **Add** to add the replicas or **Cancel** to cancel the operation\.

## Using the AWS CLI<a name="increase-replica-count-cli"></a>

To increase the number of replicas in a Redis shard, use the `increase-replica-count` command with the following parameters:
+ `--replication-group-id` – Required\. Identifies which replication group you want to increase the number of replicas in\.
+ `--apply-immediately` or `--no-apply-immediately` – Required\. Specifies whether to increase the replica count immediately \(`--apply-immediately`\) or at the next maintenance window \(`--no-apply-immediately`\)\. Currently, `--no-apply-immediately` is not supported\.
+ `--new-replica-count` – Optional\. Specifies the number of replica nodes you want when finished, up to a maximum of five\. Use this parameter for Redis \(cluster mode disabled\) replication groups where there is only one node group or Redis \(cluster mode enabled\) group, or where you want all node groups to have the same number of replicas\. If this value is not larger than the current number of replicas in the node group, the call fails with an exception\.
+ `--replica-configuration` – Optional\. Allows you to set the number of replicas and Availability Zones for each node group independently\. Use this parameter for Redis \(cluster mode enabled\) groups where you want to configure each node group independently\. 

  `--replica-configuration` has three optional members:
  + `NodeGroupId` – The four\-digit ID for the node group that you are configuring\. For Redis \(cluster mode disabled\) replication groups, the shard ID is always `0001`\. To find a Redis \(cluster mode enabled\) node group's \(shard's\) ID, see [Finding a Shard's ID](shard-find-id.md)\.
  + `NewReplicaCount` – The number of replicas that you want in this node group at the end of this operation\. The value must be more than the current number of replicas, up to a maximum of five\. If this value is not larger than the current number of replicas in the node group, the call fails with an exception\.
  + `PreferredAvailabilityZones` – A list of `PreferredAvailabilityZone` strings that specify which Availability Zones the replication group's nodes are to be in\. The number of `PreferredAvailabilityZone` values must equal the value of `NewReplicaCount` plus 1 to account for the primary node\. If this member of `--replica-configuration` is omitted, ElastiCache for Redis chooses the Availability Zone for each of the new replicas\.

**Important**  
You must include either the `--new-replica-count` or `--replica-configuration` parameter, but not both, in your call\.

**Example**  
The following example increases the number of replicas in the replication group `sample-repl-group` to three\. When the example is finished, there are three replicas in each node group\. This number applies whether this is a Redis \(cluster mode disabled\) group with a single node group or a Redis \(cluster mode enabled\) group with multiple node groups\.  
For Linux, macOS, or Unix:  

```
aws elasticache increase-replica-count \
    --replication-group-id sample-repl-group \
    --new-replica-count 3 \
    --apply-immediately
```
For Windows:  

```
aws elasticache increase-replica-count ^
    --replication-group-id sample-repl-group ^
    --new-replica-count 3 ^
    --apply-immediately
```
The following example increases the number of replicas in the replication group `sample-repl-group` to the value specified for the two specified node groups\. Given that there are multiple node groups, this is a Redis \(cluster mode enabled\) replication group\. When specifying the optional `PreferredAvailabilityZones`, the number of Availability Zones listed must equal the value of `NewReplicaCount` plus 1 more\. This approach accounts for the primary node for the group identified by `NodeGroupId`\.  
For Linux, macOS, or Unix:  

```
aws elasticache increase-replica-count \
    --replication-group-id sample-repl-group \
    --replica-configuration \
        NodeGroupId=0001,NewReplicaCount=2,PreferredAvailabilityZones=us-east-1a,us-east-1c,us-east-1b \
        NodeGroupId=0003,NewReplicaCount=3,PreferredAvailabilityZones=us-east-1a,us-east-1b,us-east-1c,us-east-1c \
    --apply-immediately
```
For Windows:  

```
aws elasticache increase-replica-count ^
    --replication-group-id sample-repl-group ^
    --replica-configuration ^
        NodeGroupId=0001,NewReplicaCount=2,PreferredAvailabilityZones=us-east-1a,us-east-1c,us-east-1b ^
        NodeGroupId=0003,NewReplicaCount=3,PreferredAvailabilityZones=us-east-1a,us-east-1b,us-east-1c,us-east-1c \
    --apply-immediately
```

For more information about increasing the number of replicas using the CLI, see [increase\-replica\-count](https://docs.aws.amazon.com/cli/latest/reference/elasticache/increase-replica-count.html) in the *Amazon ElastiCache Command Line Reference\.*

## Using the ElastiCache API<a name="increase-replica-count-api"></a>

To increase the number of replicas in a Redis shard, use the `IncreaseReplicaCount` action with the following parameters:
+ `ReplicationGroupId` – Required\. Identifies which replication group you want to increase the number of replicas in\.
+ `ApplyImmediately` – Required\. Specifies whether to increase the replica count immediately \(`ApplyImmediately=True`\) or at the next maintenance window \(`ApplyImmediately=False`\)\. Currently, `ApplyImmediately=False` is not supported\.
+ `NewReplicaCount` – Optional\. Specifies the number of replica nodes you want when finished, up to a maximum of five\. Use this parameter for Redis \(cluster mode disabled\) replication groups where there is only one node group, or Redis \(cluster mode enabled\) groups where you want all node groups to have the same number of replicas\. If this value is not larger than the current number of replicas in the node group, the call fails with an exception\.
+ `ReplicaConfiguration` – Optional\. Allows you to set the number of replicas and Availability Zones for each node group independently\. Use this parameter for Redis \(cluster mode enabled\) groups where you want to configure each node group independently\. 

  `ReplicaConfiguraion` has three optional members:
  + `NodeGroupId` – The four\-digit ID for the node group you are configuring\. For Redis \(cluster mode disabled\) replication groups, the node group \(shard\) ID is always `0001`\. To find a Redis \(cluster mode enabled\) node group's \(shard's\) ID, see [Finding a Shard's ID](shard-find-id.md)\.
  + `NewReplicaCount` – The number of replicas that you want in this node group at the end of this operation\. The value must be more than the current number of replicas and a maximum of five\. If this value is not larger than the current number of replicas in the node group, the call fails with an exception\.
  + `PreferredAvailabilityZones` – A list of `PreferredAvailabilityZone` strings that specify which Availability Zones the replication group's nodes are to be in\. The number of `PreferredAvailabilityZone` values must equal the value of `NewReplicaCount` plus 1 to account for the primary node\. If this member of `ReplicaConfiguration` is omitted, ElastiCache for Redis chooses the Availability Zone for each of the new replicas\.

**Important**  
You must include either the `NewReplicaCount` or `ReplicaConfiguration` parameter, but not both, in your call\.

**Example**  
The following example increases the number of replicas in the replication group `sample-repl-group` to three\. When the example is finished, there are three replicas in each node group\. This number applies whether this is a Redis \(cluster mode disabled\) group with a single node group or a Redis \(cluster mode enabled\) group with multiple node groups\.  

```
https://elasticache.us-west-2.amazonaws.com/
      ?Action=IncreaseReplicaCount
      &ApplyImmediately=True
      &NewReplicaCount=3
      &ReplicationGroupId=sample-repl-group
      &Version=2015-02-02
      &SignatureVersion=4
      &SignatureMethod=HmacSHA256
      &Timestamp=20150202T192317Z
      &X-Amz-Credential=<credential>
```
The following example increases the number of replicas in the replication group `sample-repl-group` to the value specified for the two specified node groups\. Given that there are multiple node groups, this is a Redis \(cluster mode enabled\) replication group\. When specifying the optional `PreferredAvailabilityZones`, the number of Availability Zones listed must equal the value of `NewReplicaCount` plus 1 more\. This approach accounts for the primary node, for the group identified by `NodeGroupId`\.  

```
https://elasticache.us-west-2.amazonaws.com/
      ?Action=IncreaseReplicaCount
      &ApplyImmediately=True
      &ReplicaConfiguration.ConfigureShard.1.NodeGroupId=0001
      &ReplicaConfiguration.ConfigureShard.1.NewReplicaCount=2
      &ReplicaConfiguration.ConfigureShard.1.PreferredAvailabilityZones.PreferredAvailabilityZone.1=us-east-1a
      &ReplicaConfiguration.ConfigureShard.1.PreferredAvailabilityZones.PreferredAvailabilityZone.2=us-east-1c
      &ReplicaConfiguration.ConfigureShard.1.PreferredAvailabilityZones.PreferredAvailabilityZone.3=us-east-1b
      &ReplicaConfiguration.ConfigureShard.2.NodeGroupId=0003
      &ReplicaConfiguration.ConfigureShard.2.NewReplicaCount=3
      &ReplicaConfiguration.ConfigureShard.2.PreferredAvailabilityZones.PreferredAvailabilityZone.1=us-east-1a
      &ReplicaConfiguration.ConfigureShard.2.PreferredAvailabilityZones.PreferredAvailabilityZone.2=us-east-1b
      &ReplicaConfiguration.ConfigureShard.2.PreferredAvailabilityZones.PreferredAvailabilityZone.3=us-east-1c
      &ReplicaConfiguration.ConfigureShard.2.PreferredAvailabilityZones.PreferredAvailabilityZone.4=us-east-1c
      &ReplicationGroupId=sample-repl-group
      &Version=2015-02-02
      &SignatureVersion=4
      &SignatureMethod=HmacSHA256
      &Timestamp=20150202T192317Z
      &X-Amz-Credential=<credential>
```

For more information about increasing the number of replicas using the API, see [IncreaseReplicaCount](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_IncreaseReplicaCount.html) in the *Amazon ElastiCache API Reference\.*