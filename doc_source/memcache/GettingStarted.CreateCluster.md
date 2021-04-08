# Step 2: Create a cluster<a name="GettingStarted.CreateCluster"></a>

The cluster you're about to launch will be live, and not running in a sandbox\. You incur the standard ElastiCache usage fees for the instance until you delete it\. The total charges are minimal \(typically less than a dollar\) if you complete the exercise described here in one sitting and delete your cluster when you are finished\. For more information about ElastiCache usage rates, see [https://aws\.amazon\.com/elasticache/](https://aws.amazon.com/elasticache/)\.

**Important**  
Your cluster is launched in an Amazon VPC\. Before you start creating your cluster, you need to create a subnet group\. For more information, see [Creating a subnet group](SubnetGroups.Creating.md)\.

**To create an ElastiCache for Memcached cluster**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. Choose **Get Started Now**\.

   If you already have an available cluster, choose **Create**\.

1. From the list in the upper\-right corner, choose the AWS Region you want to launch this cluster in\.

1. For **Cluster engine**, choose **Memcached**\.

1. Complete the **Memcached settings** section as follows:

   1. In **Name**, type a name for your cluster\.

      Cluster naming constraints are as follows:
      + Must contain 1–40 alphanumeric characters or hyphens\.
      + Must begin with a letter\.
      + Can't contain two consecutive hyphens\.
      + Can't end with a hyphen\.

   1. From the **Engine version compatibility** list, choose the Memcached engine version you want to run on this cluster\. Unless you have a specific reason to run an older version, we recommend that you choose the latest version\.

   1. In **Port**, accept the default port, 11211\. If you have a reason to use a different port, type in the port number\.

   1. From **Parameter group**, choose the parameter group you want to use with this cluster, or choose "Create new" to create a new parameter group to use with this cluster\. For this exercise, accept the *default* parameter group\.

      For more information, see [Creating a parameter group](ParameterGroups.Creating.md)\.

   1. For **Node type**, choose the node type that you want to use for this cluster\. For this exercise, you can accept the default node type or select another node type\.

      For more information, see [Choosing your Memcached node size](nodes-select-size.md#CacheNodes.SelectSize)\.

      To select another node type:

      1. Choose the down\-arrow to the right of the default node type\.

      1. Choose the **Instance family** you want for the nodes in this cluster\. Since this is just an exercise, to save costs, choose **t2**\.

      1. From the available node types, choose the box to the left of the node type you want for this cluster\. Since this is just an exercise, to save costs, choose **cache\.t2\.small**\.

      1. Choose **Save**\.

   1. From the **Number of nodes** list, choose the number of nodes \(partitions\) you want provisioned for this cluster\.

1. Choose **Advanced Memcached settings** and complete the section as follows:

   1. From the **Subnet group** list, choose the subnet you want to apply to this cluster\. For this exercise, accept the default subnet group\.

      For more information, see [Subnets and subnet groups](SubnetGroups.md)\.

   1. For **Availability zone\(s\)**, you have two options\.
      + **No preference** – ElastiCache chooses each node's Availability Zone for you\.
      + **Specify availability zones** – You specify the Availability Zone for each node\.

      For this exercise, choose **Specify availability zones** and then choose an Availability Zone for each node\.

      For more information, see [Choosing regions and availability zones](RegionsAndAZs.md)\.

   1. From the **Security groups** list, choose the security groups that you want to use for this cluster\. For this exercise, accept the default security group\.

      For more information, see [Amazon VPCs and ElastiCache security](VPCs.md)\.

   1. The **Maintenance window** is the time, generally an hour, each week where ElastiCache schedules system maintenance on your cluster\. You have two options\.
      + **No preference**—ElastiCache chooses the day of the week and time of day for the cluster's maintenance window\.
      + **Specify maintenance window**—You choose the day of the week, start time, and duration for the cluster's maintenance window\.

      After the cluster is created, you can modify it to specify a difference maintenance window\. For more information, see [Managing maintenance](maintenance-window.md)\.

   1. For **Notifications**, leave it as *Disable notifications*\.

1. Choose **Create** to launch your cluster, or **Cancel** to cancel the operation\.