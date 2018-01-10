# Modifying a Cluster with Replicas<a name="Replication.Modify"></a>

**Important Constraints**  
Currently, ElastiCache does not support modifying a Redis \(cluster mode enabled\) cluster using the API operation `ModifyReplicationGroup` \(CLI: `modify-replication-group`\)\. However, you can modify the number of shards \(node groups\) in a Redis \(cluster mode enabled\) cluster with the API operation [http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroupShardConfiguration.html](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroupShardConfiguration.html) \(CLI: [http://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group-shard-configuration.html](http://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group-shard-configuration.html)\)\. For more information, see [Scaling for Amazon ElastiCache for Redis—Redis \(cluster mode enabled\)](scaling-redis-cluster-mode-enabled.md)\.  
Other modifications to a Redis \(cluster mode enabled\) cluster require that you create the cluster anew with the new cluster incorporating the changes\.
You can upgrade to newer engine versions, but you cannot downgrade to earlier engine versions except by deleting the existing cluster or replication group and creating it anew\. For more information, see [Upgrading Engine Versions](VersionManagement.md)\.

You can modify a Redis \(cluster mode disabled\) cluster's settings using the ElastiCache console, the AWS CLI, or the ElastiCache API\. Currently, ElastiCache does not support modifying a Redis \(cluster mode enabled\) replication group except by creating a backup of the current replication group then using that backup to seed a new Redis \(cluster mode enabled\) replication group\.


+ [Modifying a Redis Cluster \(Console\)](#Replication.Modify.CON)
+ [Modifying a Replication Group \(AWS CLI\)](#Replication.Modify.CLI)
+ [Modifying a Replication Group \(ElastiCache API\)](#Replication.Modify.API)

## Modifying a Redis Cluster \(Console\)<a name="Replication.Modify.CON"></a>

To modify a Redis \(cluster mode disabled\) cluster, see [Modifying an ElastiCache Cluster](Clusters.Modify.md)\.

## Modifying a Replication Group \(AWS CLI\)<a name="Replication.Modify.CLI"></a>

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

For more information on the AWS CLI `modify-replication-group` command, see [modify\-replication\-group](http://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group.html)\.

## Modifying a Replication Group \(ElastiCache API\)<a name="Replication.Modify.API"></a>

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

For more information on the ElastiCache API `ModifyReplicationGroup` operation, see [ModifyReplicationGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroup.html)\.