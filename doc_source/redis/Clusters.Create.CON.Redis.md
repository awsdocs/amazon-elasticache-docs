# Creating a Redis \(cluster mode disabled\) Cluster \(Console\)<a name="Clusters.Create.CON.Redis"></a>

ElastiCache supports replication when you use the Redis engine\. To monitor the latency between when data is written to a Redis read/write primary cluster and when it is propagated to a read\-only secondary cluster, ElastiCache adds to the cluster a special key, `ElastiCacheMasterReplicationTimestamp`\. This key is the current Universal Universal Time \(UTC\) time\. Because a Redis cluster might be added to a replication group at a later time, this key is included in all Redis clusters, even if initially they are not members of a replication group\. For more information on replication groups, see [High Availability Using Replication Groups](Replication.md)\.

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
You can upgrade to newer engine versions\. For more information on doing so, see [Upgrading Engine Versions](VersionManagement.md)\. However, you can't downgrade to older engine versions except by deleting the existing cluster and creating it again\.

   1. In **Port**, use the default port, 6379\. If you have a reason to use a different port, enter the port number\.

   1. For **Parameter group**, choose a parameter group or create a new one\. Parameter groups control the runtime parameters of your cluster\. For more information on parameter groups, see [Redis Specific Parameters](ParameterGroups.Redis.md) and [Creating a Parameter Group](ParameterGroups.Creating.md)\.

   1. For **Node type**, click the down arrow \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-DnArrow.png)\)\. In the **Change node type** dialog box, choose the **Instance family** of the node type that you want, choose the node type that you want to use for this cluster, and then choose **Save**\.

      For more information, see [Choosing Your Node Size](nodes-select-size.md#CacheNodes.SelectSize)\.

   1. For **Number of replicas**, choose the number of read replicas that you want for this cluster\.

      If you choose to have one or more replicas, the **Multi\-AZ** check box is available\. For more information, see [Mitigating Failures when Running Redis](FaultTolerance.md#FaultTolerance.Redis)\. We strongly suggest that you enable Multi\-AZ, which helps ensure your eligibility for service level agreement \(SLA\) compliance\. For more information on SLA compliance, see [Amazon ElastiCache Service Level Agreement](https://aws.amazon.com/elasticache/sla/) on the AWS website\.

      If you enter **0**, the **Multi\-AZ** check box is not enabled\. The cluster that you create looks like the following\. This is a Redis \(cluster mode disabled\) cluster with no replica nodes\.  
![\[Image: Redis (cluster mode disabled) cluster created with no replica nodes\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-Cluster-Redis-No-Replicas.png)

      If you choose one or more replicas, the cluster that you create looks something like the following\. This is a Redis \(cluster mode disabled\) cluster with replica nodes\.  
![\[Image: Redis (cluster mode disabled) cluster created with replica nodes\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCacheClusters-CSN-Redis-Replicas.png)

1. Choose **Advanced Redis settings** and complete the section\.

   1. For **Subnet group**, create a new subnet group or choose an existing one that you want to apply to this cluster\. If you enabled Multi\-AZ, the subnet group must contain at least two subnets that reside in different availability zones\.

      For more information, see [Subnets and Subnet Groups](SubnetGroups.md)\.

   1. For **Availability zone\(s\)**, you have two options:
      + **No preference** – ElastiCache chooses the Availability Zones for your cluster's nodes\.
      + **Specify availability zones** – Under **Under availability zones placement**, a list of your nodes appears allowing you to specify the Availability Zone for each node in your cluster\. You can choose a different Availability Zone from the list to the right of each node name\.
      + If you have Multi\-AZ enabled, you must place at least two nodes in different Availability Zones\.

      For more information, see [Choosing Regions and Availability Zones](RegionsAndAZs.md)\.

   1. For **Security groups**, choose the security groups that you want for this cluster\. A security group acts as a firewall to control network access to your cluster\. You can choose use the [Default security group for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html#DefaultSecurityGroupdefault security group) or create a new one\.

      For more information, see [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)\.

   1. Encrypt your data, you have the following options:
      + **Encryption at rest** – Enables encryption of data stored on disk\. For more information, see [encryption at rest](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/at-rest-encryption.html)\.
**Note**  
You have the option to supply a different encryption key by selecting **Customer Managed Customer Master Key** and selecting the key\. For more information, see [Using Customer Managed CMKs from AWS KMS](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/at-rest-encryption.html#using-customer-managed-keys-for-elasticache-security)
      + **Encryption in\-transit** – Enables encryption of data on the wire\. For more information, see [encryption in transit](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/in-transit-encryption.html)\. 
      + **Redis AUTH** – An authentication mechanism for Redis server\. You can supply a different Redis AUTH token\. For more information, see [Redis AUTH](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/auth.html)\.

   1. \(Optional\) If you are going to seed your cluster with data from a \.rdb file, enter the Amazon S3 location of the \.rdb file for** Seed RDB file S3 location**\.

      For more information, see [Seeding a New Cluster with an Externally Created Backup](backups-seeding-redis.md)\.

   1. If you want regularly scheduled automatic backups, choose **Enable automatic backups**, and then enter the number of days that you want an automatic backup retained before it is automatically deleted\. If you don't want regularly scheduled automatic backups, clear the **Enable automatic backups** check box\. In either case, you always have the option to create manual backups, which must be deleted manually\.

      For more information on Redis backup and restore, see [Backup and Restore for ElastiCache for Redis ](backups.md)\.

   1. The **Maintenance window** is the time, generally an hour in length, each week when ElastiCache schedules system maintenance for your cluster\. You can allow ElastiCache to choose the day and time for your maintenance window \(**No preference**\), or you can choose the day, time, and duration yourself \(**Specify maintenance window**\)\. If you choose **Specify maintenance window**, choose the **Start day**, **Start time**, and **Duration** \(in hours\) for your maintenance window\. All times are UTC times\.

      For more information, see [Managing Maintenance](maintenance-window.md)\.

   1. For **Notifications**, choose an existing Amazon Simple Notification Service \(Amazon SNS\) topic, or choose manual ARN input and enter the topic Amazon Resource Name \(ARN\)\. Amazon SNS allows you to push notifications to Internet\-connected smart devices\. The default is to disable notifications\. For more information, see [https://aws\.amazon\.com/sns/](https://aws.amazon.com/sns/)\.

1. Review all your entries and choices, then go back and make any needed corrections\. When you're ready, choose **Create** to launch your cluster\.

As soon as your cluster's status is *available*, you can grant Amazon EC2 access to it, connect to it, and begin using it\. For more information, see [Authorize Access](GettingStarted.AuthorizeAccess.md) and [Connect to a Cluster's Node](GettingStarted.ConnectToCacheNode.md)\.

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not actively using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a Cluster](Clusters.Delete.md)\. 