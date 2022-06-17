# Creating a Redis \(Cluster Mode Disabled\) replication group from scratch<a name="Replication.CreatingReplGroup.NoExistingCluster.Classic"></a>

You can create a Redis \(cluster mode disabled\) replication group from scratch using the ElastiCache console, the AWS CLI, or the ElastiCache API\. A Redis \(cluster mode disabled\) replication group always has one node group, a primary cluster, and up to five read replicas\. Redis \(cluster mode disabled\) replication groups don't support partitioning your data\.

**Note**  
The node/shard limit can be increased to a maximum of 500 per cluster\. To request a limit increase, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) and include the instance type in the request\.

To create a Redis \(cluster mode disabled\) replication group from scratch, take one of the following approaches:

## Creating a Redis \(Cluster Mode Disabled\) replication group from scratch \(AWS CLI\)<a name="Replication.CreatingReplGroup.NoExistingCluster.Classic.CLI"></a>

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
ElastiCache supports the following node types\. Generally speaking, the current generation types provide more memory and computational power at lower cost when compared to their equivalent previous generation counterparts\.  
For more information on performance details for each node type, see [Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)\.  
+ General purpose:
  + Current generation: 

    **M6g node types** \(available only for Redis engine version 5\.0\.6 onward\)\.

     `cache.m6g.large`, `cache.m6g.xlarge`, `cache.m6g.2xlarge`, `cache.m6g.4xlarge`, `cache.m6g.8xlarge`, `cache.m6g.12xlarge`, `cache.m6g.16xlarge` 
**Note**  
For region availability, see [Supported node types by AWS Region](CacheNodes.SupportedTypes.md#CacheNodes.SupportedTypesByRegion)\.

    **M5 node types:** `cache.m5.large`, `cache.m5.xlarge`, `cache.m5.2xlarge`, `cache.m5.4xlarge`, `cache.m5.12xlarge`, `cache.m5.24xlarge` 

    **M4 node types:** `cache.m4.large`, `cache.m4.xlarge`, `cache.m4.2xlarge`, `cache.m4.4xlarge`, `cache.m4.10xlarge`

    **T4g node types** \(available only for Redis engine version 5\.0\.6 onward\)\.

     `cache.t4g.micro`, `cache.t4g.small`, `cache.t4g.medium` 

    **T3 node types:** `cache.t3.micro`, `cache.t3.small`, `cache.t3.medium`

    **T2 node types:** `cache.t2.micro`, `cache.t2.small`, `cache.t2.medium`
  + Previous generation: \(not recommended\. Existing clusters are still supported but creation of new clusters is not supported for these types\.\)

    **T1 node types:** `cache.t1.micro`

    **M1 node types:** `cache.m1.small`, `cache.m1.medium`, `cache.m1.large`, `cache.m1.xlarge`

    **M3 node types:** `cache.m3.medium`, `cache.m3.large`, `cache.m3.xlarge`, `cache.m3.2xlarge`
+ Compute optimized:
  + Previous generation: \(not recommended\)

    **C1 node types:** `cache.c1.xlarge`
+ Memory optimized with data tiering:
  + Current generation: 

    **R6gd node types** \(available only for Redis engine version 6\.2 onward\)\. For more information, see [Data tiering](data-tiering.md)\.

     `cache.r6gd.xlarge`, `cache.r6gd.2xlarge`, `cache.r6gd.4xlarge`, `cache.r6gd.8xlarge`, `cache.r6gd.12xlarge`, `cache.r6gd.16xlarge` 
+ Memory optimized:
  + Current generation: 

    \(*R6g node types* are available only for Redis engine version 5\.0\.6 onward\.\)

    **R6g node types:** `cache.r6g.large`, `cache.r6g.xlarge`, `cache.r6g.2xlarge`, `cache.r6g.4xlarge`, `cache.r6g.8xlarge`, `cache.r6g.12xlarge`, `cache.r6g.16xlarge` `cache.r6g.24xlarge` 
**Note**  
For region availability, see [Supported node types by AWS Region](CacheNodes.SupportedTypes.md#CacheNodes.SupportedTypesByRegion)\.

    **R5 node types:** `cache.r5.large`, `cache.r5.xlarge`, `cache.r5.2xlarge`, `cache.r5.4xlarge`, `cache.r5.12xlarge`, `cache.r5.24xlarge`

    **R4 node types:** `cache.r4.large`, `cache.r4.xlarge`, `cache.r4.2xlarge`, `cache.r4.4xlarge`, `cache.r4.8xlarge`, `cache.r4.16xlarge`
  + Previous generation: \(not recommended\)

    **M2 node types:** `cache.m2.xlarge`, `cache.m2.2xlarge`, `cache.m2.4xlarge`

    **R3 node types:** `cache.r3.large`, `cache.r3.xlarge`, `cache.r3.2xlarge`, `cache.r3.4xlarge`, `cache.r3.8xlarge`

**\-\-data\-tiering\-enabled**  
Set this parameter if you are using an r6gd node type\. If you don't want data tiering, set `--no-data-tiering-enabled`\. For more information, see [Data tiering](data-tiering.md)\.

**\-\-cache\-parameter\-group**  
Specify a parameter group that corresponds to your engine version\. If you are running Redis 3\.2\.4 or later, specify the `default.redis3.2` parameter group or a parameter group derived from `default.redis3.2` to create a Redis \(cluster mode disabled\) replication group\. For more information, see [Redis\-specific parameters](ParameterGroups.Redis.md)\.

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
        "DataTiering": "disabled"
        "PendingModifiedValues": {}
    }
}
```

For additional information and parameters you might want to use, see the AWS CLI topic [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)\.

## Creating a Redis \(cluster mode disabled\) replication group from scratch \(ElastiCache API\)<a name="Replication.CreatingReplGroup.NoExistingCluster.Classic.API"></a>

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
ElastiCache supports the following node types\. Generally speaking, the current generation types provide more memory and computational power at lower cost when compared to their equivalent previous generation counterparts\.  
For more information on performance details for each node type, see [Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)\.  
+ General purpose:
  + Current generation: 

    **M6g node types** \(available only for Redis engine version 5\.0\.6 onward\)\.

     `cache.m6g.large`, `cache.m6g.xlarge`, `cache.m6g.2xlarge`, `cache.m6g.4xlarge`, `cache.m6g.8xlarge`, `cache.m6g.12xlarge`, `cache.m6g.16xlarge` 
**Note**  
For region availability, see [Supported node types by AWS Region](CacheNodes.SupportedTypes.md#CacheNodes.SupportedTypesByRegion)\.

    **M5 node types:** `cache.m5.large`, `cache.m5.xlarge`, `cache.m5.2xlarge`, `cache.m5.4xlarge`, `cache.m5.12xlarge`, `cache.m5.24xlarge` 

    **M4 node types:** `cache.m4.large`, `cache.m4.xlarge`, `cache.m4.2xlarge`, `cache.m4.4xlarge`, `cache.m4.10xlarge`

    **T4g node types** \(available only for Redis engine version 5\.0\.6 onward\)\.

     `cache.t4g.micro`, `cache.t4g.small`, `cache.t4g.medium` 

    **T3 node types:** `cache.t3.micro`, `cache.t3.small`, `cache.t3.medium`

    **T2 node types:** `cache.t2.micro`, `cache.t2.small`, `cache.t2.medium`
  + Previous generation: \(not recommended\. Existing clusters are still supported but creation of new clusters is not supported for these types\.\)

    **T1 node types:** `cache.t1.micro`

    **M1 node types:** `cache.m1.small`, `cache.m1.medium`, `cache.m1.large`, `cache.m1.xlarge`

    **M3 node types:** `cache.m3.medium`, `cache.m3.large`, `cache.m3.xlarge`, `cache.m3.2xlarge`
+ Compute optimized:
  + Previous generation: \(not recommended\)

    **C1 node types:** `cache.c1.xlarge`
+ Memory optimized with data tiering:
  + Current generation: 

    **R6gd node types** \(available only for Redis engine version 6\.2 onward\)\. For more information, see [Data tiering](data-tiering.md)\.

     `cache.r6gd.xlarge`, `cache.r6gd.2xlarge`, `cache.r6gd.4xlarge`, `cache.r6gd.8xlarge`, `cache.r6gd.12xlarge`, `cache.r6gd.16xlarge` 
+ Memory optimized:
  + Current generation: 

    \(*R6g node types* are available only for Redis engine version 5\.0\.6 onward\.\)

    **R6g node types:** `cache.r6g.large`, `cache.r6g.xlarge`, `cache.r6g.2xlarge`, `cache.r6g.4xlarge`, `cache.r6g.8xlarge`, `cache.r6g.12xlarge`, `cache.r6g.16xlarge` `cache.r6g.24xlarge` 
**Note**  
For region availability, see [Supported node types by AWS Region](CacheNodes.SupportedTypes.md#CacheNodes.SupportedTypesByRegion)\.

    **R5 node types:** `cache.r5.large`, `cache.r5.xlarge`, `cache.r5.2xlarge`, `cache.r5.4xlarge`, `cache.r5.12xlarge`, `cache.r5.24xlarge`

    **R4 node types:** `cache.r4.large`, `cache.r4.xlarge`, `cache.r4.2xlarge`, `cache.r4.4xlarge`, `cache.r4.8xlarge`, `cache.r4.16xlarge`
  + Previous generation: \(not recommended\)

    **M2 node types:** `cache.m2.xlarge`, `cache.m2.2xlarge`, `cache.m2.4xlarge`

    **R3 node types:** `cache.r3.large`, `cache.r3.xlarge`, `cache.r3.2xlarge`, `cache.r3.4xlarge`, `cache.r3.8xlarge`

**\-\-data\-tiering\-enabled**  
Set this parameter if you are using an r6gd node type\. If you don't want data tiering, set `--no-data-tiering-enabled`\. For more information, see [Data tiering](data-tiering.md)\.

**CacheParameterGroup**  
Specify a parameter group that corresponds to your engine version\. If you are running Redis 3\.2\.4 or later, specify the `default.redis3.2` parameter group or a parameter group derived from `default.redis3.2` to create a Redis \(cluster mode disabled\) replication group\. For more information, see [Redis\-specific parameters](ParameterGroups.Redis.md)\.

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