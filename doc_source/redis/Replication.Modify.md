# Modifying a replication group<a name="Replication.Modify"></a>

**Important Constraints**  
Currently, ElastiCache supports limited modifications of a Redis \(cluster mode enabled\) replication group, for example changing the engine version, using the API operation `ModifyReplicationGroup` \(CLI: `modify-replication-group`\)\. You can modify the number of shards \(node groups\) in a Redis \(cluster mode enabled\) cluster with the API operation [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroupShardConfiguration.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroupShardConfiguration.html) \(CLI: [https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group-shard-configuration.html](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group-shard-configuration.html)\)\. For more information, see [Scaling clusters in Redis \(Cluster Mode Enabled\)](scaling-redis-cluster-mode-enabled.md)\.  
Other modifications to a Redis \(cluster mode enabled\) cluster require that you create a cluster with the new cluster incorporating the changes\.
You can upgrade Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\) clusters and replication groups to newer engine versions\. However, you can't downgrade to earlier engine versions except by deleting the existing cluster or replication group and creating it again\. For more information, see [Upgrading engine versions](VersionManagement.md)\.
You can upgrade an existing ElastiCache for Redis cluster that uses cluster mode disabled to use cluster mode enabled, using the console, [ModifyReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroup.html) API or the [modify\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group.html) CLI command, as shown in the example below\. Or you can follow the steps at [Modifying cluster mode](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/modify-cluster-mode.html)\.

You can modify a Redis \(cluster mode disabled\) cluster's settings using the ElastiCache console, the AWS CLI, or the ElastiCache API\. Currently, ElastiCache supports a limited number of modifications on a Redis \(cluster mode enabled\) replication group\. Other modifications require you create a backup of the current replication group then using that backup to seed a new Redis \(cluster mode enabled\) replication group\.

**Topics**
+ [Using the AWS Management Console](#Replication.Modify.CON)
+ [Using the AWS CLI](#Replication.Modify.CLI)
+ [Using the ElastiCache API](#Replication.Modify.API)

## Using the AWS Management Console<a name="Replication.Modify.CON"></a>

To modify a Redis \(cluster mode disabled\) cluster, see [Modifying an ElastiCache cluster](Clusters.Modify.md)\.

## Using the AWS CLI<a name="Replication.Modify.CLI"></a>

The following are AWS CLI examples of the `modify-replication-group` command\. You can use the same command to make other modifications to a replication group\.

**Enable Multi\-AZ on an existing Redis replication group:**

For Linux, macOS, or Unix:

```
aws elasticache modify-replication-group \
   --replication-group-id myReplGroup \
   --multi-az-enabled = true
```

For Windows:

```
aws elasticache modify-replication-group ^
   --replication-group-id myReplGroup ^
   --multi-az-enabled
```

**Modify cluster mode from disabled to enabled:**

To modify cluster mode from *disabled* to *enabled*, you must first set the cluster mode to *compatible*\. Compatible mode allows your Redis clients to connect using both cluster mode enabled and cluster mode disabled\. After you migrate all Redis clients to use cluster mode enabled, you can then complete cluster mode configuration and set the cluster mode to *enabled*\.

For Linux, macOS, or Unix:

Set to cluster mode to *compatible*\.

```
aws elasticache modify-replication-group \
   --replication-group-id myReplGroup \
   --cache-parameter-group-name myParameterGroupName \
   --cluster-mode compatible
```

Set to cluster mode to *enabled*\.

```
aws elasticache modify-replication-group \
   --replication-group-id myReplGroup \
   --cluster-mode enabled
```

For Windows:

Set to cluster mode to *compatible*\.

```
aws elasticache modify-replication-group ^
   --replication-group-id myReplGroup ^
   --cache-parameter-group-name myParameterGroupName ^
   --cluster-mode compatible
```

Set to cluster mode to *enabled*\.

```
aws elasticache modify-replication-group ^
   --replication-group-id myReplGroup ^
   --cluster-mode enabled
```

For more information on the AWS CLI `modify-replication-group` command, see [modify\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group.html) or [Modifying cluster mode](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/modify-cluster-mode.html) in the *ElastiCache for Redis User Guide*\.

## Using the ElastiCache API<a name="Replication.Modify.API"></a>

The following ElastiCache API operation enables Multi\-AZ on an existing Redis replication group\. You can use the same operation to make other modifications to a replication group\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=ModifyReplicationGroup
   &AutomaticFailoverEnabled=true  
   &Mutli-AZEnabled=true  
   &ReplicationGroupId=myReplGroup
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20141201T220302Z
   &Version=2014-12-01
   &X-Amz-Algorithm=&AWS;4-HMAC-SHA256
   &X-Amz-Date=20141201T220302Z
   &X-Amz-SignedHeaders=Host
   &X-Amz-Expires=20141201T220302Z
   &X-Amz-Credential=<credential>
   &X-Amz-Signature=<signature>
```

For more information on the ElastiCache API `ModifyReplicationGroup` operation, see [ModifyReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroup.html)\.