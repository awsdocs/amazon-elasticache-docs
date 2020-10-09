# Creating a Redis \(Cluster Mode Disabled\) Replication Group from Scratch<a name="Replication.CreatingReplGroup.NoExistingCluster.Classic"></a>

You can create a Redis \(cluster mode disabled\) replication group from scratch using the ElastiCache console, the AWS CLI, or the ElastiCache API\. A Redis \(cluster mode disabled\) replication group always has one node group, a primary cluster, and up to five read replicas\. Redis \(cluster mode disabled\) replication groups don't support partitioning your data\.

**Note**  
The node/shard limit can be increased to a maximum of 250 per cluster\. To request a limit increase, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) and include the instance type in the request\.

To create a Redis \(cluster mode disabled\) replication group from scratch, take one of the following approaches:
+ To do this using the console, see [Creating a Redis \(Cluster Mode Enabled\) Cluster \(Console\)](Clusters.Create.CON.RedisCluster.md)\.
+ To do this using the AWS CLI, see [Creating a Redis \(Cluster Mode Enabled\) Cluster \(AWS CLI\)](Clusters.Create.CLI.md#Clusters.Create.CLI.RedisCluster)\.
+ To do this using the ElastiCache API, see [Creating a Cache Cluster in Redis \(Cluster Mode Enabled\) \(ElastiCache API\)](Clusters.Create.API.md#Clusters.Create.API.RedisCluster)\.

## Creating a Redis \(Cluster Mode Disabled\) Replication Group from Scratch \(AWS CLI\)<a name="Replication.CreatingReplGroup.NoExistingCluster.Classic.CLI"></a>

The following procedure creates a Redis \(cluster mode disabled\) replication group using the AWS CLI\.

When you create a Redis \(cluster mode disabled\) replication group from scratch, you create the replication group and all its nodes with a single call to the AWS CLI `create-replication-group` command\. Include the following parameters\.

**\-\-replication\-group\-id**  
The name of the replication group you are creating\.  
Redis \(cluster mode disabled\) replication group naming constraints are as follows:  
+ Must contain 1–40 alphanumeric characters or hyphens\.
+ Must begin with a letter\.
+ Can't contain two consecutive hyphens\.
+ Can't end with a hyphen\.

**\-\-replication\-group\-description**  
Description of the replication group\.

**\-\-num\-cache\-clusters**  
The number of nodes you want created with this replication group, primary and read replicas combined\.  
If you enable Multi\-AZ \(`--automatic-failover-enabled`\), the value of `--num-cache-clusters` must be at least 2\.

**\-\-cache\-node\-type**  
The node type for each node in the replication group\.  
The following node types are supported by ElastiCache\. Generally speaking, the current generation types provide more memory and computational power at lower cost when compared to their equivalent previous generation counterparts\.  
+ General purpose:
  + Current generation: 

    **M5 node types:** `cache.m5.large`, `cache.m5.xlarge`, `cache.m5.2xlarge`, `cache.m5.4xlarge`, `cache.m5.12xlarge`, `cache.m5.24xlarge` 

    **M4 node types:** `cache.m4.large`, `cache.m4.xlarge`, `cache.m4.2xlarge`, `cache.m4.4xlarge`, `cache.m4.10xlarge`

    **T3 node types:** `cache.t3.micro`, `cache.t3.small`, `cache.t3.medium`

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
+ Redis Multi\-AZ is not supported on T1 instances\.
+ Redis configuration variables `appendonly` and `appendfsync` are not supported on Redis version 2\.8\.22 and later\.

**\-\-cache\-parameter\-group**  
Specify a parameter group that corresponds to your engine version\. If you are running Redis 3\.2\.4 or later, specify the `default.redis3.2` parameter group or a parameter group derived from `default.redis3.2` to create a Redis \(cluster mode disabled\) replication group\. For more information, see [Redis Specific Parameters](ParameterGroups.Redis.md)\.

**\-\-engine**  
redis

**\-\-engine\-version**  
To have the richest set of features, choose the latest engine version\.

The names of the nodes will be derived from the replication group name by postpending `-00`*\#* to the replication group name\. For example, using the replication group name `myReplGroup`, the name for the primary will be `myReplGroup-001` and the read replicas `myReplGroup-002` through `myReplGroup-006`\.

If you want to enable in\-transit or at\-rest encryption on this replication group, add either or both of the `--transit-encryption-enabled` or `--at-rest-encryption-enabled` parameters and meet the following conditions\.
+ Your replication group must be running Redis version 3\.2\.6 or 4\.0\.10\.
+ The replication group must be created in an Amazon VPC\.
+ You must also include the parameter `--cache-subnet-group`\.
+ You must also include the parameter `--auth-token` with the customer specified string value for your AUTH token \(password\) needed to perform operations on this replication group\.

The following operation creates a Redis \(cluster mode disabled\) replication group `sample-repl-group` with three nodes, a primary and two replicas\.

For Linux, macOS, or Unix:

```
aws elasticache create-replication-group \
   --replication-group-id sample-repl-group \
   --replication-group-description "Demo cluster with replicas" \
   --num-cache-clusters 3 \
   --cache-node-type cache.m4.large \
   --cache-parameter-group default.redis3.2 \
   --engine redis \
   --engine-version 3.2.4
```

For Windows:

```
aws elasticache create-replication-group ^
   --replication-group-id sample-repl-group ^
   --replication-group-description "Demo cluster with replicas" ^
   --num-cache-clusters 3 ^
   --cache-node-type cache.m4.large ^
   --cache-parameter-group default.redis3.2 ^
   --engine redis ^
   --engine-version 3.2.4
```

Output from the this command is something like this\.

```
{
    "ReplicationGroup": {
        "Status": "creating",
        "Description": "Demo cluster with replicas",
        "ClusterEnabled": false,
        "ReplicationGroupId": "sample-repl-group",
        "SnapshotRetentionLimit": 0,
        "AutomaticFailover": "disabled",
        "SnapshotWindow": "01:30-02:30",
        "MemberClusters": [
            "sample-repl-group-001",
            "sample-repl-group-002",
            "sample-repl-group-003"
        ],
        "CacheNodeType": "cache.m4.large",
        "PendingModifiedValues": {}
    }
}
```

For additional information and parameters you might want to use, see the AWS CLI topic [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)\.

## Creating a Redis \(cluster mode disabled\) Replication Group from Scratch \(ElastiCache API\)<a name="Replication.CreatingReplGroup.NoExistingCluster.Classic.API"></a>

The following procedure creates a Redis \(cluster mode disabled\) replication group using the ElastiCache API\.

When you create a Redis \(cluster mode disabled\) replication group from scratch, you create the replication group and all its nodes with a single call to the ElastiCache API `CreateReplicationGroup` operation\. Include the following parameters\.

**ReplicationGroupId**  
The name of the replication group you are creating\.  
Redis \(cluster mode enabled\) replication group naming constraints are as follows:  
+ Must contain 1–40 alphanumeric characters or hyphens\.
+ Must begin with a letter\.
+ Can't contain two consecutive hyphens\.
+ Can't end with a hyphen\.

**ReplicationGroupDescription**  
Your description of the replication group\.

**NumCacheClusters**  
The total number of nodes you want created with this replication group, primary and read replicas combined\.  
If you enable Multi\-AZ \(`AutomaticFailoverEnabled=true`\), the value of `NumCacheClusters` must be at least 2\.

**CacheNodeType**  
The node type for each node in the replication group\.  
The following node types are supported by ElastiCache\. Generally speaking, the current generation types provide more memory and computational power at lower cost when compared to their equivalent previous generation counterparts\.  
+ General purpose:
  + Current generation: 

    **M5 node types:** `cache.m5.large`, `cache.m5.xlarge`, `cache.m5.2xlarge`, `cache.m5.4xlarge`, `cache.m5.12xlarge`, `cache.m5.24xlarge` 

    **M4 node types:** `cache.m4.large`, `cache.m4.xlarge`, `cache.m4.2xlarge`, `cache.m4.4xlarge`, `cache.m4.10xlarge`

    **T3 node types:** `cache.t3.micro`, `cache.t3.small`, `cache.t3.medium`

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
+ Redis Multi\-AZ is not supported on T1 instances\.
+ Redis configuration variables `appendonly` and `appendfsync` are not supported on Redis version 2\.8\.22 and later\.

**CacheParameterGroup**  
Specify a parameter group that corresponds to your engine version\. If you are running Redis 3\.2\.4 or later, specify the `default.redis3.2` parameter group or a parameter group derived from `default.redis3.2` to create a Redis \(cluster mode disabled\) replication group\. For more information, see [Redis Specific Parameters](ParameterGroups.Redis.md)\.

**Engine**  
redis

**EngineVersion**  
3\.2\.4

The names of the nodes will be derived from the replication group name by postpending `-00`*\#* to the replication group name\. For example, using the replication group name `myReplGroup`, the name for the primary will be `myReplGroup-001` and the read replicas `myReplGroup-002` through `myReplGroup-006`\.

If you want to enable in\-transit or at\-rest encryption on this replication group, add either or both of the `TransitEncryptionEnabled=true` or `AtRestEncryptionEnabled=true` parameters and meet the following conditions\.
+ Your replication group must be running Redis version 3\.2\.6 or 4\.0\.10\.
+ The replication group must be created in an Amazon VPC\.
+ You must also include the parameter `CacheSubnetGroup`\.
+ You must also include the parameter `AuthToken` with the customer specified string value for your AUTH token \(password\) needed to perform operations on this replication group\.

The following operation creates the Redis \(cluster mode disabled\) replication group `myReplGroup` with three nodes, a primary and two replicas\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=CreateReplicationGroup 
   &CacheNodeType=cache.m4.large
   &CacheParameterGroup=default.redis3.2
   &Engine=redis
   &EngineVersion=3.2.4
   &NumCacheClusters=3
   &ReplicationGroupDescription=test%20group
   &ReplicationGroupId=myReplGroup
   &Version=2015-02-02
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```

For additional information and parameters you might want to use, see the ElastiCache API topic [CreateReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)\.