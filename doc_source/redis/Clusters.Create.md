# Creating a cluster<a name="Clusters.Create"></a>

The following examples show how to create a Redis cluster using the AWS Management Console, AWS CLI and ElastiCache API\.

## Creating a Redis \(cluster mode disabled\) \(Console\)<a name="Clusters.Create.CON.Redis"></a>

ElastiCache supports replication when you use the Redis engine\. To monitor the latency between when data is written to a Redis read/write primary cluster and when it is propagated to a read\-only secondary cluster, ElastiCache adds to the cluster a special key, `ElastiCacheMasterReplicationTimestamp`\. This key is the current Universal Universal Time \(UTC\) time\. Because a Redis cluster might be added to a replication group at a later time, this key is included in all Redis clusters, even if initially they are not members of a replication group\. For more information on replication groups, see [High availability using replication groups](Replication.md)\.

**To create a standalone Redis \(cluster mode disabled\) cluster**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the list in the upper\-right corner, choose the AWS Region that you want to launch this cluster in\.

1. Choose **Redis** from the navigation pane\.

1. Choose **Create**\.

1. For **Cluster engine**, choose **Redis**\. Make sure the **Cluster Mode enabled** check box is cleared\.

1. Complete the **Redis settings** section\.

   1. In **Name**, enter a name for your cluster\.

   1. In the **Description** box, enter a description for this cluster\.

   1. For **Engine version compatibility**, choose the ElastiCache for Redis engine version that you want to run on this cluster\. Unless you have a specific reason to run an older version, we recommend that you choose the latest version\.
**Important**  
You can upgrade to newer engine versions\. If you upgrade major engine versions, for example from 5\.0\.6 to 6\.x, you need to select a parameter group family that is comptabile with the new engine version\. For more information on doing so, see [Upgrading engine versions](VersionManagement.md)\. However, you can't downgrade to older engine versions except by deleting the existing cluster and creating it again\.

   1. In **Port**, use the default port, 6379\. If you have a reason to use a different port, enter the port number\.

   1. For **Parameter group**, choose a parameter group or create a new one\. Parameter groups control the runtime parameters of your cluster\. For more information on parameter groups, see [Redis\-specific parameters](ParameterGroups.Redis.md) and [Creating a parameter group](ParameterGroups.Creating.md)\.

   1. For **Node type**, click the down arrow \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-DnArrow.png)\)\. In the **Change node type** dialog box, choose the **Instance family** of the node type that you want, choose the node type that you want to use for this cluster, and then choose **Save**\.

      For more information, see [Choosing your node size](nodes-select-size.md#CacheNodes.SelectSize)\.

   1. For **Number of replicas**, choose the number of read replicas that you want for this cluster\.

      If you choose to have one or more replicas, the **Multi\-AZ** check box is available\. For more information, see [Mitigating Failures when Running Redis](FaultTolerance.md#FaultTolerance.Redis)\. We strongly suggest that you enable Multi\-AZ, which helps ensure your eligibility for service level agreement \(SLA\) compliance\. For more information on SLA compliance, see [Amazon ElastiCache Service Level Agreement](https://aws.amazon.com/elasticache/sla/) on the AWS website\.

      If you enter **0**, the **Multi\-AZ** check box is not enabled\. The cluster that you create looks like the following\. This is a Redis \(cluster mode disabled\) cluster with no replica nodes\.  
![\[Image: Redis (cluster mode disabled) cluster created with no replica nodes\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-Cluster-Redis-No-Replicas.png)

      If you choose one or more replicas, the cluster that you create looks something like the following\. This is a Redis \(cluster mode disabled\) cluster with replica nodes\.  
![\[Image: Redis (cluster mode disabled) cluster created with replica nodes\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCacheClusters-CSN-Redis-Replicas.png)

1. Choose **Advanced Redis settings** and complete the section\.

   1. For **Subnet group**, create a new subnet group or choose an existing one that you want to apply to this cluster\. If you enabled Multi\-AZ, the subnet group must contain at least two subnets that reside in different availability zones\.

      If you are using [Using local zones with ElastiCache ](Local_zones.md), you must create or choose a subnet that is in the local zone\. Multi\-AZ is automatically disabled\. Local Zones don't support global datastores at this time\.

      For more information, see [Subnets and subnet groups](SubnetGroups.md)\.

   1. For **Availability zone\(s\)**, you have two options:
      + **No preference** – ElastiCache chooses the Availability Zones for your cluster's nodes\.
      + **Specify availability zones** – Under **Under availability zones placement**, a list of your nodes appears allowing you to specify the Availability Zone for each node in your cluster\. You can choose a different Availability Zone from the list to the right of each node name\.
      + If you have Multi\-AZ enabled, you must place at least two nodes in different Availability Zones\.

      For more information, see [Choosing regions and availability zones](RegionsAndAZs.md)\.

   1. For **Security groups**, choose the security groups that you want for this cluster\. A security group acts as a firewall to control network access to your cluster\. You can choose use the [Default security group for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html#DefaultSecurityGroupdefault security group) or create a new one\.

      For more information, see [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)\.

   1. Encrypt your data; you have the following options:
      + **Encryption at rest** – Enables encryption of data stored on disk\. For more information, see [encryption at rest](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/at-rest-encryption.html)\.
**Note**  
You have the option to supply a different encryption key by choosing **Customer Managed Customer Master Key** and choosing the key\. For more information, see [Using Customer Managed CMKs from AWS KMS](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/at-rest-encryption.html#using-customer-managed-keys-for-elasticache-security)
      + **Encryption in\-transit** – Enables encryption of data on the wire\. For more information, see [encryption in transit](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/in-transit-encryption.html)\. For Redis engine version 6\.x, if you enable encryption in transit you're prompted to specify one of the following **Access Control** options:
        + **No Access Control** – This is the default setting\. This indicates no restrictions on user access to the cluster\.
        + **User Group Access Control List** – Choose a user group with a defined set of users that can access the cluster\. For more information, see [Managing User Groups with the Console and CLI](Clusters.RBAC.md#User-Groups)\.
        + **Redis AUTH Default User** – An authentication mechanism for Redis server\. For more information, see [Redis AUTH](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/auth.html)\.
**Note**  
For Redis versions between 3\.2\.6, when encryption in transit was first supported, and 6\.x, not including version 3\.2\.10, Redis AUTH is the sole option\.

   1. \(Optional\) For **Logs**:
      + Under **Log format**, choose either **Text** or **JSON**\.
      + Under **Destination Type**, choose either **CloudWatch Logs** or **Kinesis Firehose**\.
      + Under **Log destination**, choose either **Create new** and enter either your CloudWatch Logs log group name or your Kinesis Data Firehose stream name, or choose **Select existing** and then choose either your CloudWatch Logs log group name or your Kinesis Data Firehose stream name,

   1. \(Optional\) If you are going to seed your cluster with data from a \.rdb file, enter the Amazon S3 location of the \.rdb file for** Seed RDB file S3 location**\.

      For more information, see [Seeding a new cluster with an externally created backup](backups-seeding-redis.md)\.

   1. \(Optional\) For regularly scheduled automatic backups, choose **Enable automatic backups**, and then enter the number of days that you want an automatic backup retained before it is automatically deleted\. If you don't want regularly scheduled automatic backups, clear the **Enable automatic backups** check box\. In either case, you always have the option to create manual backups, which must be deleted manually\.

      For more information on Redis backup and restore, see [Backup and restore for ElastiCache for Redis ](backups.md)\.

   1. For **Maintenance window**, choose a maintenance window\. The *maintenance window* is the time, generally an hour in length, each week when ElastiCache schedules system maintenance for your cluster\. 

      You can enable ElastiCache to choose the day and time for your maintenance window \(**No preference**\)\. Or you can choose the day, time, and duration yourself \(**Specify maintenance window**\)\. If you choose **Specify maintenance window**, choose the **Start day**, **Start time**, and **Duration** \(in hours\) for your maintenance window\. All times are UTC times\.

      For more information, see [Managing maintenance](maintenance-window.md)\.

   1. For **Notifications**, choose an existing Amazon Simple Notification Service \(Amazon SNS\) topic, or choose manual ARN input and enter the topic Amazon Resource Name \(ARN\)\. If you use Amazon SNS, you can push notifications to internet\-connected smart devices\. The default is to disable notifications\. For more information, see [https://aws\.amazon\.com/sns/](https://aws.amazon.com/sns/)\.

1. Review all your entries and choices, then go back and make any needed corrections\. When you're ready, choose **Create** to launch your cluster\.

As soon as your cluster's status is *available*, you can grant Amazon EC2 access to it, connect to it, and begin using it\. For more information, see [Step 2: Authorize access to the cluster](GettingStarted.AuthorizeAccess.md) and [Step 3: Connect to the cluster's node](GettingStarted.ConnectToCacheNode.md)\.

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not actively using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a cluster](Clusters.Delete.md)\. 

## Creating a Redis \(cluster mode enabled\) cluster \(Console\)<a name="Clusters.Create.CON.RedisCluster"></a>

If you are running Redis 3\.2\.4 or later, you can create a Redis \(cluster mode enabled\) cluster\. Redis \(cluster mode enabled\) clusters support partitioning your data across 1 to 500 shards \(API/CLI: node groups\) but with some limitations\. For a comparison of Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\), see [Supported ElastiCache for Redis versions](supported-engine-versions.md)\.

You can create a Redis \(cluster mode enabled\) cluster \(API/CLI: replication group\) using the ElastiCache management console, the AWS CLI for ElastiCache, and the ElastiCache API\.

**To create a Redis \(cluster mode enabled\) cluster using the ElastiCache console**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the list in the upper\-right corner, choose the AWS Region that you want to launch this cluster in\.

1. Choose **Redis** from the navigation pane\.

1. Choose **Create**\.

1. For **Cluster engine**, choose **Redis**, and then choose **Cluster Mode enabled**\. These choices create a Redis \(cluster mode enabled\) cluster that looks something like the following\.  
![\[Image: Redis (cluster mode enabled) cluster created with replication and data partitioning\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCacheClusters-Redis-Clustering.png)

   *Redis \(cluster mode enabled\) cluster created with replication and data partitioning*

1. Complete the **Redis settings** section\.

   1. In **Name**, enter a name for your cluster\.

   1. In the **Description** box, enter a description for this cluster\.

   1. For **Engine version compatibility**, choose the ElastiCache for Redis engine version that you want to run on this cluster\. Unless you have a specific reason to run an older version, we recommend that you choose the latest version\.
**Important**  
For more information on doing so, see [Upgrading engine versions](VersionManagement.md)\. However, you can't downgrade to older engine versions except by deleting the existing cluster and creating it again\.

   1. In **Port**, use the default port, 6379\. If you have a reason to use a different port, enter the port number\.

   1. For **Parameter group**, choose a parameter group or create a new one\. Parameter groups control the runtime parameters of your cluster\. For more information on parameter groups, see [Redis\-specific parameters](ParameterGroups.Redis.md) and [Creating a parameter group](ParameterGroups.Creating.md)\.

   1. For **Node type**, choose the down arrow \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-DnArrow.png)\)\. In the **Change node type** dialog box, choose a value for **Instance family** for the node type that you want\. Then choose the node type that you want to use for this cluster, and then choose **Save**\.

      For more information, see [Choosing your node size](nodes-select-size.md#CacheNodes.SelectSize)\.

   1. For **Number of shards**, choose the number of shards \(partitions/node groups\) that you want for this Redis \(cluster mode enabled\) cluster\.

      For some versions of Redis \(cluster mode enabled\), you can change the number of shards in your cluster dynamically:
      + **Redis 3\.2\.10 and later** – If your cluster is running Redis 3\.2\.10 or later versions, you can change the number of shards in your cluster dynamically\. For more information, see [Scaling clusters in Redis \(Cluster Mode Enabled\)](scaling-redis-cluster-mode-enabled.md)\.
      + **Other Redis versions** – If your cluster is running a version of Redis before version 3\.2\.10, there's another approach\. To change the number of shards in your cluster in this case, create a new cluster with the new number of shards\. For more information, see [Restoring from a backup with optional cluster resizing](backups-restoring.md)\.

   1. For **Replicas per shard**, choose the number of read replica nodes that you want in each shard\.

      The following restrictions exist for Redis \(cluster mode enabled\)\.
      + If you have Multi\-AZ enabled, make sure that you have at least one replica per shard\.
      + The number of replicas is the same for each shard when creating the cluster using the console\.
      + The number of read replicas per shard is fixed and cannot be changed\. If you find you need more or fewer replicas per shard \(API/CLI: node group\), you must create a new cluster with the new number of replicas\. For more information, see [Seeding a new cluster with an externally created backup](backups-seeding-redis.md)\.

   1. For **Subnet group**, create a new subnet group or choose an existing one that you want to apply to this cluster\. If you enabled Multi\-AZ, the subnet group must contain at least two subnets that reside in different availability zones\.

      If you are using [Using local zones with ElastiCache ](Local_zones.md), you must create or choose a subnet that is in the local zone\. Multi\-AZ is automatically disabled\. Local Zones don't support global datastores at this time\.

      For more information, see [Subnets and subnet groups](SubnetGroups.md)\.

1. Click **Advanced Redis settings** and complete the section\.

   1. For **Slots and keyspaces**, choose how you want your keys distributed over your shards \(partitions\)\. There are 16,384 keys to be distributed \(numbered 0 through 16383\)\.
      + **Equal distribution** – ElastiCache distributes your keyspace as equally as possible over your shards\.

         
      + **Custom distribution** – You specify the range of keys for each shard in the table below **Availability zone\(s\)**\.

   1. For **Availability zone\(s\)**, you have two options:
      + **No preference** – ElastiCache chooses the Availability Zone\.

         
      + **Specify availability zones** – You specify the Availability Zone for each cluster\.

        If you chose to specify the Availability Zones, for each cluster in each shard, choose the Availability Zone from the list\.

      For more information, see [Choosing regions and availability zones](RegionsAndAZs.md)\.  
![\[Image: Specifying Keyspaces and Availability Zones\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-ClusterOn-Slots-AZs.png)

      *Specifying Keyspaces and Availability Zones*

   1. For **Security groups**, choose the security groups that you want for this cluster\. A *security group* acts as a firewall to control network access to your cluster\. You can use the default security group for your VPC or create a new one\.

      For more information on security groups, see [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide\.*

   1. Encrypt your data, you have the following options:
      + **Encryption at rest** – Enables encryption of data stored on disk\. For more information, see [Encryption at Rest](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/at-rest-encryption.html)\.
**Note**  
You have the option to supply a different encryption key by choosing **Customer Managed Customer Master Key** and choosing the key\. For more information, see [Using Customer Managed CMKs from AWS KMS](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/at-rest-encryption.html#using-customer-managed-keys-for-elasticache-security)\.
      + **Encryption in\-transit** – Enables encryption of data on the wire\. For more information, see [encryption in transit](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/in-transit-encryption.html)\. For Redis engine version 6\.x and above, if you enable Encryption in\-transit you will be prompted to specify one of the following **Access Control** options:
        + **No Access Control** – This is the default setting\. This indicates no restrictions on user access to the cluster\.
        + **User Group Access Control List** – Select a user group with a defined set of users that can access the cluster\. For more information, see [Managing User Groups with the Console and CLI](Clusters.RBAC.md#User-Groups)\.
        + **Redis AUTH Default User** – An authentication mechanism for Redis server\. For more information, see [Redis AUTH](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/auth.html)\.
      + **Redis AUTH** – An authentication mechanism for Redis server\. For more information, see [Redis AUTH](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/auth.html)\.
**Note**  
For Redis versions between 3\.2\.6, when Encryption in\-transit was first supported, and 6\.x, Redis AUTH is the sole option\.

   1. \(Optional\) For **Logs**:
      + Under **Log format**, choose either **Text** or **JSON**\.
      + Under **Destination Type**, choose either **CloudWatch Logs** or **Kinesis Firehose**\.
      + Under **Log destination**, choose either **Create new** and enter either your CloudWatch Logs log group name or your Kinesis Data Firehose stream name, or choose **Select existing** and then choose either your CloudWatch Logs log group name or your Kinesis Data Firehose stream name,

   1. \(Optional\) If you are going to seed your cluster with data from a \.rdb file, enter the S3 location of the \.rdb file for** Seed RDB file S3 location**\.

      For more information, see [Seeding a new cluster with an externally created backup](backups-seeding-redis.md)\.

      For Redis \(cluster mode enabled\), make sure that you have a separate \.rdb file for each node group\.

   1. For regularly scheduled automatic backups, select **Enable automatic backups** and then enter the number of days that you want each automatic backup retained before it is automatically deleted\. If you don't want regularly scheduled automatic backups, clear the **Enable automatic backups** check box\. In either case, you always have the option to create manual backups\.

      For more information on Redis backup and restore, see [Backup and restore for ElastiCache for Redis ](backups.md)\.

   1. \(Optional\_ Specify a maintenance window\. The *maintenance window* is the time, generally an hour in length, each week when ElastiCache schedules system maintenance for your cluster\. You can allow ElastiCache to choose the day and time for your maintenance window \(*No preference*\), or you can choose the day, time, and duration yourself \(*Specify maintenance window*\)\. If you choose *Specify maintenance window* from the lists, choose the *Start day*, *Start time*, and *Duration* \(in hours\) for your maintenance window\. All times are UCT times\.

      For more information, see [Managing maintenance](maintenance-window.md)\.

   1. For **Notifications**, choose an existing Amazon Simple Notification Service \(Amazon SNS\) topic, or choose Manual ARN input and enter the topic's Amazon Resource Name \(ARN\)\. Amazon SNS allows you to push notifications to Internet\-connected smart devices\. The default is to disable notifications\. For more information, see [https://aws\.amazon\.com/sns/](https://aws.amazon.com/sns/)\.

1. Review all your entries and choices, then make any needed corrections\. When you're ready, choose **Create cluster** to launch your cluster, or **Cancel** to cancel the operation\.

To create the equivalent using the ElastiCache API or AWS CLI instead of the ElastiCache console, see the following: 
+ API: [CreateReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)
+ CLI: [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

As soon as your cluster's status is *available*, you can grant EC2 access to it, connect to it, and begin using it\. For more information, see [Step 2: Authorize access to the cluster](GettingStarted.AuthorizeAccess.md) and [Step 3: Connect to the cluster's node](GettingStarted.ConnectToCacheNode.md)\.

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not actively using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a cluster](Clusters.Delete.md)\. 

## Creating a cluster \(AWS CLI\)<a name="Clusters.Create.CLI"></a>

To create a cluster using the AWS CLI, use the `create-cache-cluster` command\.

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not actively using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a cluster](Clusters.Delete.md)\. 

### Creating a cache cluster for Redis \(cluster mode disabled\) \(AWS CLI\)<a name="Clusters.Create.CLI.Redis"></a>

**Example – A Redis \(cluster mode disabled\) Cluster with no read replicas**  
The following CLI code creates a Redis \(cluster mode disabled\) cache cluster with no replicas\.  
For Linux, macOS, or Unix:  

```
aws elasticache create-cache-cluster \
--cache-cluster-id my-cluster \
--cache-node-type cache.r4.large \
--engine redis \
--engine-version 3.2.4 \
--num-cache-nodes 1 \
--cache-parameter-group default.redis3.2 \
--snapshot-arns arn:aws:s3:::my_bucket/snapshot.rdb
```
For Windows:  

```
aws elasticache create-cache-cluster ^
--cache-cluster-id my-cluster ^
--cache-node-type cache.r4.large ^
--engine redis ^
--engine-version 3.2.4 ^
--num-cache-nodes 1 ^
--cache-parameter-group default.redis3.2 ^
--snapshot-arns arn:aws:s3:::my_bucket/snapshot.rdb
```

### Creating a Redis \(cluster mode enabled\) cluster \(AWS CLI\)<a name="Clusters.Create.CLI.RedisCluster"></a>

Redis \(cluster mode enabled\) clusters \(API/CLI: replication groups\) cannot be created using the `create-cache-cluster` operation\. To create a Redis \(cluster mode enabled\) cluster \(API/CLI: replication group\), see [Creating a Redis \(Cluster Mode Enabled\) replication group from scratch \(AWS CLI\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.CLI)\.

For more information, see the AWS CLI for ElastiCache reference topic [https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)\.

## Creating a cluster \(ElastiCache API\)<a name="Clusters.Create.API"></a>

To create a cluster using the ElastiCache API, use the `CreateCacheCluster` action\. 

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a cluster](Clusters.Delete.md)\. 

**Topics**
+ [Creating a Redis \(cluster mode disabled\) cache cluster \(ElastiCache API\)](#Clusters.Create.API.Redis)
+ [Creating a cache cluster in Redis \(cluster mode enabled\) \(ElastiCache API\)](#Clusters.Create.API.RedisCluster)

### Creating a Redis \(cluster mode disabled\) cache cluster \(ElastiCache API\)<a name="Clusters.Create.API.Redis"></a>

The following code creates a Redis \(cluster mode disabled\) cache cluster \(ElastiCache API\)\.

Line breaks are added for ease of reading\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=CreateCacheCluster
    &CacheClusterId=my-cluster
    &CacheNodeType=cache.r4.large
    &CacheParameterGroup=default.redis3.2
    &Engine=redis
    &EngineVersion=3.2.4
    &NumCacheNodes=1
    &SignatureVersion=4       
    &SignatureMethod=HmacSHA256
    &SnapshotArns.member.1=arn%3Aaws%3As3%3A%3A%3AmyS3Bucket%2Fdump.rdb
    &Timestamp=20150508T220302Z
    &Version=2015-02-02
    &X-Amz-Algorithm=AWS4-HMAC-SHA256
    &X-Amz-Credential=<credential>
    &X-Amz-Date=20150508T220302Z
    &X-Amz-Expires=20150508T220302Z
    &X-Amz-SignedHeaders=Host
    &X-Amz-Signature=<signature>
```

### Creating a cache cluster in Redis \(cluster mode enabled\) \(ElastiCache API\)<a name="Clusters.Create.API.RedisCluster"></a>

Redis \(cluster mode enabled\) clusters \(API/CLI: replication groups\) cannot be created using the `CreateCacheCluster` operation\. To create a Redis \(cluster mode enabled\) cluster \(API/CLI: replication group\), see [Creating a replication group in Redis \(Cluster Mode Enabled\) from scratch \(ElastiCache API\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.API)\.

For more information, see the ElastiCache API reference topic [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)\.