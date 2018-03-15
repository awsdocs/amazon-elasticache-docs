# Creating a Redis \(cluster mode disabled\) Cluster \(Console\)<a name="Clusters.Create.CON.Redis"></a>

ElastiCache supports replication when you use the Redis engine\. To monitor the latency between when data is written to a Redis read/write primary cluster and when it is propagated to a read\-only secondary cluster, ElastiCache adds to the cluster a special key, `ElastiCacheMasterReplicationTimestamp`\. This key is the current Universal Coordinated Time \(UCT\) time\. Because a Redis cluster might be added to a replication group at a later time, this key is included in all Redis clusters, even if initially they are not members of a replication group\. For more information on replication groups, see [ElastiCache Replication \(Redis\)](Replication.md)\.

**To create a standalone Redis \(cluster mode disabled\) cluster**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the dropdown in the upper right corner, choose the region you want to launch this cluster in\.

1. Choose **Redis** from the navigation pane\.

1. Choose **Create**\.

1. For **Cluster engine**, choose **Redis**, and then clear the **Cluster Mode enabled \(Scale Out\)** check box\.

1. Complete the **Redis settings** section\.

   1. In **Name**, type a name for your cluster\.

**Cluster naming constraints**

      + Must contain from 1 to 20 alphanumeric characters or hyphens\.

      + Must begin with a letter\.

      + Cannot contain two consecutive hyphens\.

      + Cannot end with a hyphen\.

   1. In the **Description** box, type in a description for this cluster\.

   1. For **Engine version compatibility**, choose the ElastiCache for Redis engine version you want to run on this cluster\. Unless you have a specific reason to run an older version, we recommend that you choose the latest version\.
**Important**  
You can upgrade to newer engine versions \(see [Upgrading Engine Versions](VersionManagement.md)\), but you cannot downgrade to older engine versions except by deleting the existing Cluster and creating it anew\.

      Because the newer Redis versions provide a better and more stable user experience, Redis versions 2\.6\.13, 2\.8\.6, and 2\.8\.19 are deprecated when using the ElastiCache console\. We recommend against using these Redis versions\. If you need to use one of them, work with the AWS CLI or ElastiCache API\.

      For more information, see the following topics:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/Clusters.Create.CON.Redis.html)

   1. To encrypt your data while it is in transit, for **Encryption**, choose **Yes**\.

   1. If you chose **Yes** for **Encryption**, you can require users to enter a password when executing Redis commands\. To require a password when executing commands, do the following:

      1. Choose **Yes** from the **AUTH** list\.

      1. Type in a password in the **AUTH token** box:

**AUTH Token Constraints when using with ElastiCache**

         + Passwords must be at least 16 and a maximum of 128 printable characters\.

         + The printable characters `@`, `"`, and `/` cannot be used in passwords\.

         + AUTH can only be enabled when creating clusters where in\-transit encryption is enabled\.

         + The password set at cluster creation cannot be changed\.

         We recommend that you follow a stricter policy such as:

         + Must include a mix of characters that includes at least three of the following character types:

           + Uppercase characters

           + Lowercase characters

           + Digits 

           + Non\-alphanumeric characters \(`!`, `&`, `#`, `$`, `^`, `<`, `>`, `-`\)

         + Must not contain a dictionary word or a slightly modified dictionary word\.

         + Must not be the same as or similar to a recently used password\.

   1. In **Port**, accept the default port, 6379\. If you have a reason to use a different port, type the port number\.

   1. For **Parameter group**, choose the parameter group you want to use with this cluster, or choose *Create new* to create a new parameter group to use with this cluster\.

      Parameter groups control the runtime parameters of your cluster\. For more information on parameter groups, see [Redis Specific Parameters](ParameterGroups.Redis.md) and [Creating a Parameter Group](ParameterGroups.Creating.md)\.

   1. For **Node type**, click the down arrow \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-DnArrow.png)\)\. In the **Change node type** dialog box, choose the **Instance family** of the node type you want, choose the node type you want to use for this cluster, and then choose **Save**\.

      For more information, see [Choosing Your Node Size](CacheNodes.SelectSize.md)\.

   1. For **Number of replicas**, choose the number of read replicas you want for this cluster\.

      If you choose **None**, the **description** and **Multi\-AZ with Auto\-Failover** fields disappear and the cluster your create look like the following\.  
![\[Image: Redis (cluster mode disabled) cluster created with no replica nodes\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-Cluster-Redis-No-Replicas.png)

      *Redis \(cluster mode disabled\) cluster created with no replica nodes*

      If you choose one or more replicas, the cluster you create looks something like the following\.  
![\[Image: Redis (cluster mode disabled) cluster created with replica nodes\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCacheClusters-CSN-Redis-Replicas.png)

      *Redis \(cluster mode disabled\) cluster created with replica nodes*

1. Choose **Advanced Redis settings** and complete the section\.

   1. If you chose to have one or more replicas, the **Multi\-AZ with Auto\-Failover** check box is available\. We strongly suggest that you enable Multi\-AZ with Auto\-Failover\. For more information, see [Mitigating Failures when Running Redis](FaultTolerance.md#FaultTolerance.Redis)\.

   1. For **Subnet group**, choose the subnet you want to apply to this cluster\.

      For more information, see [Subnets and Subnet Groups](SubnetGroups.md)\.

   1. For **Availability zone\(s\)**, you have two options:

      + **No preference** – ElastiCache chooses the Availability Zones for your cluster's nodes\.

      + **Specify availability zones** – A list of your nodes appears allowing you to specify the Availability Zone for each node in your cluster by choosing the Availability Zone from the list to the right of each node name\.

      For more information, see [Choosing Regions and Availability Zones](RegionsAndAZs.md)\.

   1. For **Security groups**, choose the security groups you want for this cluster\.

      For more information, see [ElastiCache and Security Groups](VPCs.md)\.

   1. If you are going to seed your cluster with data from a \.RDB file, in the ** Seed RDB file S3 location** box, type the Amazon S3 location of the \.RDB file\.

      For more information, see [Seeding a New Cluster with an Externally Created Backup \(Redis\)](backups-seeding-redis.md)\.

   1. If you want regularly scheduled automatic backups, choose **Enable automatic backups**, and then type the number of days you want an automatic backup retained before it is automatically deleted\. If you don't want regularly scheduled automatic backups, clear the **Enable automatic backups** check box\. In either case, you always have the option to create manual backups, which must be deleted manually\.

      For more information on Redis backup and restore, see [ElastiCache Backup and Restore \(Redis\)](backups.md)\.

   1. The **Maintenance window** is the time, generally an hour in length, each week when ElastiCache schedules system maintenance for your cluster\. You can allow ElastiCache to choose the day and time for your maintenance window \(**No preference**\), or you can choose the day, time, and duration yourself \(**Specify maintenance window**\)\. If you choose **Specify maintenance window**, choose the **Start day**, **Start time**, and **Duration** \(in hours\) for your maintenance window\. All times are UCT times\.

      For more information, see [Maintenance Window](VersionManagement.MaintenanceWindow.md)\.

   1. For **Notifications**, choose an existing Amazon Simple Notification Service \(Amazon SNS\) topic, or choose manual ARN input and type in the topic Amazon Resource Name \(ARN\)\. Amazon SNS allows you to push notifications to Internet\-connected smart devices\. The default is to disable notifications\. For more information, see [https://aws\.amazon\.com/sns/](https://aws.amazon.com/sns/)\.

1. Review all your entries and choices, then go back and make any needed corrections\. When you're ready to launch your cluster, choose **Create**\.

As soon as your cluster's status is *available*, you can grant Amazon EC2 access to it, connect to it, and begin using it\. For more information, see [Step 4: Authorize Access](GettingStarted.AuthorizeAccess.md) and [Step 5: Connect to a Cluster's Node](GettingStarted.ConnectToCacheNode.md)\.

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not actively using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a Cluster](Clusters.Delete.md)\. 
