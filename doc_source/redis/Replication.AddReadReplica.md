# Adding a read replica, for Redis \(Cluster Mode Disabled\) replication groups<a name="Replication.AddReadReplica"></a>

Information in the following topic applies to Redis \(cluster mode disabled\) replication groups only\.

As your read traffic increases, you might want to spread those reads across more nodes and reduce the read pressure on any one node\. In this topic, you can find how to add a read replica to a Redis \(cluster mode disabled\) cluster\. 

A Redis \(cluster mode disabled\) replication group can have a maximum of five read replicas\. If you attempt to add a read replica to a replication group that already has five read replicas, the operation fails\.

For information about adding replicas to a Redis \(cluster mode enabled\) replication group, see the following:
+ [Scaling clusters in Redis \(Cluster Mode Enabled\)](scaling-redis-cluster-mode-enabled.md)
+ [Increasing the number of replicas in a shard](increase-replica-count.md)

You can add a read replica to a Redis \(cluster mode disabled\) cluster using the ElastiCache Console, the AWS CLI, or the ElastiCache API\.

**Related topics**
+ [Adding nodes to a cluster](Clusters.AddNode.md)
+ [Adding a read replica to a replication group \(AWS CLI\)](#Replication.AddReadReplica.CLI)
+ [Adding a read replica to a replication group using the API ](#Replication.AddReadReplica.API)

## Adding a read replica to a replication group \(AWS CLI\)<a name="Replication.AddReadReplica.CLI"></a>

To add a read replica to a Redis \(cluster mode disabled\) replication group, use the AWS CLI `create-cache-cluster` command, with the parameter `--replication-group-id` to specify which replication group to add the cluster \(node\) to\.

The following example creates the cluster `my-read replica` and adds it to the replication group `my-replication-group`\. The node types, parameter groups, security groups, maintenance window, and other settings for the read replica are the same as for the other nodes in `my-replication-group`\. 

For Linux, OS X, or Unix:

```
aws elasticache create-cache-cluster \
      --cache-cluster-id my-read-replica \
      --replication-group-id my-replication-group
```

For Windows:

```
aws elasticache create-cache-cluster ^
      --cache-cluster-id my-read-replica ^
      --replication-group-id my-replication-group
```

For more information on adding a read replica using the CLI, see [create\-cache\-cluster](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-cache-cluster.html) in the *Amazon ElastiCache Command Line Reference\.*

## Adding a read replica to a replication group using the API<a name="Replication.AddReadReplica.API"></a>

To add a read replica to a Redis \(cluster mode disabled\) replication group, use the ElastiCache `CreateCacheCluster` operation, with the parameter `ReplicationGroupId` to specify which replication group to add the cluster \(node\) to\.

The following example creates the cluster `myReadReplica` and adds it to the replication group `myReplicationGroup`\. The node types, parameter groups, security groups, maintenance window, and other settings for the read replica are the same as for the other nodes `myReplicationGroup`\.

```
https://elasticache.us-west-2.amazonaws.com/
      ?Action=CreateCacheCluster
      &CacheClusterId=myReadReplica
      &ReplicationGroupId=myReplicationGroup
      &Version=2015-02-02
      &SignatureVersion=4
      &SignatureMethod=HmacSHA256
      &Timestamp=20150202T192317Z
      &X-Amz-Credential=<credential>
```

For more information on adding a read replica using the API, see [CreateCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheCluster.html) in the *Amazon ElastiCache API Reference\.*