# Adding a Read Replica to a Redis Cluster<a name="Replication.AddReadReplica"></a>

**Important**  
 Currently, ElastiCache does not support adding read replicas to a Redis \(cluster mode enabled\)\. If you need more read replicas, create the cluster anew with the desired number of read replicas\.

As your read traffic increases, you might want to spread those reads across more nodes thereby reducing the read pressure on any one node\. This topic covers how to add a read replica to a cluster\. You can add a read replica to a cluster using the ElastiCache Console, the AWS CLI, or the ElastiCache API\.

+ [Adding Nodes to a Cluster](Clusters.AddNode.md)

+ [Adding a Read Replica to a Replication Group \(AWS CLI\)](#Replication.AddReadReplica.CLI)

+ [Adding a Read Replica to a Replication Group \(ElastiCache API\)](#Replication.AddReadReplica.API)


+ [Adding a Read Replica to a Cluster \(Console\)](#Replication.AddReadReplica.CON)
+ [Adding a Read Replica to a Replication Group \(AWS CLI\)](#Replication.AddReadReplica.CLI)
+ [Adding a Read Replica to a Replication Group \(ElastiCache API\)](#Replication.AddReadReplica.API)

## Adding a Read Replica to a Cluster \(Console\)<a name="Replication.AddReadReplica.CON"></a>

To add a replica to a Redis \(cluster mode disabled\) cluster, see [Adding Nodes to a Cluster](Clusters.AddNode.md)\.

## Adding a Read Replica to a Replication Group \(AWS CLI\)<a name="Replication.AddReadReplica.CLI"></a>

To add a read replica to a replication group, use the AWS CLI `create-cache-cluster` command, with the parameter `--replication-group-id` to specify which replication group to add the cluster \(node\) to\.

A replication group can have a maximum of 5 read replicas\. If you attempt to add a read replica to a replication group that already has 5 read replicas, the operation will fail\.

The following example creates the cluster `my-read-replica` and adds it to the replication group `my-replication-group`\. The node types, parameter groups, security groups, maintenance window and other settings for my read replica will be the same as the other nodes in my replication group

For Linux, macOS, or Unix:

```
aws elasticache create-cache-cluster \
   --cache-cluster-id my-read-replica \
   --replicationgroup-id my-replication-group
```

For Windows:

```
aws elasticache create-cache-cluster ^
   --cache-cluster-id my-read-replica ^
   --replicationgroup-id my-replication-group
```

For more information, see the AWS CLI topic [create\-cache\-cluster](http://docs.aws.amazon.com/cli/latest/reference/elasticache/create-cache-cluster.html)\.

## Adding a Read Replica to a Replication Group \(ElastiCache API\)<a name="Replication.AddReadReplica.API"></a>

To add a read replica to a replication group, use the ElastiCache `CreateCacheCluster` operation, with the parameter `ReplicationGroupId` to specify which replication group to add the cluster \(node\) to\.

A replication group can have a maximum of five read replicas\. If you attempt to add a read replica to a replication group that already has five read replicas, the operation will fail\.

The following example creates the cluster `myReadReplica` and adds it to the replication group `myReplicationGroup`\. The node types, parameter groups, security groups, maintenance window and other settings for my read replica will be the same as the other nodes in my replication group

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

For more information, see the ElastiCache API topic [CreateCacheCluster](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheCluster.html)\.