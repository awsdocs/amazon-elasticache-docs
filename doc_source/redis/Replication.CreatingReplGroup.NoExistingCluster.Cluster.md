# Creating a Redis \(cluster mode enabled\) Replication Group from Scratch<a name="Replication.CreatingReplGroup.NoExistingCluster.Cluster"></a>

You can create a Redis \(cluster mode enabled\) cluster \(API/CLI: *replication group*\) using the ElastiCache console, the AWS CLI, or the ElastiCache API\. A Redis \(cluster mode enabled\) replication group has from 1 to 90 shards \(API/CLI: node groups\), a primary node in each shard, and up to 5 read replicas in each shard\. You can create a cluster with higher number of shards and lower number of replicas totaling up to 90 nodes per cluster\. This cluster configuration can range from 90 shards and 0 replicas to 15 shards and 5 replicas, which is the maximum number of replicas allowed\.

**Note**  
The node or shard limit can be increased to a maximum of 250 per cluster\. To request a limit increase, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) and select limit type "Nodes per cluster per instance type”\. 

**Topics**
+ [Using the ElastiCache Console](#Replication.CreatingReplGroup.NoExistingCluster.Cluster.CON)
+ [Creating a Redis \(cluster mode enabled\) Replication Group from Scratch \(AWS CLI\)](#Replication.CreatingReplGroup.NoExistingCluster.Cluster.CLI)
+ [Creating a Redis \(cluster mode enabled\) Replication Group from Scratch \(ElastiCache API\)](#Replication.CreatingReplGroup.NoExistingCluster.Cluster.API)

## Creating a Redis \(cluster mode enabled\) Cluster \(Console\)<a name="Replication.CreatingReplGroup.NoExistingCluster.Cluster.CON"></a>

To create a Redis \(cluster mode enabled\) cluster, see [Creating a Redis \(cluster mode enabled\) Cluster \(Console\)](Clusters.Create.CON.RedisCluster.md)\. Be sure to enable cluster mode, **Cluster Mode enabled \(Scale Out\)**, and specify at least two shards and one replica node in each\.

## Creating a Redis \(cluster mode enabled\) Replication Group from Scratch \(AWS CLI\)<a name="Replication.CreatingReplGroup.NoExistingCluster.Cluster.CLI"></a>

The following procedure creates a Redis \(cluster mode enabled\) replication group using the AWS CLI\.

When you create a Redis \(cluster mode enabled\) replication group from scratch, you create the replication group and all its nodes with a single call to the AWS CLI `create-replication-group` command\. Include the following parameters\.

**\-\-replication\-group\-id**  
The name of the replication group you are creating\.  

**Redis \(cluster mode enabled\) Replication Group naming constraints**
+ Must contain from 1 to 20 alphanumeric characters or hyphens\.
+ Must begin with a letter\.
+ Cannot contain two consecutive hyphens\.
+ Cannot end with a hyphen\.

**\-\-replication\-group\-description**  
Description of the replication group\.

**\-\-cache\-node\-type**  
The node type for each node in the replication group\.  
The following node types are supported by ElastiCache\. Generally speaking, the current generation types provide more memory and computational power at lower cost when compared to their equivalent previous generation counterparts\.  
+ General purpose:
  + Current generation: 

    **M5 node types:** `cache.m5.large`, `cache.m5.xlarge`, `cache.m5.2xlarge`, `cache.m5.4xlarge`, `cache.m5.12xlarge`, `cache.m5.24xlarge` 

    **M4 node types:** `cache.m4.large`, `cache.m4.xlarge`, `cache.m4.2xlarge`, `cache.m4.4xlarge`, `cache.m4.10xlarge`

    **T2 node types:** `cache.t2.micro`, `cache.t2.small`, `cache.t2.medium`
  + Previous generation: \(not recommended\)

    **T1 node types:** `cache.t1.micro`

    **M1 node types:** `cache.m1.small`, `cache.m1.medium`, `cache.m1.large`, `cache.m1.xlarge`

    **M3 node types:** `cache.m3.medium`, `cache.m3.large`, `cache.m3.xlarge`, `cache.m3.2xlarge`
+ Compute optimized:
  + Previous generation: \(not recommended\)

    **C1 node types:** `cache.c1.xlarge`
+ Memory optimized:
  + Current generation: 

    **R5 node types:** `cache.r5.large`, `cache.r5.xlarge`, `cache.r5.2xlarge`, `cache.r5.4xlarge`, `cache.r5.12xlarge`, `cache.r5.24xlarge`

    **R4 node types:** `cache.r4.large`, `cache.r4.xlarge`, `cache.r4.2xlarge`, `cache.r4.4xlarge`, `cache.r4.8xlarge`, `cache.r4.16xlarge`
  + Previous generation: \(not recommended\)

    **M2 node types:** `cache.m2.xlarge`, `cache.m2.2xlarge`, `cache.m2.4xlarge`

    **R3 node types:** `cache.r3.large`, `cache.r3.xlarge`, `cache.r3.2xlarge`, `cache.r3.4xlarge`, `cache.r3.8xlarge`
**Additional node type info**  
+ All current generation instance types are created in Amazon VPC by default\.
+ Redis append\-only files \(AOF\) are not supported for T1 or T2 instances\.
+ Redis Multi\-AZ with automatic failover is not supported on T1 instances\.
+ Redis configuration variables `appendonly` and `appendfsync` are not supported on Redis version 2\.8\.22 and later\.

**\-\-cache\-parameter\-group**  
Specify the `default.redis3.2.cluster.on` parameter group or a parameter group derived from `default.redis3.2.cluster.on` to create a Redis \(cluster mode enabled\) replication group\. For more information, see [Redis 3\.2\.4 Parameter Changes](ParameterGroups.Redis.md#ParameterGroups.Redis.3-2-4)\.

**\-\-engine**  
redis

**\-\-engine\-version**  
3\.2\.4

**\-\-num\-node\-groups**  
The number of node groups in this replication group\. Valid values are 1 to 90\.  
The node/shard limit can be increased to a maximum of 250 per cluster\. To request a limit increase, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) and select limit type "Nodes per cluster per instance type”\. 

**\-\-replicas\-per\-node\-group**  
The number of replica nodes in each node group\. Valid values are 0 to 5\.

If you want to enable in\-transit or at\-rest encryption on this replication group, add either or both of the `--transit-encryption-enabled` or `--at-rest-encryption-enabled` parameters and meet the following conditions\.
+ Your replication group must be running Redis version 3\.2\.6 or 4\.0\.10\.
+ The replication group must be created in an Amazon VPC\.
+ You must also include the parameter `--cache-subnet-group`\.
+ You must also include the parameter `--auth-token` with the customer specified string value for your AUTH token \(password\) needed to perform operations on this replication group\.

The following operation creates the Redis \(cluster mode enabled\) replication group `sample-repl-group` with three node groups/shards \(\-\-num\-node\-groups\), each with three nodes, a primary and two read replicas \(\-\-replicas\-per\-node\-group\)\.

For Linux, macOS, or Unix:

```
aws elasticache create-replication-group \
   --replication-group-id sample-repl-group \
   --replication-group-description "Demo cluster with replicas" \
   --num-node-groups 3 \
   --replicas-per-node-group 2 \
   --cache-node-type cache.m4.large \
   --cache-parameter-group default.redis3.2.cluster.on \
   --engine redis \
   --engine-version 3.2.4
   --security-group-ids SECURITY_GROUP_ID       
   --cache-subnet-group-name SUBNET_GROUP_NAME>
```

For Windows:

```
aws elasticache create-replication-group ^
   --replication-group-id sample-repl-group ^
   --replication-group-description "Demo cluster with replicas" ^
   --num-node-groups 3 ^
   --replicas-per-node-group 2 ^
   --cache-node-type cache.m4.large ^
   --cache-parameter-group default.redis3.2.cluster.on ^
   --engine redis ^
   --engine-version 3.2.4
   --security-group-ids SECURITY_GROUP_ID       
   --cache-subnet-group-name SUBNET_GROUP_NAME>
```

The preceding command generates the following output\.

```
{
    "ReplicationGroup": {
        "Status": "creating", 
        "Description": "Demo cluster with replicas", 
        "ReplicationGroupId": "sample-repl-group", 
        "SnapshotRetentionLimit": 0, 
        "AutomaticFailover": "enabled", 
        "SnapshotWindow": "05:30-06:30", 
        "MemberClusters": [
            "sample-repl-group-0001-001", 
            "sample-repl-group-0001-002", 
            "sample-repl-group-0001-003", 
            "sample-repl-group-0002-001", 
            "sample-repl-group-0002-002", 
            "sample-repl-group-0002-003", 
            "sample-repl-group-0003-001", 
            "sample-repl-group-0003-002", 
            "sample-repl-group-0003-003"
        ], 
        "PendingModifiedValues": {}
    }
}
```

When you create a Redis \(cluster mode enabled\) replication group from scratch, you are able to configure each shard in the cluster using the `--node-group-configuration` parameter as shown in the following example which configures two node groups \(Console: shards\)\. The first shard has two nodes, a primary and one read replica\. The second shard has three nodes, a primary and two read replicas\.

**\-\-node\-group\-configuration**  
The configuration for each node group\. The `--node-group-configuration` parameter consists of the following fields\.  
+ `PrimaryAvailabilityZone` – The Availability Zone where the primary node of this node group is located\. If this parameter is omitted, ElastiCache chooses the Availability Zone for the primary node\.

  **Example:** us\-west\-2a\.
+ `ReplicaAvailabilityZones` – A comma separated list of Availability Zones where the read replicas are located\. The number of Availability Zones in this list must match the value of `ReplicaCount`\. If this parameter is omitted, ElastiCache chooses the Availability Zones for the replica nodes\.

  **Example:** "us\-west\-2a,us\-west\-2b,us\-west\-2c"
+ `ReplicaCount` – The number of replica nodes in this node group\.
+ `Slots` – A string that specifies the keyspace for the node group\. The string is in the format `startKey-endKey`\. If this parameter is omitted, ElastiCache allocates keys equally among the node groups\.

  **Example:** "0\-4999"

   

The following operation creates the Redis \(cluster mode enabled\) replication group `new-group` with two node groups/shards \(`--num-node-groups`\)\. Unlike the preceding example, each node group is configured differently from the other node group \(`--node-group-configuration`\)\.

For Linux, macOS, or Unix:

```
aws elasticache create-replication-group \
  --replication-group-id new-group \
  --replication-group-description "Sharded replication group" \
  --engine redis \
  --engine-version 3.2.4 \
  --cache-parameter-group default.redis3.2.cluster.on \
  --snapshot-retention-limit 8 \
  --cache-node-type cache.m4.medium \
  --num-node-groups 2 \
  --node-group-configuration \
      "ReplicaCount=1,Slots=0-8999,PrimaryAvailabilityZone='us-east-1c',ReplicaAvailabilityZones='us-east-1b'" \
      "ReplicaCount=2,Slots=9000-16383,PrimaryAvailabilityZone='us-east-1a',ReplicaAvailabilityZones='us-east-1a','us-east-1c'"
```

For Windows:

```
aws elasticache create-replication-group ^
  --replication-group-id new-group ^
  --replication-group-description "Sharded replication group" ^
  --engine redis ^
  --engine-version 3.2.4 ^
  --cache-parameter-group default.redis3.2.cluster.on ^
  --snapshot-retention-limit 8 ^
  --cache-node-type cache.m4.medium ^
  --num-node-groups 2 ^
  --node-group-configuration \
      "ReplicaCount=1,Slots=0-8999,PrimaryAvailabilityZone='us-east-1c',ReplicaAvailabilityZones='us-east-1b'" \
      "ReplicaCount=2,Slots=9000-16383,PrimaryAvailabilityZone='us-east-1a',ReplicaAvailabilityZones='us-east-1a','us-east-1c'"
```

The preceding operation generates the following output\.

```
{
    "ReplicationGroup": {
        "Status": "creating", 
        "Description": "Sharded replication group", 
        "ReplicationGroupId": "rc-rg", 
        "SnapshotRetentionLimit": 8, 
        "AutomaticFailover": "enabled", 
        "SnapshotWindow": "10:00-11:00", 
        "MemberClusters": [
            "rc-rg-0001-001", 
            "rc-rg-0001-002", 
            "rc-rg-0002-001", 
            "rc-rg-0002-002", 
            "rc-rg-0002-003"
        ], 
        "PendingModifiedValues": {}
    }
}
```

For additional information and parameters you might want to use, see the AWS CLI topic [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)\.

## Creating a Redis \(cluster mode enabled\) Replication Group from Scratch \(ElastiCache API\)<a name="Replication.CreatingReplGroup.NoExistingCluster.Cluster.API"></a>

The following procedure creates a Redis \(cluster mode enabled\) replication group using the ElastiCache API\.

When you create a Redis \(cluster mode enabled\) replication group from scratch, you create the replication group and all its nodes with a single call to the ElastiCache API `CreateReplicationGroup` operation\. Include the following parameters\.

**ReplicationGroupId**  
The name of the replication group you are creating\.  

**Redis \(cluster mode enabled\) Replication Group naming constraints**
+ Must contain from 1 to 20 alphanumeric characters or hyphens\.
+ Must begin with a letter\.
+ Cannot contain two consecutive hyphens\.
+ Cannot end with a hyphen\.

**ReplicationGroupDescription**  
Description of the replication group\.

**NumNodeGroups**  
The number of node groups you want created with this replication group\. Valid values are 1 to 90\.

**ReplicasPerNodeGroup**  
The number of replica nodes in each node group\. Valid values are 1 to 5\.

**NodeGroupConfiguration**  
The configuration for each node group\. The `NodeGroupConfiguration` parameter consists of the following fields\.  
+ `PrimaryAvailabilityZone` – The Availability Zone where the primary node of this node group is located\. If this parameter is omitted, ElastiCache chooses the Availability Zone for the primary node\.

  **Example:** us\-west\-2a\.
+ `ReplicaAvailabilityZones` – A list of Availability Zones where the read replicas are located\. The number of Availability Zones in this list must match the value of `ReplicaCount`\. If this parameter is omitted, ElastiCache chooses the Availability Zones for the replica nodes\.
+ `ReplicaCount` – The number of replica nodes in this node group\.
+ `Slots` – A string that specifies the keyspace for the node group\. The string is in the format `startKey-endKey`\. If this parameter is omitted, ElastiCache allocates keys equally among the node groups\.

  **Example:** "0\-4999"

   

**CacheNodeType**  
The node type for each node in the replication group\.  
The following node types are supported by ElastiCache\. Generally speaking, the current generation types provide more memory and computational power at lower cost when compared to their equivalent previous generation counterparts\.  
+ General purpose:
  + Current generation: 

    **M5 node types:** `cache.m5.large`, `cache.m5.xlarge`, `cache.m5.2xlarge`, `cache.m5.4xlarge`, `cache.m5.12xlarge`, `cache.m5.24xlarge` 

    **M4 node types:** `cache.m4.large`, `cache.m4.xlarge`, `cache.m4.2xlarge`, `cache.m4.4xlarge`, `cache.m4.10xlarge`

    **T2 node types:** `cache.t2.micro`, `cache.t2.small`, `cache.t2.medium`
  + Previous generation: \(not recommended\)

    **T1 node types:** `cache.t1.micro`

    **M1 node types:** `cache.m1.small`, `cache.m1.medium`, `cache.m1.large`, `cache.m1.xlarge`

    **M3 node types:** `cache.m3.medium`, `cache.m3.large`, `cache.m3.xlarge`, `cache.m3.2xlarge`
+ Compute optimized:
  + Previous generation: \(not recommended\)

    **C1 node types:** `cache.c1.xlarge`
+ Memory optimized:
  + Current generation: 

    **R5 node types:** `cache.r5.large`, `cache.r5.xlarge`, `cache.r5.2xlarge`, `cache.r5.4xlarge`, `cache.r5.12xlarge`, `cache.r5.24xlarge`

    **R4 node types:** `cache.r4.large`, `cache.r4.xlarge`, `cache.r4.2xlarge`, `cache.r4.4xlarge`, `cache.r4.8xlarge`, `cache.r4.16xlarge`
  + Previous generation: \(not recommended\)

    **M2 node types:** `cache.m2.xlarge`, `cache.m2.2xlarge`, `cache.m2.4xlarge`

    **R3 node types:** `cache.r3.large`, `cache.r3.xlarge`, `cache.r3.2xlarge`, `cache.r3.4xlarge`, `cache.r3.8xlarge`
**Additional node type info**  
+ All current generation instance types are created in Amazon VPC by default\.
+ Redis append\-only files \(AOF\) are not supported for T1 or T2 instances\.
+ Redis Multi\-AZ with automatic failover is not supported on T1 instances\.
+ Redis configuration variables `appendonly` and `appendfsync` are not supported on Redis version 2\.8\.22 and later\.

**CacheParameterGroup**  
Specify the `default.redis3.2.cluster.on` parameter group or a parameter group derived from `default.redis3.2.cluster.on` to create a Redis \(cluster mode enabled\) replication group\. For more information, see [Redis 3\.2\.4 Parameter Changes](ParameterGroups.Redis.md#ParameterGroups.Redis.3-2-4)\.

**Engine**  
redis

**EngineVersion**  
3\.2\.4

If you want to enable in\-transit or at\-rest encryption on this replication group, add either or both of the `TrasitEncryptionEnabled=true` or `AtRestEncryptionEnabled=true` parameters and meet the following conditions\.
+ Your replication group must be running Redis version 3\.2\.6 or 4\.0\.10\.
+ The replication group must be created in an Amazon VPC\.
+ You must also include the parameter `CacheSubnetGroup`\.
+ You must also include the parameter `AuthToken` with the customer specified string value for your AUTH token \(password\) needed to perform operations on this replication group\.

Line breaks are added for ease of reading\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=CreateReplicationGroup 
   &CacheNodeType=cache.m4.large
   &CacheParemeterGroup=default.redis3.2.cluster.on
   &Engine=redis
   &EngineVersion=3.2.4
   &NumNodeGroups=3
   &ReplicasPerNodeGroup=2
   &ReplicationGroupDescription=test%20group
   &ReplicationGroupId=myReplGroup
   &Version=2015-02-02
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```

For additional information and parameters you might want to use, see the ElastiCache API topic [CreateReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)\.