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

**\-\-data\-tiering\-enabled**  
Set this parameter if you are using an r6gd node type\. If you don't want data tiering, set `--no-data-tiering-enabled`\. For more information, see [Data tiering](data-tiering.md)\.

**\-\-cache\-parameter\-group**  
Specify a parameter group that corresponds to your engine version\. If you are running Redis 3\.2\.4 or later, specify the `default.redis3.2` parameter group or a parameter group derived from `default.redis3.2` to create a Redis \(cluster mode disabled\) replication group\. For more information, see [Redis\-specific parameters](ParameterGroups.Redis.md)\.

**\-\-network\-type**  
Either `ipv4`, `ipv6` or `dual-stack`\. If you choose dual\-stack, you must set the `--IpDiscovery` parameter to either `ipv4` or `ipv6`\.

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
   --engine redis
```

For Windows:

```
aws elasticache create-replication-group ^
   --replication-group-id sample-repl-group ^
   --replication-group-description "Demo cluster with replicas" ^
   --num-cache-clusters 3 ^
   --cache-node-type cache.m4.large ^  
   --engine redis
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
        "DataTiering": "disabled",
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

**\-\-data\-tiering\-enabled**  
Set this parameter if you are using an r6gd node type\. If you don't want data tiering, set `--no-data-tiering-enabled`\. For more information, see [Data tiering](data-tiering.md)\.

**CacheParameterGroup**  
Specify a parameter group that corresponds to your engine version\. If you are running Redis 3\.2\.4 or later, specify the `default.redis3.2` parameter group or a parameter group derived from `default.redis3.2` to create a Redis \(cluster mode disabled\) replication group\. For more information, see [Redis\-specific parameters](ParameterGroups.Redis.md)\.

**\-\-network\-type**  
Either `ipv4`, `ipv` or `dual-stack`\. If you choose dual\-stack, you must set the `--IpDiscovery` parameter to either `ipv4` or `ipv6`\.

**Engine**  
redis

**EngineVersion**  
6\.0

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
   &CacheParameterGroup=default.redis6.x
   &Engine=redis
   &EngineVersion=6.0
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