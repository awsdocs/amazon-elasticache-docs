# Creating a Redis \(cluster mode enabled\) Cluster \(Console\)<a name="Clusters.Create.CON.RedisCluster"></a>

If you are running Redis 3\.2\.4 or later, you can create a Redis \(cluster mode enabled\) cluster\. Redis \(cluster mode enabled\) clusters support partitioning your data across 1 to 15 shards \(API/CLI: node groups\) but with some limitations\. For a comparison of Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\), see [Supported ElastiCache for Redis Versions](supported-engine-versions.md)\.

You can create a Redis \(cluster mode enabled\) cluster \(API/CLI: replication group\) using the ElastiCache management console, the AWS CLI for ElastiCache, and the ElastiCache API\.

**To create a Redis \(cluster mode enabled\) cluster using the ElastiCache console**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the list in the upper\-right corner, choose the AWS Region you want to launch this cluster in\.

1. Choose **Redis** from the navigation pane\.

1. Choose **Create**\.

1. For **Cluster engine**, choose **Redis**, and then choose **Cluster Mode enabled \(Scale Out\)**\. These selections create a Redis \(cluster mode enabled\) cluster that looks something like the following\.  
![\[Image: Redis (cluster mode enabled) cluster created with replication and data partitioning\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCacheClusters-Redis-Clustering.png)

   *Redis \(cluster mode enabled\) cluster created with replication and data partitioning*

1. Complete the **Redis \(cluster mode enabled\) settings** section\.

   1. In the **Name** box, type a name for your cluster\.

**Cluster naming constraints**
      + Must contain from 1 to 20 alphanumeric characters or hyphens\.
      + Must begin with a letter\.
      + Cannot contain two consecutive hyphens\.
      + Cannot end with a hyphen\.

   1. In the **Description** box, type a description of the cluster\.

   1. If you want to enable in\-transit encryption for this cluster, choose **In\-transit encryption**\.

      If you choose **In\-transit encryption**, two additional options appear: **Redis auth token** and a box where you type in the token \(password\) value\.

   1. If you want to enable at\-rest encryption for this cluster, choose **At\-rest encryption**\.

   1. To require a password for operations to be performed on this cluster:

      1. Choose **Redis auth token\.**

      1. In the **Redis auth token** box, type the token \(password\) that must be used when performing operations on this cluster\.

   1. For **Engine version compatibility**, choose the ElastiCache for Redis engine version you want to run on this cluster\. Unless you have a specific reason to run an older version, we recommend that you choose the latest version\.

   1. To encrypt your data while it is in transit, for **Encryption**, choose **Yes**\.

   1. If you chose **Yes** for **Encryption**, you can require users to enter a password when executing Redis commands\. To require a password when executing commands, do the following:

      1. Choose **Yes** from the **AUTH** list\.

      1. Type in a password in the **AUTH token** box\.

   1. In the **Port** box, accept the default port, 6379\. If you have a reason to use a different port, type the port number\.

   1. For **Parameter group**, choose the parameter group you want to use with this cluster, or choose **Create new** to create a new parameter group to use with this cluster\.

      Parameter groups control the run\-time parameters of your cluster\. For more information on parameter groups, see [Redis Specific Parameters](ParameterGroups.Redis.md) and [Creating a Parameter Group](ParameterGroups.Creating.md)\.

   1. For **Node type**, choose the down arrow \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-DnArrow.png)\)\. In the **Change node type** dialog box, choose the **Instance family** of the node type you want, choose the node type you want to use for this cluster, and then choose **Save**\.

      For more information, see [Choosing Your Node Size](nodes-select-size.md#CacheNodes.SelectSize)\.

   1. For **Number of shards**, choose the number of shards \(partitions/node groups\) you want for this Redis \(cluster mode enabled\) cluster\.

      In Redis \(cluster mode enabled\), depending upon the version of Redis running on your cluster, you may be able to change the number of shards in your cluster dynamically\.
      + **Redis 3\.2\.10**—If your cluster is running Redis 3\.2\.10 you can change the number of shards in your cluster dynamically\. For more information, see [Scaling Redis \(cluster mode enabled\) Clusters](scaling-redis-cluster-mode-enabled.md)\.
      + **Other Redis versions**—If your cluster is running a version of Redis other than version 3\.2\.10, to change the number of shards in your cluster, you must create a new cluster with the new number of shards\. For more information, see [Restoring From a Backup with Optional Cluster Resizing](backups-restoring.md)\.

   1. For **Replicas per shard**, choose the number of read replica nodes you want in each shard\.

      The following restrictions exist for Redis \(cluster mode enabled\)\.
      + The number of replicas is the same for each shard when creating the cluster using the console\.

         
      + The number of read replicas per shard is fixed and cannot be changed\. If you find you need more or fewer replicas per shard \(API/CLI: node group\), you must create a new cluster with the new number of replicas\. For more information, see [Seeding a New Cluster with an Externally Created Backup](backups-seeding-redis.md)\.

   1. For **Subnet group**, choose the subnet you want to apply to this cluster\.

      For more information, see [Subnets and Subnet Groups](SubnetGroups.md)\.

1. Click **Advanced Redis settings** and complete the section\.

   1. For **Slots and keyspaces**, choose how you want your keys distributed over your shards \(partitions\)\. There are 16,384 keys to be distributed \(numbered 0 through 16383\)\.
      + **Equal distribution** – ElastiCache distributes your keyspace as equally as possible over your shards\.

         
      + **Custom distribution** – You specify the range of keys for each shard in the table below **Availability zone\(s\)**\.

   1. For **Availability zone\(s\)**, you have two options:
      + **No preference** – ElastiCache chooses the Availability Zone\.

         
      + **Specify availability zones** – You specify the Availability Zone for each cluster\.

        If you chose to specify the Availability Zones, for each cluster in each shard, choose the Availability Zone from the list\.

      For more information, see [Choosing Regions and Availability Zones](RegionsAndAZs.md)\.  
![\[Image: Specifying Keyspaces and Availability Zones\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-ClusterOn-Slots-AZs.png)

      *Specifying Keyspaces and Availability Zones*

   1. For **Security groups**, choose the security groups you want for this cluster\.

      For more information, see [Amazon VPCs and ElastiCache Security](VPCs.md)\.

   1. If you are going to seed your cluster with data from a \.RDB file, in the ** Seed RDB file S3 location** box, enter the S3 location of the \.RDB file\.

      For more information, see [Seeding a New Cluster with an Externally Created Backup](backups-seeding-redis.md)\.

      For Redis \(cluster mode enabled\) you must have a separate \.RDB file for each node group\.

   1. If you want regularly scheduled automatic backups, choose **Enable automatic backups** the type the number of days you want each automatic backup retained before it is automatically deleted\. If you don't want regularly scheduled automatic backups, clear the **Enable automatic backups** check box\. In either case, you always have the option to create manual backups\.

      For more information on Redis backup and restore, see [ElastiCache for Redis Backup and Restore](backups.md)\.

   1. The **Maintenance window** is the time, generally an hour in length, each week when ElastiCache schedules system maintenance for your cluster\. You can allow ElastiCache to choose the day and time for your maintenance window \(*No preference*\), or you can choose the day, time, and duration yourself \(*Specify maintenance window*\)\. If you choose *Specify maintenance window* from the lists, choose the *Start day*, *Start time*, and *Duration* \(in hours\) for your maintenance window\. All times are UCT times\.

      For more information, see [Managing Maintenance](maintenance-window.md)\.

   1. For **Notifications**, choose an existing Amazon Simple Notification Service \(Amazon SNS\) topic, or choose Manual ARN input and type in the topic's Amazon Resource Name \(ARN\)\. Amazon SNS allows you to push notifications to Internet\-connected smart devices\. The default is to disable notifications\. For more information, see [https://aws\.amazon\.com/sns/](https://aws.amazon.com/sns/)\.

1. Review all your entries and choices, then go back and make any needed corrections\. When you're ready, choose **Create cluster** to launch your cluster, or **Cancel** to cancel the operation\.

To create the equivalent using the ElastiCache API or AWS CLI instead of the ElastiCache console, see: 
+ API: [CreateReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)
+ CLI: [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

As soon as your cluster's status is *available*, you can grant EC2 access to it, connect to it, and begin using it\. For more information, see [Step 2: Authorize Access](GettingStarted.AuthorizeAccess.md) and [Step 3: Connect to a Cluster's Node](GettingStarted.ConnectToCacheNode.md)\.

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not actively using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a Cluster](Clusters.Delete.md)\. 