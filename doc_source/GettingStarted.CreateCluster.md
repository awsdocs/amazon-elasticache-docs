# Step 2: Launch a Cluster<a name="GettingStarted.CreateCluster"></a>

Before you continue, be sure you have completed [Determine Your Requirements \[Every time\]](getting-started-determine-requirements.md)\.

The cluster you're about to launch will be live, and not running in a sandbox\. You will incur the standard ElastiCache usage fees for the instance until you delete it\. The total charges will be minimal \(typically less than a dollar\) if you complete the exercise described here in one sitting and delete your cluster when you are finished\. For more information about ElastiCache usage rates, see [https://aws\.amazon\.com/elasticache/](https://aws.amazon.com/elasticache/)\.

**Important**  
Your cluster will be launched in an Amazon VPC\. Before you start creating your cluster, you need to create a subnet group\. For more information, see [Creating a Subnet Group](SubnetGroups.Creating.md)\.

**To create a standalone Redis \(cluster mode disabled\) cluster**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. Choose **Get Started Now**\.

   If you already have an available cluster, choose **Launch Cluster**\.

1. From the dropdown in the upper right corner, choose the region you want to launch this cluster in\.

1. For **Cluster engine**, choose **Redis**\.

1. Make sure **Cluster Mode enabled \(Scale Out\)** is not chosen\.

1. Complete the **Redis settings** section as follows:

   1. In **Name**, type a name for your cluster\.

**Cluster naming constraints**

      + Must contain from 1 to 20 alphanumeric characters or hyphens\.

      + Must begin with a letter\.

      + Cannot contain two consecutive hyphens\.

      + Cannot end with a hyphen\.

   1. From the **Engine version compatibility** list, choose the Redis engine version you want to run on this cluster\. Unless you have a specific reason to run an older version, we recommend that you choose the latest version\.

   1. In **Port**, accept the default port, 6379\. If you have a reason to use a different port, enter the port number\.

   1. From **Parameter group**, choose the parameter group you want to use with this cluster, or choose "Create new" to create a new parameter group to use with this cluster\. For this exercise, accept the *default* parameter group\.

      For more information, see [Creating a Parameter Group](ParameterGroups.Creating.md)\.

   1. For **Node type**, choose the node type that you want to use for this cluster\. For this exercise, above the table choose the **t2** instance family, choose **cache\.t2\.small**, and finally choose **Save**\.

      For more information, see [Choosing Your Node Size](CacheNodes.SelectSize.md)\.

   1. From **Number of replicas**, choose the number of read replicas you want for this cluster\. Since in this exercise we're creating a standalone cluster, choose *None*\.

      When you choose *None*, the **Replication group description** field disappears\.

1. Choose **Advanced Redis settings** and complete the section as follows: 
**Note**  
The **Advanced Redis settings** details are slightly different if you are creating a Redis \(cluster mode enabled\) replication group\. For a step\-by\-step walk through to create a Redis \(cluster mode enabled\) replication group, see [Creating a Redis \(cluster mode enabled\) Cluster with Replicas from Scratch](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md)\.

   1. From the **Subnet group** list, choose the subnet you want to apply to this cluster\. For this exercise, choose *default*\.

      For more information, see [Subnets and Subnet Groups](SubnetGroups.md)\.

   1. For **Availability zone\(s\)**, you have two options\.

      + **No preference** – ElastiCache will choose the Availability Zone\.

      + **Specify availability zones** – You specify the Availability Zone for your cluster\.

      For this exercise, choose **Specify availability zones** and then choose an Availability Zone from the list below **Primary**\.

      For more information, see [Choosing Regions and Availability Zones](RegionsAndAZs.md)\.

   1. From the **Security groups** list, choose the security groups that you want to use for this cluster\. For this exercise, choose *default*\.

      For more information, see [ElastiCache and Security Groups](VPCs.md)\.

   1. If you are going to seed your cluster with data from a \.RDB file, in the ** Seed RDB file S3 location** box, enter the Amazon S3 location of the \.RDB file\.

      For more information, see [Seeding a New Cluster with an Externally Created Backup \(Redis\)](backups-seeding-redis.md)\.

   1. Because this is not a production cluster, clear the **Enable automatic backups** check box\.

      For more information on Redis backup and restore, see [ElastiCache Backup and Restore \(Redis\)](backups.md)\.

   1. The **Maintenance window** is the time, generally an hour, each week where ElastiCache schedules system maintenance on your cluster\. You can allow ElastiCache to specify the day and time for your maintenance window \(*No preference*\), or you can specify the day and time yourself \(*Specify maintenance window*\. If you choose *Specify maintenance window*, specify the **Start day**, **Start time**, and **Duration** \(in hours\) for your maintenance window\. For this exercise, choose *No preference*\.

      For more information, see [Maintenance Window](VersionManagement.MaintenanceWindow.md)\.

   1. For **Notifications**, leave it as *Disabled*\.

1. Choose **Create cluster** to launch your cluster, or **Cancel** to cancel the operation\.