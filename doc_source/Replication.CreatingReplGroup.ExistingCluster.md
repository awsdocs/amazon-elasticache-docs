# Creating a Cluster with Replicas Using an Available Redis \(cluster mode disabled\) Cluster<a name="Replication.CreatingReplGroup.ExistingCluster"></a>

An available cluster is an existing single\-node Redis cluster\. Currently, Redis \(cluster mode enabled\) does not support creating a cluster with replicas using an available single\-node cluster\. If you want to create a Redis \(cluster mode enabled\) cluster, see [Creating a Redis \(cluster mode enabled\) Cluster \(Console\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.CON)\.

The following procedure can only be used if you have a Redis \(cluster mode disabled\) single\-node cluster\. This cluster becomes the primary in the new cluster\. If you do not have a Redis \(cluster mode disabled\) cluster you can use as the new cluster's primary, see [Creating a Redis Cluster with Replicas from Scratch](Replication.CreatingReplGroup.NoExistingCluster.md)\.

## Creating a Cluster with Replicas Using an Available Redis Cluster \(Console\)<a name="Replication.CreatingReplGroup.ExistingCluster.CON"></a>

See the topic [Adding Nodes to a Cluster \(Console\)](Clusters.AddNode.md#Clusters.AddNode.CON)\.

## Creating a Replication Group Using an Available Redis Cache Cluster \(AWS CLI\)<a name="Replication.CreatingReplGroup.ExistingCluster.CLI"></a>

There are two steps to creating a replication group with read replicas when using an available Redis Cache Cluster for the primary when using the AWS CLI\.

When using the AWS CLI you create a replication group specifying the available stand\-alone node as the cluster's primary node, `--primary-cluster-id` and the number of nodes you want in the cluster using the CLI command, `create-replication-group`\. Include the following parameters\.

**\-\-replication\-group\-id**  
The name of the replication group you are creating\. The value of this parameter is used as the basis for the names of the added nodes with a sequential 3\-digit number added to the end of the `--replication-group-id`\. For example, `new-group-001`\.  

**Redis \(cluster mode disabled\) Replication Group naming constraints**

+ Must contain from 1 to 20 alphanumeric characters or hyphens\.

+ Must begin with a letter\.

+ Cannot contain two consecutive hyphens\.

+ Cannot end with a hyphen\.

**\-\-replication\-group\-description**  
Description of the replication group\.

**\-\-num\-cache\-clusters**  
The total number of nodes you want in this clusters, including the primary node\. This parameter has a maximum value of 6\.

**\-\-primary\-cluster\-id**  
The name of the available Redis \(cluster mode disabled\) cluster that you want to be the primary node in this replication group\.

If you want to enable in\-transit or at\-rest encryption on this cluster, add these parameters:

+ `--transit-encryption-enabled`

  If you enable in\-transit encryption, the cluster must be created in a Amazon VPC and you must also include the parameter `--cache-subnet-group`\.

+ `--auth-token` with the customer specified string value for your AUTH token \(password\) needed to perform operations on this cluster\.

+ `--at-rest-encryption-enabled`

The following command creates the replication group `new-group` using the available Redis \(cluster mode disabled\) cluster `redis01` as the replication group's primary node\. It creates 2 new nodes which are read replicas\. The settings of `redis01` \(that is, parameter group, security group, node type, engine version, etc\.\) will be applied to all nodes in the replication group\.

For Linux, macOS, or Unix:

```
aws elasticache create-replication-group \
   --replication-group-id new-group \
   --replication-group-description "demo cluster with replicas" \
   --num-cache-clusters 3 \
   --primary-cluster-id redis01
```

For Windows:

```
aws elasticache create-replication-group ^
   --replication-group-id new-group ^
   --replication-group-description "demo cluster with replicas" ^
   --num-cache-clusters 3 ^
   --primary-cluster-id redis01
```

For additional information and parameters you might want to use, see the AWS CLI topic [create\-replication\-group](http://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)\.

**Next, add read replicas to the replication group**  
After the replication group is created, add one to five read replicas to it using the `create-cache-cluster` command, being sure to include the following parameters\. 

**\-\-cache\-cluster\-id**  
The name of the cluster you are adding to the replication group\.  

**Cluster naming constraints**

+ Must contain from 1 to 20 alphanumeric characters or hyphens\.

+ Must begin with a letter\.

+ Cannot contain two consecutive hyphens\.

+ Cannot end with a hyphen\.

**\-\-replication\-group\-id**  
The name of the replication group to which you are adding this cache cluster\.

Repeat this command for each read replica you want to add to the replication group, changing only the value of the `--cache-cluster-id` parameter\.

**Note**  
Remember, a replication group cannot have more than five read replicas\. Attempting to add a read replica to a replication group that already has five read replicas causes the operation to fail\.

The following code adds the read replica `my-replica01` to the replication group `my-repl-group`\. The settings of the primary cluster–parameter group, security group, node type, etc\.–will be applied to nodes as they are added to the replication group\.

For Linux, macOS, or Unix:

```
aws elasticache create-cache-cluster \
   --cache-cluster-id my-replica01 \
   --replication-group-id my-repl-group
```

For Windows:

```
aws elasticache create-cache-cluster ^
   --cache-cluster-id my-replica01 ^
   --replication-group-id my-repl-group
```

Output from this command will look something like this\.

```
{
    "ReplicationGroup": {
        "Status": "creating",
        "Description": "demo cluster with replicas",
        "ClusterEnabled": false,
        "ReplicationGroupId": "new-group",
        "SnapshotRetentionLimit": 1,
        "AutomaticFailover": "disabled",
        "SnapshotWindow": "00:00-01:00",
        "SnapshottingClusterId": "redis01",
        "MemberClusters": [
            "new-group-001",
            "new-group-002",
            "redis01"
        ],
        "CacheNodeType": "cache.m4.large",
        "PendingModifiedValues": {}
    }
}
```

For additional information, see the AWS CLI topics:

+ [create\-replication\-group](http://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

+ [modify\-replication\-group](http://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group.html)

## Adding Replicas to a Stand\-Alone Redis \(cluster mode disabled\) Cluster \(ElastiCache API\)<a name="Replication.CreatingReplGroup.ExistingCluster.API"></a>

When using the ElastiCache API you create a replication group specifying the available stand\-alone node as the cluster's primary node, `PrimaryClusterId` and the number of nodes you want in the cluster using the CLI command, `CreateReplicationGroup`\. Include the following parameters\.

**ReplicationGroupId**  
The name of the replication group you are creating\. The value of this parameter is used as the basis for the names of the added nodes with a sequential 3\-digit number added to the end of the `ReplicationGroupId`\. For example, `new-group-001`\.  

**Redis \(cluster mode disabled\) Replication Group naming constraints**

+ Must contain from 1 to 20 alphanumeric characters or hyphens\.

+ Must begin with a letter\.

+ Cannot contain two consecutive hyphens\.

+ Cannot end with a hyphen\.

**ReplicationGroupDescription**  
Description of the cluster with replicas\.

**NumCacheClusters**  
The total number of nodes you want in this cluster, including the primary node\. This parameter has a maximum value of 6\.

**PrimaryClusterId**  
The name of the available Redis \(cluster mode disabled\) cluster that you want to be the primary node in this cluster\.

The following command creates the cluster with replicas `new-group` using the available Redis \(cluster mode disabled\) cluster `redis01` as the replication group's primary node\. It creates 2 new nodes which are read replicas\. The settings of `redis01` \(that is, parameter group, security group, node type, engine version, etc\.\) will be applied to all nodes in the replication group\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=CreateReplicationGroup 
   &Engine=redis
   &EngineVersion=3.2.4
   &ReplicationGroupDescription=Demo%20cluster%20with%20replicas
   &ReplicationGroupId=new-group
   &PrimaryClusterId=redis01
   &Version=2015-02-02
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```

For additional information, see the ElastiCache APL topics:

+ [CreateReplicationGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)

+ [ModifyReplicationGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroup.html)

**Next, add read replicas to the replication group**  
After the replication group is created, add one to five read replicas to it using the `CreateCacheCluster` operation, being sure to include the following parameters\. 

**CacheClusterId**  
The name of the cluster you are adding to the replication group\.  

**Cluster naming constraints**

+ Must contain from 1 to 20 alphanumeric characters or hyphens\.

+ Must begin with a letter\.

+ Cannot contain two consecutive hyphens\.

+ Cannot end with a hyphen\.

**ReplicationGroupId**  
The name of the replication group to which you are adding this cache cluster\.

Repeat this operation for each read replica you want to add to the replication group, changing only the value of the `CacheClusterId` parameter\.

The following code adds the read replica `myReplica01` to the replication group `myReplGroup` The settings of the primary cluster–parameter group, security group, node type, etc\.–will be applied to nodes as they are added to the replication group\.

```
https://elasticache.us-west-2.amazonaws.com/
	?Action=CreateCacheCluster
	&CacheClusterId=myReplica01
	&ReplicationGroupId=myReplGroup
	&SignatureMethod=HmacSHA256
	&SignatureVersion=4
	&Version=2015-02-02
	&X-Amz-Algorithm=AWS4-HMAC-SHA256
	&X-Amz-Credential=[your-access-key-id]/20150202/us-west-2/elasticache/aws4_request
	&X-Amz-Date=20150202T170651Z
	&X-Amz-SignedHeaders=content-type;host;user-agent;x-amz-content-sha256;x-amz-date
	&X-Amz-Signature=[signature-value]
```

For additional information and parameters you might want to use, see the ElastiCache API topic [CreateCacheCluster](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheCluster.html)\.