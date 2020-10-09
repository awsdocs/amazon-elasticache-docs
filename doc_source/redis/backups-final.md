# Creating a Final Backup<a name="backups-final"></a>

You can create a final backup using the ElastiCache console, the AWS CLI, or the ElastiCache API\.

## Creating a Final Backup \(Console\)<a name="backups-final-CON"></a>

You can create a final backup when you delete either a Redis cluster \(for the API or CLI, a replication group\) using the ElastiCache console\.

To create a final backup when deleting a Redis cluster, on the delete dialog box \(step 5\), choose **Yes** and give the backup a name\.

**Related Topics**
+ [Using the AWS Management Console](Clusters.Delete.md#Clusters.Delete.CON)
+ [Deleting a Replication Group \(Console\)](Replication.DeletingRepGroup.md#Replication.DeletingRepGroup.CON)

## Creating a Final Backup \(AWS CLI\)<a name="backups-final-CLI"></a>

You can create a final backup when deleting a Redis cluster \(for the API or CLI, a replication group\) using the AWS CLI\.

**Topics**
+ [When Deleting a Redis Cluster With No Read Replicas](#w61aac18c42c47b9b7)
+ [When Deleting a Redis Cluster With Read Replicas](#w61aac18c42c47b9b9)

### When Deleting a Redis Cluster With No Read Replicas<a name="w61aac18c42c47b9b7"></a>

To create a final backup, use the `delete-cache-cluster` AWS CLI operation with the following parameters\.
+ `--cache-cluster-id` – Name of the cluster being deleted\.
+ `--final-snapshot-identifier` – Name of the backup\.

The following code creates the final backup `bkup-20150515-final` when deleting the cluster `myRedisCluster`\.

For Linux, macOS, or Unix:

```
aws elasticache delete-cache-cluster \
        --cache-cluster-id myRedisCluster \
        --final-snapshot-identifier bkup-20150515-final
```

For Windows:

```
aws elasticache delete-cache-cluster ^
        --cache-cluster-id myRedisCluster ^
        --final-snapshot-identifier bkup-20150515-final
```

For more information, see [delete\-cache\-cluster](https://docs.aws.amazon.com/cli/latest/reference/elasticache/delete-cache-cluster.html) in the *AWS CLI Command Reference*\.

### When Deleting a Redis Cluster With Read Replicas<a name="w61aac18c42c47b9b9"></a>

To create a final backup when deleting a replication group, use the `delete-replication-group` AWS CLI operation, with the following parameters:
+ `--replication-group-id` – Name of the replication group being deleted\.
+ `--final-snapshot-identifier` – Name of the final backup\.

The following code takes the final backup `bkup-20150515-final` when deleting the replication group `myReplGroup`\.

For Linux, macOS, or Unix:

```
aws elasticache delete-replication-group \
        --replication-group-id myReplGroup \
        --final-snapshot-identifier bkup-20150515-final
```

For Windows:

```
aws elasticache delete-replication-group ^
        --replication-group-id myReplGroup ^
        --final-snapshot-identifier bkup-20150515-final
```

For more information, see [delete\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/delete-replication-group.html) in the *AWS CLI Command Reference*\.

## Creating a Final Backup \(ElastiCache API\)<a name="backups-final-API"></a>

You can create a final backup when deleting a Redis cluster or replication group using the ElastiCache API\.

**Topics**
+ [When Deleting a Redis Cluster](#backups-final-API-Redis-cluster)
+ [When Deleting a Redis Replication Group](#backups-final-API-Redis-rg)

### When Deleting a Redis Cluster<a name="backups-final-API-Redis-cluster"></a>

To create a final backup, use the `DeleteCacheCluster` ElastiCache API operation with the following parameters\.
+ `CacheClusterId` – Name of the cluster being deleted\.
+ `FinalSnapshotIdentifier` – Name of the backup\.

The following ElastiCache API operation creates the backup `bkup-20150515-final` when deleting the cluster `myRedisCluster`\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=DeleteCacheCluster
    &CacheClusterId=myRedisCluster
    &FinalSnapshotIdentifier=bkup-20150515-final
    &Version=2015-02-02
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20150202T192317Z
    &X-Amz-Credential=<credential>
```

For more information, see [DeleteCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheCluster.html) in the *Amazon ElastiCache API Reference*\.

### When Deleting a Redis Replication Group<a name="backups-final-API-Redis-rg"></a>

To create a final backup when deleting a replication group, use the `DeleteReplicationGroup` ElastiCache API operation, with the following parameters:
+ `ReplicationGroupId` – Name of the replication group being deleted\.
+ `FinalSnapshotIdentifier` – Name of the final backup\.

The following ElastiCache API operation creates the backup `bkup-20150515-final` when deleting the replication group `myReplGroup`\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=DeleteReplicationGroup
    &FinalSnapshotIdentifier=bkup-20150515-final
    &ReplicationGroupId=myReplGroup
    &Version=2015-02-02
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20150202T192317Z
    &X-Amz-Credential=<credential>
```

For more information, see [DeleteReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteReplicationGroup.html) in the *Amazon ElastiCache API Reference*\.