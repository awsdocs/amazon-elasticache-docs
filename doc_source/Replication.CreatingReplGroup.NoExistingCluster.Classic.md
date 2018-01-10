# Creating a Redis \(cluster mode disabled\) Cluster with Replicas from Scratch<a name="Replication.CreatingReplGroup.NoExistingCluster.Classic"></a>

You can create a Redis \(cluster mode disabled\) replication group from scratch using the ElastiCache console, the AWS CLI, or the ElastiCache API\. A Redis \(cluster mode disabled\) replication group always has one node group, a primary cluster, and up to 5 read replicas\. Redis \(cluster mode disabled\) replication groups do not support partitioning your data\.

Creating a Redis \(cluster mode disabled\) Cluster with Replicas from Scratch

## Creating a Redis \(cluster mode disabled\) Cluster with Replicas from Scratch \(Console\)<a name="Replication.CreatingReplGroup.NoExistingCluster.Classic.CON"></a>

To create a Redis \(cluster mode disabled\) cluster with replicas, see [Creating a Redis \(cluster mode disabled\) Cluster \(Console\)](Clusters.Create.CON.Redis.md)\. Specify at least one replica node\.

## Creating a Redis \(cluster mode disabled\) Cluster with Replicas from Scratch \(AWS CLI\)<a name="Replication.CreatingReplGroup.NoExistingCluster.Classic.CLI"></a>

The following procedure creates a Redis \(cluster mode disabled\) replication group using the AWS CLI\.

When you create a Redis \(cluster mode disabled\) replication group from scratch, you create the replication group and all its nodes with a single call to the AWS CLI `create-replication-group` command\. Include the following parameters\.

**\-\-replication\-group\-id**  
The name of the replication group you are creating\.  

**Redis \(cluster mode disabled\) Replication Group naming constraints**

+ Must contain from 1 to 20 alphanumeric characters or hyphens\.

+ Must begin with a letter\.

+ Cannot contain two consecutive hyphens\.

+ Cannot end with a hyphen\.

**\-\-replication\-group\-description**  
\(Optional\) Description of the replication group\.

**\-\-num\-cache\-clusters**  
The total number of clusters \(nodes\) you want created with this replication group, primary and read replicas combined\.  
If you enable Multi\-AZ \(`--automatic-failover-enabled`\), the value of `--num-cache-clusters` must be at least 2\.

**\-\-cache\-node\-type**  
The node type for each node in the replication group\.  
The following node types are supported by ElastiCache\. Generally speaking, the current generation types provide more memory and computational power at lower cost when compared to their equivalent previous generation counterparts\.  

+ General purpose:

  + Current generation: 

    **T2 node types:** `cache.t2.micro`, `cache.t2.small`, `cache.t2.medium`

    **M3 node types:** `cache.m3.medium`, `cache.m3.large`, `cache.m3.xlarge`, `cache.m3.2xlarge`

    **M4 node types:** `cache.m4.large`, `cache.m4.xlarge`, `cache.m4.2xlarge`, `cache.m4.4xlarge`, `cache.m4.10xlarge`

  + Previous generation: \(not recommended\)

    **T1 node types:** `cache.t1.micro`

    **M1 node types:** `cache.m1.small`, `cache.m1.medium`, `cache.m1.large`, `cache.m1.xlarge`

+ Compute optimized:

  + Previous generation: \(not recommended\)

    **C1 node types:** `cache.c1.xlarge`

+ Memory optimized:

  + Current generation: 

    **R3 node types:** `cache.r3.large`, `cache.r3.xlarge`, `cache.r3.2xlarge`, `cache.r3.4xlarge`, `cache.r3.8xlarge`

    **R4 node types:** `cache.r4.large`, `cache.r4.xlarge`, `cache.r4.2xlarge`, `cache.r4.4xlarge`, `cache.r4.8xlarge`, `cache.r4.16xlarge`

  + Previous generation: \(not recommended\)

    **M2 node types:** `cache.m2.xlarge`, `cache.m2.2xlarge`, `cache.m2.4xlarge`
**Additional node type info**  

+ All T2 instances are created in an Amazon Virtual Private Cloud \(Amazon VPC\)\.

+ Redis backup and restore is not supported for T2 instances\.

+ Redis append\-only files \(AOF\) are not supported for T1 or T2 instances\.

+ Redis Multi\-AZ with automatic failover is not supported on T1 instances\.

+ Redis Multi\-AZ with automatic failover is supported on T2 instances only when running Redis \(cluster mode enabled\) \- version 3\.2\.4 or later with the `default.redis3.2.cluster.on` parameter group or one derived from it\.

+ Redis configuration variables `appendonly` and `appendfsync` are not supported on Redis version 2\.8\.22 and later\.

**\-\-cache\-parameter\-group**  
Specify a parameter group that corresponds to your engine version\. If you are running Redis 3\.2\.4 or later, specify the `default.redis3.2` parameter group or a parameter group derived from `default.redis3.2` to create a Redis \(cluster mode disabled\) replication group\. For more information, see [Redis Specific Parameters](ParameterGroups.Redis.md)\.

**\-\-engine**  
redis

**\-\-engine\-version**  
To have the richest set of features, choose the latest engine version\.

The names of the nodes will be derived from the replication group name by postpending `-00`*\#* to the replication group name\. For example, using the replication group name `myReplGroup`, the name for the primary will be `myReplGroup-001` and the read replicas `myReplGroup-002` through `myReplGroup-006`\.

If you want to enable in\-transit or at\-rest encryption on this cluster, add these parameters:

+ `--transit-encryption-enabled`

  If you enable in\-transit encryption, the cluster must be created in a Amazon VPC and you must also include the parameter `--cache-subnet-group`\.

+ `--auth-token` with the customer specified string value for your AUTH token \(password\) needed to perform operations on this cluster\.

+ `--at-rest-encryption-enabled`

The following operation creates a Redis \(cluster mode disabled\) replication group `new-group` with three nodes, a primary and two replicas\.

For Linux, macOS, or Unix:

```
aws elasticache create-replication-group \
   --replication-group-id new-group \
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
   --replication-group-id new-group ^
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
        "ReplicationGroupId": "new-group",
        "SnapshotRetentionLimit": 0,
        "AutomaticFailover": "disabled",
        "SnapshotWindow": "01:30-02:30",
        "MemberClusters": [
            "new-group-001",
            "new-group-002",
            "new-group-003"
        ],
        "CacheNodeType": "cache.m4.large",
        "PendingModifiedValues": {}
    }
}
```

For additional information and parameters you might want to use, see the AWS CLI topic [create\-replication\-group](http://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)\.

## Creating a Redis \(cluster mode disabled\) Cluster with Replicas from Scratch \(ElastiCache API\)<a name="Replication.CreatingReplGroup.NoExistingCluster.Classic.API"></a>

The following procedure creates a Redis \(cluster mode disabled\) replication group using the ElastiCache API\.

When you create a Redis \(cluster mode disabled\) replication group from scratch, you create the replication group and all its nodes with a single call to the ElastiCache API `CreateReplicationGroup` operation\. Include the following parameters\.

**ReplicationGroupId**  
The name of the replication group you are creating\.  

**Redis \(cluster mode enabled\) Replication Group naming constraints**

+ Must contain from 1 to 20 alphanumeric characters or hyphens\.

+ Must begin with a letter\.

+ Cannot contain two consecutive hyphens\.

+ Cannot end with a hyphen\.

**ReplicationGroupDescription**  
Your description of the replication group\.

**NumCacheClusters**  
The total number of clusters \(nodes\) you want created with this replication group, primary and read replicas combined\.  
If you enable Multi\-AZ \(`AutomaticFailoverEnabled=true`\), the value of `NumCacheClusters` must be at least 2\.

**CacheNodeType**  
The node type for each node in the replication group\.  
The following node types are supported by ElastiCache\. Generally speaking, the current generation types provide more memory and computational power at lower cost when compared to their equivalent previous generation counterparts\.  

+ General purpose:

  + Current generation: 

    **T2 node types:** `cache.t2.micro`, `cache.t2.small`, `cache.t2.medium`

    **M3 node types:** `cache.m3.medium`, `cache.m3.large`, `cache.m3.xlarge`, `cache.m3.2xlarge`

    **M4 node types:** `cache.m4.large`, `cache.m4.xlarge`, `cache.m4.2xlarge`, `cache.m4.4xlarge`, `cache.m4.10xlarge`

  + Previous generation: \(not recommended\)

    **T1 node types:** `cache.t1.micro`

    **M1 node types:** `cache.m1.small`, `cache.m1.medium`, `cache.m1.large`, `cache.m1.xlarge`

+ Compute optimized:

  + Previous generation: \(not recommended\)

    **C1 node types:** `cache.c1.xlarge`

+ Memory optimized:

  + Current generation: 

    **R3 node types:** `cache.r3.large`, `cache.r3.xlarge`, `cache.r3.2xlarge`, `cache.r3.4xlarge`, `cache.r3.8xlarge`

    **R4 node types:** `cache.r4.large`, `cache.r4.xlarge`, `cache.r4.2xlarge`, `cache.r4.4xlarge`, `cache.r4.8xlarge`, `cache.r4.16xlarge`

  + Previous generation: \(not recommended\)

    **M2 node types:** `cache.m2.xlarge`, `cache.m2.2xlarge`, `cache.m2.4xlarge`
**Additional node type info**  

+ All T2 instances are created in an Amazon Virtual Private Cloud \(Amazon VPC\)\.

+ Redis backup and restore is not supported for T2 instances\.

+ Redis append\-only files \(AOF\) are not supported for T1 or T2 instances\.

+ Redis Multi\-AZ with automatic failover is not supported on T1 instances\.

+ Redis Multi\-AZ with automatic failover is supported on T2 instances only when running Redis \(cluster mode enabled\) \- version 3\.2\.4 or later with the `default.redis3.2.cluster.on` parameter group or one derived from it\.

+ Redis configuration variables `appendonly` and `appendfsync` are not supported on Redis version 2\.8\.22 and later\.

**CacheParameterGroup**  
Specify a parameter group that corresponds to your engine version\. If you are running Redis 3\.2\.4 or later, specify the `default.redis3.2` parameter group or a parameter group derived from `default.redis3.2` to create a Redis \(cluster mode disabled\) replication group\. For more information, see [Redis Specific Parameters](ParameterGroups.Redis.md)\.

**Engine**  
redis

**EngineVersion**  
3\.2\.4

The names of the nodes will be derived from the replication group name by postpending `-00`*\#* to the replication group name\. For example, using the replication group name `myReplGroup`, the name for the primary will be `myReplGroup-001` and the read replicas `myReplGroup-002` through `myReplGroup-006`\.

If you want to enable in\-transit or at\-rest encryption on this cluster, add these parameters:

+ `--transit-encryption-enabled`

  If you enable in\-transit encryption, the cluster must be created in a Amazon VPC and you must also include the parameter `--cache-subnet-group`\.

+ `--auth-token` with the customer specified string value for your AUTH token \(password\) needed to perform operations on this cluster\.

+ `--at-rest-encryption-enabled`

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

For additional information and parameters you might want to use, see the ElastiCache API topic [CreateReplicationGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)\.