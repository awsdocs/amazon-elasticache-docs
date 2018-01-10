# Promoting a Read\-Replica to Primary<a name="Replication.PromoteReplica"></a>

**Important**  
Currently, ElastiCache does not support promoting a read replica to primary for a Redis \(cluster mode enabled\) replication group\.

You can promote a read replica to primary using the ElastiCache console, the AWS CLI, or the ElastiCache API\. However, you cannot promote a read replica to primary while Multi\-AZ is enabled on the replication group\. If Multi\-AZ is enabled you must:

**To promote a read replica node to primary**

1. Modify the replication group to disable Multi\-AZ \(this does not require that all your clusters be in the same Availability Zone\)\.

   For information on modifying a replication group's settings, see [Modifying a Cluster with Replicas](Replication.Modify.md)\.

1. Promote the read replica to primary\.

1. Modify the replication group to re\-enable Multi\-AZ\.

Multi\-AZ with automatic failover is not available on replication groups running Redis 2\.6\.13\.


+ [Promoting a Read\-Replica to Primary \(Console\)](#Replication.PromoteReplica.CON)
+ [Promoting a Read\-Replica to Primary \(AWS CLI\)](#Replication.PromoteReplica.CLI)
+ [Promoting a Read\-Replica to Primary \(ElastiCache API\)](#Replication.PromoteReplica.API)

## Promoting a Read\-Replica to Primary \(Console\)<a name="Replication.PromoteReplica.CON"></a>

**To promote a read replica to primary \(console\)**

1. If the replica you want to promote is a member of a Redis \(cluster mode disabled\) cluster with replicas where Multi\-AZ is enabled, modify the cluster to disable Multi\-AZ before you proceed \(this does not require that all your clusters be in the same Availability Zone\)\. For more information on modifying a cluster, see [Modifying a Cluster \(Console\)](Clusters.Modify.md#Clusters.Modify.CON)\.

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. Choose **Redis**\.

   A list of clusters running Redis appears\.

1. From the list of clusters, choose the name of the cluster you wish to modify\. This cluster must be running the "Redis" engine, not the "Clusterd Redis" engine, and it must have 2 or more nodes\.

   A list of the cluster's nodes appears\.

1. Choose the box to the left of the name of the replica node you want to promote to Primary\.

   Choose **Promote**\.

1. In the **Promote Read Replica** dialog box:

   1. Choose *Yes* to promote the read replica immediately, or *No* to promote it at the cluster's next maintenance window\.

   1. Choose **Promote** to promote the read replica or **Cancel** to cancel the operation\.

1. If the cluster had Multi\-AZ enabled before you began the promotion process, modify the cluster to re\-enable Multi\-AZ\. For more information about modifying a cluster, see [Modifying a Cluster \(Console\)](Clusters.Modify.md#Clusters.Modify.CON)

## Promoting a Read\-Replica to Primary \(AWS CLI\)<a name="Replication.PromoteReplica.CLI"></a>

You cannot promote a read replica to primary if the replication group is Multi\-AZ enabled\. If the replica you want to promote is a member of a replication group where Multi\-AZ is enabled, you must modify the replication group to disable Multi\-AZ before you proceed \(this does not require that all your clusters be in the same Availability Zone\)\. For more information on modifying a replication group, see [Modifying a Replication Group \(AWS CLI\)](Replication.Modify.md#Replication.Modify.CLI)\.

The following AWS CLI command modifies the replication group `new-group`, making the read replica `my-replica-1` the primary in the replication group\.

For Linux, macOS, or Unix:

```
aws elasticache modify-replication-group \
   --replication-group-id new-group \
   --primary-cluster-id my-replica-1
```

For Windows:

```
aws elasticache modify-replication-group ^
   --replication-group-id new-group ^
   --primary-cluster-id my-replica-1
```

For more information on modifying a replication group, see the AWS CLI topic [modify\-replication\-group](http://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group.html)\.

## Promoting a Read\-Replica to Primary \(ElastiCache API\)<a name="Replication.PromoteReplica.API"></a>

You cannot promote a read replica to primary if the replication group is Multi\-AZ enabled\. If the replica you want to promote is a member of a replication group where Multi\-AZ is enabled, you must modify the replication group to disable Multi\-AZ before you proceed \(this does not require that all your clusters be in the same Availability Zone\)\. For more information on modifying a replication group, see [Modifying a Replication Group \(ElastiCache API\)](Replication.Modify.md#Replication.Modify.API)\.

The following ElastiCache API action modifies the replication group `myReplGroup`, making the read replica `myReplica-1` the primary in the replication group\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=ModifyReplicationGroup
   &ReplicationGroupId=myReplGroup
   &PrimaryClusterId=myReplica-1  
   &Version=2014-12-01
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20141201T220302Z
   &X-Amz-Algorithm=AWS4-HMAC-SHA256
   &X-Amz-Date=20141201T220302Z
   &X-Amz-SignedHeaders=Host
   &X-Amz-Expires=20141201T220302Z
   &X-Amz-Credential=<credential>
   &X-Amz-Signature=<signature>
```

For more information on modifying a replication group, see the ElastiCache API topic [ModifyReplicationGroup](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroup.html)\.