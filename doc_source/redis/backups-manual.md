# Making Manual Backups<a name="backups-manual"></a>

In addition to automatic backups, you can create a *manual* backup at any time\. Unlike automatic backups, which are automatically deleted after a specified retention period, manual backups do not have a retention period after which they are automatically deleted\. You must manually delete any manual backup\. Even if you delete a cluster or node, any manual backups from that cluster or node are retained\. If you no longer want to keep a manual backup, you must explicitly delete it yourself\.

Manual backups are useful for testing and archiving\. For example, suppose that you've developed a set of baseline data for testing purposes\. You can create a manual backup of the data and restore it whenever you want\. After you test an application that modifies the data, you can reset the data by creating a new cluster and restoring from your baseline backup\. When the cluster is ready, you can test your applications against the baseline data again—and repeat this process as often as needed\.

In addition to directly creating a manual backup, you can create a manual backup in one of the following ways:
+ [Copying a Backup](backups-copying.md) It does not matter whether the source backup was created automatically or manually\.
+ [Creating a Final Backup](backups-final.md) Create a backup immediately before deleting a cluster or node\.

**Other topics of import**
+ [Backup Constraints](backups.md#backups-constraints)
+ [Backup Costs](backups.md#backups-costs)
+ [Performance Impact of Backups](backups.md#backups-performance)

You can create a manual backup of a node using the AWS Management Console, the AWS CLI, or the ElastiCache API\.

## Creating a Manual Backup \(Console\)<a name="backups-manual-CON"></a>

**To create a backup of a cluster \(console\)**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Redis**\.

   The Redis clusters screen appears\.

1. Choose the box to the left of the name of the Redis cluster you want to back up\.

1. Choose **Backup**\.

1. In the **Create Backup** dialog, type in a name for your backup in the **Backup Name** box\. We recommend that the name indicate which cluster was backed up and the date and time the backup was made\.

   Cluster naming constraints are as follows:
   + Must contain 1–40 alphanumeric characters or hyphens\.
   + Must begin with a letter\.
   + Can't contain two consecutive hyphens\.
   + Can't end with a hyphen\.

1. Choose **Create Backup**\.

   The status of the cluster changes to *snapshotting*\. When the status returns to *available* the backup is complete\.

## Creating a Manual Backup \(AWS CLI\)<a name="backups-manual-CLI"></a>

To create a manual backup of a cluster using the AWS CLI, use the `create-snapshot` AWS CLI operation with the following parameters:
+ `--cache-cluster-id`
  + If the cluster you're backing up has no replica nodes, `--cache-cluster-id` is the name of the cluster you are backing up, for example *mycluster*\.
  + If the cluster you're backing up has one or more replica nodes, `--cache-cluster-id` is the name of the node in the cluster that you want to use for the backup\. For example, the name might be *mycluster\-002*\.

  Use this parameter only when backing up a Redis \(cluster mode disabled\) cluster\.

   
+ `--replication-group-id` – Name of the Redis \(cluster mode enabled\) cluster \(CLI/API: a replication group\) to use as the source for the backup\. Use this parameter when backing up a Redis \(cluster mode enabled\) cluster\.

   
+ `--snapshot-name` – Name of the snapshot to be created\.

  Cluster naming constraints are as follows:
  + Must contain 1–40 alphanumeric characters or hyphens\.
  + Must begin with a letter\.
  + Can't contain two consecutive hyphens\.
  + Can't end with a hyphen\.

### Example 1: Backing Up a Redis \(Cluster Mode Disabled\) Cluster That Has No Replica Nodes<a name="backups-manual-CLI-example1"></a>

The following AWS CLI operation creates the backup `bkup-20150515` from the Redis \(cluster mode disabled\) cluster `myNonClusteredRedis` that has no read replicas\.

For Linux, macOS, or Unix:

```
aws elasticache create-snapshot \
    --cache-cluster-id myNonClusteredRedis \
    --snapshot-name bkup-20150515
```

For Windows:

```
aws elasticache create-snapshot ^
    --cache-cluster-id myNonClusteredRedis ^
    --snapshot-name bkup-20150515
```

### Example 2: Backing Up a Redis \(Cluster Mode Disabled\) Cluster with Replica Nodes<a name="backups-manual-CLI-example2"></a>

The following AWS CLI operation creates the backup `bkup-20150515` from the Redis \(cluster mode disabled\) cluster `myNonClusteredRedis`\. This backup has one or more read replicas\.

For Linux, macOS, or Unix:

```
aws elasticache create-snapshot \
    --cache-cluster-id myNonClusteredRedis-001 \
    --snapshot-name bkup-20150515
```

For Windows:

```
aws elasticache create-snapshot ^
    --cache-cluster-id myNonClusteredRedis-001 ^
    --snapshot-name bkup-20150515
```

**Example Output: Backing Up a Redis \(Cluster Mode Disabled\) Cluster with Replica Nodes**

Output from the operation looks something like the following\.

```
{
    "Snapshot": {
        "Engine": "redis", 
        "CacheParameterGroupName": "default.redis3.2", 
        "VpcId": "vpc-91280df6", 
        "CacheClusterId": "myNonClusteredRedis-001", 
        "SnapshotRetentionLimit": 0, 
        "NumCacheNodes": 1, 
        "SnapshotName": "bkup-20150515", 
        "CacheClusterCreateTime": "2017-01-12T18:59:48.048Z", 
        "AutoMinorVersionUpgrade": true, 
        "PreferredAvailabilityZone": "us-east-1c", 
        "SnapshotStatus": "creating", 
        "SnapshotSource": "manual", 
        "SnapshotWindow": "08:30-09:30", 
        "EngineVersion": "3.2.4", 
        "NodeSnapshots": [
            {
                "CacheSize": "", 
                "CacheNodeId": "0001", 
                "CacheNodeCreateTime": "2017-01-12T18:59:48.048Z"
            }
        ], 
        "CacheSubnetGroupName": "default", 
        "Port": 6379, 
        "PreferredMaintenanceWindow": "wed:07:30-wed:08:30", 
        "CacheNodeType": "cache.m3.2xlarge"
    }
}
```

### Example 3: Backing Up a Cluster for Redis \(Cluster Mode Enabled\)<a name="backups-manual-CLI-example3"></a>

The following AWS CLI operation creates the backup `bkup-20150515` from the Redis \(cluster mode enabled\) cluster `myClusteredRedis`\. Note the use of `--replication-group-id` instead of `--cache-cluster-id` to identify the source\.

For Linux, macOS, or Unix:

```
aws elasticache create-snapshot \
    --replication-group-id myClusteredRedis \
    --snapshot-name bkup-20150515
```

For Windows:

```
aws elasticache create-snapshot ^
    --replication-group-id myClusteredRedis ^
    --snapshot-name bkup-20150515
```

**Example Output: Backing Up a Redis \(Cluster Mode Enabled\) Cluster**

Output from this operation looks something like the following\.

```
{
    "Snapshot": {
        "Engine": "redis", 
        "CacheParameterGroupName": "default.redis3.2.cluster.on", 
        "VpcId": "vpc-91280df6", 
        "NodeSnapshots": [
            {
                "CacheSize": "", 
                "NodeGroupId": "0001"
            }, 
            {
                "CacheSize": "", 
                "NodeGroupId": "0002"
            }
        ], 
        "NumNodeGroups": 2, 
        "SnapshotName": "bkup-20150515", 
        "ReplicationGroupId": "myClusteredRedis", 
        "AutoMinorVersionUpgrade": true, 
        "SnapshotRetentionLimit": 1, 
        "AutomaticFailover": "enabled", 
        "SnapshotStatus": "creating", 
        "SnapshotSource": "manual", 
        "SnapshotWindow": "10:00-11:00", 
        "EngineVersion": "3.2.4", 
        "CacheSubnetGroupName": "default", 
        "ReplicationGroupDescription": "2 shards 2 nodes each", 
        "Port": 6379, 
        "PreferredMaintenanceWindow": "sat:03:30-sat:04:30", 
        "CacheNodeType": "cache.r3.large"
    }
}
```

### Related Topics<a name="backups-manual-CLI-see-also"></a>

For more information, see [create\-snapshot](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-snapshot.html) in the *AWS CLI Command Reference*\.

## Creating a Manual Backup \(ElastiCache API\)<a name="backups-manual-API"></a>

To create a manual backup of a cluster using the ElastiCache API, use the `CreateSnapshot` ElastiCache API operation with the following parameters:
+ `CacheClusterId`
  + If the cluster you're backing up has no replica nodes, *CacheClusterId* is the name of the cluster you are backing up, for example *mycluster*\.
  + If the cluster you're backing up has one or more replica nodes, *CacheClusterId* is the name of the node in the cluster that you want to use for the backup, for example *mycluster\-002*\.

  Only use this parameter when backing up a Redis \(cluster mode disabled\) cluster\.

   
+ `ReplicationGroupId` – Name of the Redis \(cluster mode enabled\) cluster \(CLI/API: a replication group\) to use as the source for the backup\. Use this parameter when backing up a Redis \(cluster mode enabled\) cluster\.

   
+ `SnapshotName` – Name of the snapshot to be created\.

  Cluster naming constraints are as follows:
  + Must contain 1–40 alphanumeric characters or hyphens\.
  + Must begin with a letter\.
  + Can't contain two consecutive hyphens\.
  + Can't end with a hyphen\.

**Topics**
+ [Redis \(Cluster Mode Disabled\) with No Replica Nodes](#backups-manual-API-example1)
+ [Redis \(Cluster Mode Disabled\) with Replica Nodes](#backups-manual-API-example2)
+ [Redis \(Cluster Mode Enabled\)](#backups-manual-API-example3)
+ [Related Topics](#backups-manual-api-see-also)

### Example 1: Backing Up a Redis \(Cluster Mode Disabled\) Cluster That Has No Replica Nodes<a name="backups-manual-API-example1"></a>

The following ElastiCache API operation creates the backup `bkup-20150515` from the Redis \(cluster mode disabled\) cluster `myNonClusteredRedis` that has no read replicas\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=CreateSnapshot
    &CacheClusterId=myNonClusteredRedis
    &SnapshotName=bkup-20150515
    &Version=2015-02-02
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20150202T192317Z
    &X-Amz-Credential=<credential>
```

### Example 2: Backing Up a Redis \(Cluster Mode Disabled\) Cluster with Replica Nodes<a name="backups-manual-API-example2"></a>

The following ElastiCache API operation creates the backup `bkup-20150515` from the Redis \(cluster mode disabled\) cluster `myNonClusteredRedis` which has one or more read replicas\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=CreateSnapshot
    &CacheClusterId=myNonClusteredRedis-001
    &SnapshotName=bkup-20150515
    &Version=2015-02-02
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20150202T192317Z
    &X-Amz-Credential=<credential>
```

### Example 3: Backing Up a Redis \(Cluster Mode Enabled\) Cluster<a name="backups-manual-API-example3"></a>

The following ElastiCache API operation creates the backup `bkup-20150515` from the Redis \(cluster mode enabled\) cluster `myClusteredRedis`\. Note the use of `ReplicationGroupId` instead of `CacheClusterId` to identify the source\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=CreateSnapshot
    &ReplicationGroupId=myClusteredRedis
    &SnapshotName=bkup-20150515
    &Version=2015-02-02
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20150202T192317Z
    &X-Amz-Credential=<credential>
```

For more information, see [CreateSnapshot](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateSnapshot.html) in the *Amazon ElastiCache API Reference*\.

### Related Topics<a name="backups-manual-api-see-also"></a>

For more information, see [CreateSnapshot](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateSnapshot.html) in the *Amazon ElastiCache API Reference*\.