# Data tiering<a name="data-tiering"></a>

Clusters that comprise a replication group and use a node type from the r6gd family have their data tiered between memory and local SSD \(solid state drives\) storage\. Data tiering provides a new price\-performance option for Redis workloads by utilizing lower\-cost solid state drives \(SSDs\) in each cluster node in addition to storing data in memory\. It is ideal for workloads that access up to 20 percent of their overall dataset regularly, and for applications that can tolerate additional latency when accessing data on SSD\.

On clusters with data tiering, ElastiCache monitors the last access time of every item it stores\. When available memory \(DRAM\) is fully consumed, ElastiCache uses a least\-recently used \(LRU\) algorithm to automatically move infrequently accessed items from memory to SSD\. When data on SSD is subsequently accessed, ElastiCache automatically and asynchronously moves it back to memory before processing the request\. If you have a workload that accesses only a subset of its data regularly, data tiering is an optimal way to scale your capacity cost\-effectively\.

Note that when using data tiering, keys themselves always remain in memory, while the LRU governs the placement of values on memory vs\. disk\. In general, we recommend that your key sizes are smaller than your value sizes when using data tiering\.

Data tiering is designed to have minimal performance impact to application workloads\. For example, assuming 500\-byte String values, you can expect an additional 300 microseconds of latency on average for requests to data stored on SSD compared to requests to data in memory\.

With the largest data tiering node size \(cache\.r6gd\.16xlarge\), you can store up to 1 petabyte in a single 500\-node cluster \(500 TB when using 1 read replica\)\. Data tiering is compatible with all Redis commands and data structures supported in ElastiCache\. You don't need any client\-side changes to use this feature\. 

**Topics**
+ [Best practices](#data-tiering-best-practices)
+ [Limitations](#data-tiering-prerequisites)
+ [Pricing](#data-tiering-pricing)
+ [Monitoring](#data-tiering-monitoring)
+ [Using data tiering](#data-tiering-enabling)
+ [Restoring data from backup into clusters with data tiering enabled](#data-tiering-enabling-snapshots)

## Best practices<a name="data-tiering-best-practices"></a>

We recommend the following best practices:
+ Data tiering is ideal for workloads that access up to 20 percent of their overall dataset regularly, and for applications that can tolerate additional latency when accessing data on SSD\.
+ When using SSD capacity available on data\-tiered nodes, we recommend that value size be larger than the key size\. When items are moved between DRAM and SSD, keys will always remain in memory and only the values are moved to the SSD tier\. 

## Limitations<a name="data-tiering-prerequisites"></a>

Data tiering has the following limitations:
+ You can only use data tiering on clusters that are part of a replication group\.
+ The node type you use must be from the r6gd family, which is available in the following regions: `us-east-2`, `us-east-1`, `us-west-2`, `us-west-1`, `eu-west-1`, `eu-central-1`, `ap-northeast-1`, `ap-southeast-1`, `ap-southeast-2`, `ap-south-1`, `ca-central-1` and `sa-east-1`\.
+ You must use the Redis 6\.2 or later engine\.
+ You cannot restore a backup of an r6gd cluster into another cluster unless it also uses r6gd\.
+ You cannot export a backup to Amazon S3 for data\-tiering clusters\.
+ Online migration is not supported for clusters running on the r6gd node type\.
+ Scaling is not supported from a data tiering cluster \(for example, a cluster using an r6gd node type\) to a cluster that does not use data tiering \(for example, a cluster using an r6g node type\)\.
+ Auto scaling is not supported for clusters running using data tiering\.
+ Data tiering only supports `volatile-lru`, `allkeys-lru` and `noeviction` maxmemory policies\. 
+ Forkless save is not supported\. For more information, see [How synchronization and backup are implemented](Replication.Redis.Versions.md)\.
+ Items larger than 128 MiB are not moved to SSD\.

## Pricing<a name="data-tiering-pricing"></a>

R6gd nodes have 4\.8x more total capacity \(memory \+ SSD\) and can help you achieve over 60 percent savings when running at maximum utilization compared to R6g nodes \(memory only\)\. For more information, see [ElastiCache pricing](https://aws.amazon.com/elasticache/pricing/)\.

## Monitoring<a name="data-tiering-monitoring"></a>

ElastiCache for Redis offers metrics designed specifically to monitor the performance clusters that use data tiering\. To monitor the ratio of items in DRAM compared to SSD, you can use the `CurrItems` metric at [Metrics for Redis](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/CacheMetrics.Redis.html)\. You can calculate the percentage as: *\(CurrItems with Dimension: Tier = Memory \* 100\) / \(CurrItems with no dimension filter\)*\. When the percentage of items in memory decreases below 5 percent, we recommend that you consider scale out for [Cluster Mode Enabled clusters](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/scaling-redis-cluster-mode-enabled.html) or scale up for [Cluster Mode disabled clusters ](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/scaling-redis-cluster-mode-enabled.html)\. For more information, see *Metrics for Redis clusters that use data tiering* at [Metrics for Redis](CacheMetrics.Redis.md)\.

## Using data tiering<a name="data-tiering-enabling"></a>

### Using data tiering using the AWS Management Console<a name="data-tiering-enabling-console"></a>

When creating a cluster as part of a replication group, you use data tiering by selecting a node type from the r6gd family, such as *cache\.r6gd\.xlarge*\. Selecting that node type automatically enables data tiering\. 

For more information on creating a cluster, see [Creating a cluster](Clusters.Create.md)\.

### Enabling data tiering using the AWS CLI<a name="data-tiering-enabling-cli"></a>

When creating a replication group using the AWS CLI, you use data tiering by selecting a node type from the r6gd family, such as *cache\.r6gd\.xlarge* and setting the `--data-tiering-enabled` parameter\. 

You cannot opt out of data tiering when selecting a node type from the r6gd family\. If you set the `--no-data-tiering-enabled` parameter, the operation will fail\.

For Linux, macOS, or Unix:

```
aws elasticache create-replication-group \
   --replication-group-id redis-dt-cluster \
   --replication-group-description "Redis cluster with data tiering" \
   --num-node-groups 1 \
   --replicas-per-node-group 1 \
   --cache-node-type cache.r6gd.xlarge \
   --engine redis \
   --engine-version 6.2 \
   --cache-subnet-group-name default \
   --automatic-failover-enabled \
   --data-tiering-enabled
```

For Windows:

```
aws elasticache create-replication-group ^
   --replication-group-id redis-dt-cluster ^
   --replication-group-description "Redis cluster with data tiering" ^
   --num-node-groups 1 ^
   --replicas-per-node-group 1 ^
   --cache-node-type cache.r6gd.xlarge ^
   --engine redis ^
   --engine-version 6.2 ^
   --cache-subnet-group-name default ^
   --automatic-failover-enabled ^
   --data-tiering-enabled
```

After running this operation, you will see a response similar to the following:

```
{
    "ReplicationGroup": {
        "ReplicationGroupId": "redis-dt-cluster",
        "Description": "Redis cluster with data tiering",
        "Status": "creating",           
        "PendingModifiedValues": {},
        "MemberClusters": [
            "redis-dt-cluster"
        ],
        "AutomaticFailover": "disabled",
        "DataTiering": "enabled",
        "SnapshotRetentionLimit": 0,
        "SnapshotWindow": "06:00-07:00",
        "ClusterEnabled": false,
        "CacheNodeType": "cache.r6gd.xlarge",       
        "TransitEncryptionEnabled": false,
        "AtRestEncryptionEnabled": false
    }
}
```

## Restoring data from backup into clusters with data tiering enabled<a name="data-tiering-enabling-snapshots"></a>

You can restore a backup to a new cluster with data tiering enabled using the \(Console\), \(AWS CLI\) or \(ElastiCache API\)\. When you create a cluster using node types in the r6gd family, data tiering is enabled\. 

### Restoring data from backup into clusters with data tiering enabled \(console\)<a name="data-tiering-enabling-snapshots-console"></a>

**To restore a backup to a new cluster with data tiering enabled \(console\)**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Backups**\.

1. In the list of backups, choose the box to the left of the backup name you want to restore from\.

1. Choose **Restore**\.

1. Complete the **Restore Cluster** dialog box\. Be sure to complete all the **Required** fields and any of the others you want to change from the defaults\.

   1. **Cluster ID** – Required\. The name of the new cluster\.

   1. **Cluster mode enabled \(scale out\)** – Choose this for a Redis \(cluster mode enabled\) cluster\. 

   1. **Node Type** – Specify **cache\.r6gd\.xlarge** or any other node type from the r6gd family\.

   1. **Number of Shards** – Choose the number of shards you want in the new cluster \(API/CLI: node groups\)\.

   1. **Replicas per Shard** – Choose the number of read replica nodes you want in each shard\.

   1. **Slots and keyspaces** – Choose how you want keys distributed among the shards\. If you choose to specify the key distributions complete the table specifying the key ranges for each shard\.

   1. **Availability zone\(s\)** – Specify how you want the cluster's Availability Zones selected\.

   1. **Port** – Change this only if you want the new cluster to use a different port\.

   1. **Choose a VPC** – Choose the VPC in which to create this cluster\.

   1. **Parameter Group** – Choose a parameter group that reserves sufficient memory for Redis overhead for the node type you selected\.

1. When the settings are as you want them, choose **Create**\.

For more information on creating a cluster, see [Creating a cluster](Clusters.Create.md)\.

### Restoring data from backup into clusters with data tiering enabled \(AWS CLI\)<a name="data-tiering-enabling-snapshots-cli"></a>

When creating a replication group using the AWS CLI, data tiering is by default used by selecting a node type from the r6gd family, such as *cache\.r6gd\.xlarge* and setting the `--data-tiering-enabled` parameter\. 

You cannot opt out of data tiering when selecting a node type from the r6gd family\. If you set the `--no-data-tiering-enabled` parameter, the operation will fail\.

For Linux, macOS, or Unix:

```
aws elasticache create-replication-group \
   --replication-group-id redis-dt-cluster \
   --replication-group-description "Redis cluster with data tiering" \
   --num-node-groups 1 \
   --replicas-per-node-group 1 \
   --cache-node-type cache.r6gd.xlarge \
   --engine redis \
   --engine-version 6.2 \
   --cache-subnet-group-name default \
   --automatic-failover-enabled \
   --data-tiering-enabled \
   --snapshot-name my-snapshot
```

For Linux, macOS, or Unix:

```
aws elasticache create-replication-group ^
   --replication-group-id redis-dt-cluster ^
   --replication-group-description "Redis cluster with data tiering" ^
   --num-node-groups 1 ^
   --replicas-per-node-group 1 ^
   --cache-node-type cache.r6gd.xlarge ^
   --engine redis ^
   --engine-version 6.2 ^
   --cache-subnet-group-name default ^
   --automatic-failover-enabled ^
   --data-tiering-enabled ^
   --snapshot-name my-snapshot
```

After running this operation, you will see a response similar to the following:

```
{
    "ReplicationGroup": {
        "ReplicationGroupId": "redis-dt-cluster",
        "Description": "Redis cluster with data tiering",
        "Status": "creating",           
        "PendingModifiedValues": {},
        "MemberClusters": [
            "redis-dt-cluster"
        ],
        "AutomaticFailover": "disabled",
        "DataTiering": "enabled",
        "SnapshotRetentionLimit": 0,
        "SnapshotWindow": "06:00-07:00",
        "ClusterEnabled": false,
        "CacheNodeType": "cache.r6gd.xlarge",        
        "TransitEncryptionEnabled": false,
        "AtRestEncryptionEnabled": false
    }
}
```