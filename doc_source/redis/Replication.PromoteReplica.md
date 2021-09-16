# Promoting a read replica to primary, for Redis \(cluster mode disabled\) replication groups<a name="Replication.PromoteReplica"></a>

Information in the following topic applies to only Redis \(cluster mode disabled\) replication groups\.

You can promote a Redis \(cluster mode disabled\) read replica to primary using the AWS Management Console, the AWS CLI, or the ElastiCache API\. You can't promote a read replica to primary while Multi\-AZ with Automatic Failover is enabled on the Redis \(cluster mode disabled\) replication group\. To promote a Redis \(cluster mode disabled\) replica to primary on a Multi\-AZ enabled replication group, do the following:

1. Modify the replication group to disable Multi\-AZ \(doing this doesn't require that all your clusters be in the same Availability Zone\)\. For more information, see [Modifying a replication group](Replication.Modify.md)\.

1. Promote the read replica to primary\.

1. Modify the replication group to re\-enable Multi\-AZ\.

Multi\-AZ is not available on replication groups running Redis 2\.6\.13 or earlier\.

## Using the AWS Management Console<a name="Replication.PromoteReplica.CON"></a>

The following procedure uses the console to promote a replica node to primary\. 

**To promote a read replica to primary \(console\)**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. If the replica you want to promote is a member of a Redis \(cluster mode disabled\) replication group where Multi\-AZ is enabled, modify the replication group to disable Multi\-AZ before you proceed\. For more information, see [Modifying a replication group](Replication.Modify.md)\.

1. Choose **Redis**, then from the list of clusters, choose the replication group that you want to modify\. This replication group must be running the "Redis" engine, not the "Clustered Redis" engine, and must have two or more nodes\.

1. From the list of nodes, choose the replica node you want to promote to primary, then for **Actions**, choose **Promote**\.

1. In the **Promote Read Replica** dialog box, do the following:

   1. For **Apply Immediately**, choose **Yes** to promote the read replica immediately, or **No** to promote it at the cluster's next maintenance window\.

   1. Choose **Promote** to promote the read replica or **Cancel** to cancel the operation\.

1. If the cluster was Multi\-AZ enabled before you began the promotion process, wait until the replication group's status is **available**, then modify the cluster to re\-enable Multi\-AZ\. For more information, see [Modifying a replication group](Replication.Modify.md)\.

## Using the AWS CLI<a name="Replication.PromoteReplica.CLI"></a>

You can't promote a read replica to primary if the replication group is Multi\-AZ enabled\. In some cases, the replica that you want to promote might be a member of a replication group where Multi\-AZ is enabled\. In these cases, you must modify the replication group to disable Multi\-AZ before you proceed\. Doing this doesn't require that all your clusters be in the same Availability Zone\. For more information on modifying a replication group, see [Modifying a replication group](Replication.Modify.md)\.

The following AWS CLI command modifies the replication group `sample-repl-group`, making the read replica `my-replica-1` the primary in the replication group\.

For Linux, macOS, or Unix:

```
aws elasticache modify-replication-group \
   --replication-group-id sample-repl-group \
   --primary-cluster-id my-replica-1
```

For Windows:

```
aws elasticache modify-replication-group ^
   --replication-group-id sample-repl-group ^
   --primary-cluster-id my-replica-1
```

For more information on modifying a replication group, see [modify\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group.html) in the *Amazon ElastiCache Command Line Reference\.*

## Using the ElastiCache API<a name="Replication.PromoteReplica.API"></a>

You can't promote a read replica to primary if the replication group is Multi\-AZ enabled\. In some cases, the replica that you want to promote might be a member of a replication group where Multi\-AZ is enabled\. In these cases, you must modify the replication group to disable Multi\-AZ before you proceed\. Doing this doesn't require that all your clusters be in the same Availability Zone\. For more information on modifying a replication group, see [Modifying a replication group](Replication.Modify.md)\.

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
   &X-Amz-Algorithm=&AWS;4-HMAC-SHA256
   &X-Amz-Date=20141201T220302Z
   &X-Amz-SignedHeaders=Host
   &X-Amz-Expires=20141201T220302Z
   &X-Amz-Credential=<credential>
   &X-Amz-Signature=<signature>
```

For more information on modifying a replication group, see [ModifyReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroup.html) in the *Amazon ElastiCache API Reference\.*