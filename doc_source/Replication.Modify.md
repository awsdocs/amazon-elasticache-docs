# Modifying a Replication Group<a name="Replication.Modify"></a>

**Important Constraints**  
Currently, ElastiCache supports limited modifications of a Redis \(cluster mode enabled\) replication group, for example changing the engine version, using the API operation `ModifyReplicationGroup` \(CLI: `modify-replication-group`\)\. You can modify the number of shards \(node groups\) in a Redis \(cluster mode enabled\) cluster with the API operation [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroupShardConfiguration.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroupShardConfiguration.html) \(CLI: [https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group-shard-configuration.html](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group-shard-configuration.html)\)\. For more information, see [Scaling Redis \(cluster mode enabled\) Clusters](scaling-redis-cluster-mode-enabled.md)\.  
Other modifications to a Redis \(cluster mode enabled\) cluster require that you create a cluster with the new cluster incorporating the changes\.
You can upgrade Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\) clusters and replication groups to newer engine versions, but you cannot downgrade to earlier engine versions except by deleting the existing cluster or replication group and creating it anew\. For more information, see [Upgrading Engine Versions](VersionManagement.md)\.

You can modify a Redis \(cluster mode disabled\) cluster's settings using the ElastiCache console, the AWS CLI, or the ElastiCache API\. Currently, ElastiCache supports a limited number of modifications on a Redis \(cluster mode enabled\) replication group\. Other modifications require you create a backup of the current replication group then using that backup to seed a new Redis \(cluster mode enabled\) replication group\.

**Topics**
+ [Using the AWS Management Console](#Replication.Modify.CON)
+ [Using the AWS CLI](#Replication.Modify.CLI)
+ [Using the ElastiCache API](#Replication.Modify.API)

## Using the AWS Management Console<a name="Replication.Modify.CON"></a>

To modify a Redis \(cluster mode disabled\) cluster, see [Modifying an ElastiCache Cluster](Clusters.Modify.md)\.

## Using the AWS CLI<a name="Replication.Modify.CLI"></a>

The following AWS CLI command enables Multi\-AZ on an existing Redis replication group\. You can use the same command to make other modifications to a replication group\.

For Linux, macOS, or Unix:

```
aws elasticache modify-replication-group \
   --replication-group-id myReplGroup \
   --automatic-failover-enabled
```

For Windows:

```
aws elasticache modify-replication-group ^
   --replication-group-id myReplGroup ^
   --automatic-failover-enabled
```

For more information on the AWS CLI `modify-replication-group` command, see [modify\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group.html)\.

## Using the ElastiCache API<a name="Replication.Modify.API"></a>

The following ElastiCache API operation enables Multi\-AZ on an existing Redis replication group\. You can use the same operation to make other modifications to a replication group\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=ModifyReplicationGroup
   &AutomaticFailoverEnabled=true  
   &ReplicationGroupId=myReplGroup
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20141201T220302Z
   &Version=2014-12-01
   &X-Amz-Algorithm=AWS4-HMAC-SHA256
   &X-Amz-Date=20141201T220302Z
   &X-Amz-SignedHeaders=Host
   &X-Amz-Expires=20141201T220302Z
   &X-Amz-Credential=<credential>
   &X-Amz-Signature=<signature>
```

For more information on the ElastiCache API `ModifyReplicationGroup` operation, see [ModifyReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroup.html)\.