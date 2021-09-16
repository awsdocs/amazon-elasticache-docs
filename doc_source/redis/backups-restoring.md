# Restoring from a backup with optional cluster resizing<a name="backups-restoring"></a>

You can restore the data from a Redis \.rdb backup file to a new cluster at any time\.

The Amazon ElastiCache for Redis restore process supports the following:
+ Upgrading from a Redis \(cluster mode disabled\) cluster to a Redis \(cluster mode enabled\) cluster running Redis version 3\.2\.4\.
+ Migrating from one or more \.rdb backup files you created from your self\-managed Redis clusters to a single ElastiCache for Redis \(cluster mode enabled\) cluster\.

  The \.rdb files must be put in S3 to perform the restore\.
+ Specifying a number of shards \(API/CLI: node groups\) in the new cluster that is different from the number of shards in the cluster that was used to create the backup file\.
+ Specifying a different node type for the new cluster—larger or smaller\. If scaling to a smaller node type, be sure that the new node type has sufficient memory for your data and Redis overhead\. For more information, see [Choosing your node size](nodes-select-size.md#CacheNodes.SelectSize)\.
+ Configuring the slots of the new Redis \(cluster mode enabled\) cluster differently than in the cluster that was used to create the backup file\.

**Important**  
You cannot restore from a backup created using a Redis \(cluster mode enabled\) cluster to a Redis \(cluster mode disabled\) cluster\.
Redis \(cluster mode enabled\) clusters do not support multiple databases\. Therefore, when restoring to a Redis \(cluster mode enabled\) your restore fails if the \.rdb file references more than one database\.

Whether you make any changes when restoring a cluster from a backup is governed by choices that you make\. You make these choices in the **Restore Cluster** dialog box when using the ElastiCache console to restore\. You make these choices by setting parameter values when using the AWS CLI or ElastiCache API to restore\.

During the restore operation, ElastiCache creates the new cluster, and then populates it with data from the backup file\. When this process is complete, the Redis cluster is warmed up and ready to accept requests\.

**Important**  
Before you proceed, be sure you have created a backup of the cluster you want to restore from\. For more information, see [Making manual backups](backups-manual.md)\.   
If you want to restore from an externally created backup, see [Seeding a new cluster with an externally created backup](backups-seeding-redis.md)\.

The following procedures show you how to restore a backup to a new cluster using the ElastiCache console, the AWS CLI, or the ElastiCache API\.

## Restoring from a backup \(Console\)<a name="backups-restoring-CON"></a>

You can restore a Redis backup in two ways\. You can restore to a single\-node Redis \(cluster mode disabled\) cluster\. Or you can restore to a Redis cluster with read replicas \(a replication group\), either Redis \(cluster mode disabled\) or Redis \(cluster mode enabled\)\.

**To restore a backup to a new cluster \(console\)**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Backups**\.

1. In the list of backups, choose the box to the left of the backup name you want to restore from\.

1. Choose **Restore**\.

1. Complete the **Restore Cluster** dialog box\. Be sure to complete all the "Required" fields and any of the others you want to change from the defaults\.

**Redis \(Cluster Mode Disabled\)**

   1. **Cluster ID** – Required\. The name of the new cluster\.

   1. **Engine version compatibility** – The ElastiCache for Redis engine version you want to run\.

   1. **Cluster mode enabled \(scale out\)** – Choose this to convert your Redis \(cluster mode disabled\) cluster to a Redis \(cluster mode enabled\)\. The engine version becomes 3\.2\.4\.

      If you choose **Cluster mode enabled \(scale out\)**:

      1. Choose the number of shards you want in the new cluster \(API/CLI: node groups\)\.

      1. Choose the number of read replicas you want in each shard\.

      1. Distribute your keys among the slots as you desire\.

   1. **Node Type** – Specify the node type you want for the new cluster\.

   1. **Availability zone\(s\)** – Specify how you want the cluster's Availability Zones selected\.

   1. **Port** – Change this only if you want the new cluster to use a different port\.

   1. **Choose a VPC** – Choose the VPC in which to create this cluster\.

   1. **Parameter Group** – Choose a parameter group that reserves sufficient memory for Redis overhead for the node type you selected\.

    

**Redis \(Cluster Mode Enabled\)**

   1. **Cluster ID** – Required\. The name of the new cluster\.

   1. **Cluster mode enabled \(scale out\)** – Choose this for a Redis \(cluster mode enabled\) cluster\. Clear it for a Redis \(cluster mode disabled\) cluster\.

   1. **Node Type** – Specify the node type you want for the new cluster\.

   1. **Number of Shards** – Choose the number of shards you want in the new cluster \(API/CLI: node groups\)\.

   1. **Replicas per Shard** – Choose the number of read replica nodes you want in each shard\.

   1. **Slots and keyspaces** – Choose how you want keys distributed among the shards\. If you choose to specify the key distributions complete the table specifying the key ranges for each shard\.

   1. **Availability zone\(s\)** – Specify how you want the cluster's Availability Zones selected\.

   1. **Port** – Change this only if you want the new cluster to use a different port\.

   1. **Choose a VPC** – Choose the VPC in which to create this cluster\.

   1. **Parameter Group** – Choose a parameter group that reserves sufficient memory for Redis overhead for the node type you selected\.

1. When the settings are as you want them, choose **Create**\.

## Restoring from a backup \(AWS CLI\)<a name="backups-restoring-CLI"></a>

You can restore a Redis \(cluster mode disabled\) backup in two ways\. You can restore to a single\-node Redis \(cluster mode disabled\) cluster using the AWS CLI operation `create-cache-cluster`\. Or you can restore to a Redis cluster with read replicas \(a replication group\)\. To do the latter, you can use either Redis \(cluster mode disabled\) or Redis \(cluster mode enabled\) with the AWS CLI operation `create-replication-group`\. In this case, you seed the restore with a Redis \.rdb file\.

When using either the `create-cache-cluster` or `create-replication-group` operation, be sure to include the parameter `--snapshot-name` or `--snapshot-arns` to seed the new cluster or replication group with the data from the backup\.

For more information, see the following:
+ [Creating a cluster \(AWS CLI\)](Clusters.Create.md#Clusters.Create.CLI) in the *ElastiCache User Guide*\.
+ [create\-cache\-cluster](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-cache-cluster.html) in the *AWS CLI Command Reference*\.

   
+ [Creating a Redis replication group from scratch](Replication.CreatingReplGroup.NoExistingCluster.md) in the *ElastiCache User Guide*\.
+ [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html) in the *AWS CLI Command Reference*\.

## Restoring from a backup \(ElastiCache API\)<a name="backups-restoring-API"></a>

You can restore a Redis backup to either a single\-node Redis \(cluster mode disabled\) cluster using the ElastiCache API operation `CreateCacheCluster` or to a Redis cluster with read replicas \(replication group\)— either Redis \(cluster mode disabled\) or Redis \(cluster mode enabled\) using the ElastiCache API operation `CreateReplicationGroup` and seeding it with a Redis \.rdb file\.

When using either the `CreateCacheCluster` or `CreateReplicationGroup` operation, be sure to include the parameter `SnapshotName` or `SnapshotArns` to seed the new cluster or replication group with the data from the backup\.

For more information, see the following:
+ [Creating a cluster \(ElastiCache API\)](Clusters.Create.md#Clusters.Create.API) in the *ElastiCache User Guide*\.
+ [CreateCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheCluster.html) in the *ElastiCache API Reference*\.

   
+ [Creating a Redis replication group from scratch](Replication.CreatingReplGroup.NoExistingCluster.md) in the *ElastiCache User Guide*\.
+ [CreateReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html) in the *ElastiCache API Reference*\.

 