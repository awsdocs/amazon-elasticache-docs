# Step 2: Create a cluster<a name="GettingStarted.CreateCluster"></a>

The cluster you're about to launch will be live, and not running in a sandbox\. You incur the standard ElastiCache usage fees for the instance until you delete it\. The total charges are minimal \(typically less than a dollar\) if you complete the exercise described here in one sitting and delete your cluster when you are finished\. For more information about ElastiCache usage rates, see [https://aws\.amazon\.com/elasticache/](https://aws.amazon.com/elasticache/)\.

**Important**  
Your cluster is launched in an Amazon VPC\. Before you start creating your cluster, you need to create a subnet group\. For more information, see [Creating a subnet group](SubnetGroups.Creating.md)\.

**To create an ElastiCache for Memcached cluster**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the list in the upper\-right corner, choose the AWS Region you want to launch this cluster in\.

1. Choose **Get started**\.

1. Choose **Create VPC** and follow the steps outlined at [Creating a Virtual Private Cloud \(VPC\)](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/VPCs.CreatingVPC.html)\.

1. On the ElastiCache dashboard page, choose **Create cluster** and then choose **Create Memcached cluster**\.

1. Under **Location**:

------
#### [ AWS Cloud  ]

   1. Under **Cluster info**

      1. Enter a value for **Name**\. 

      1. \(Optional\) Enter a value for **Description**\.

   1. Under **Cluster settings**

      1. For **Engine version**, choose an available version\.

      1. For **Port**, accept the default port, 11211\. If you have a reason to use a different port, type in the port number\.

      1. For **Parameter groups**, choose the parameter group you want to use with this cluster, or choose "Create new" to create a new parameter group to use with this cluster\. For this exercise, accept the *default* parameter group\.

         For more information, see [Creating a parameter group](ParameterGroups.Creating.md)\.

      1. For **Node type**, choose the node type that you want to use for this cluster\. For this exercise, you can accept the default node type or select another node type\.

         For more information, see [Choosing your Memcached node size](nodes-select-size.md#CacheNodes.SelectSize)\.

         To select another node type:

         1. Choose the down\-arrow to the right of the default node type\.

         1. Choose the **Instance family** you want for the nodes in this cluster\. Since this is just an exercise, to save costs, choose **t2**\.

         1. From the available node types, choose the box to the left of the node type you want for this cluster\. Since this is just an exercise, to save costs, choose **cache\.t2\.small**\.

         1. Choose **Save**\.

      1. From the **Number of nodes** list, choose the number of nodes \(partitions\) you want provisioned for this cluster\.

      1. For **Subnet group settings** list, you can either create a new one or choose the subnet you want to apply to this cluster\. For this exercise, accept the default subnet group\.

         For more information, see [Subnets and subnet groups](SubnetGroups.md)\.

      1. For **Availability zone placemnts**, you have two options\.
         + **No preference** – ElastiCache chooses each node's Availability Zone for you\.
         + **Specify availability zones** – You specify the Availability Zone for each node\.

         For this exercise, choose **Specify availability zones** and then choose an Availability Zone for each node\.

         For more information, see [Choosing regions and availability zones](RegionsAndAZs.md)\.

   1. Under **Advanced settings**

      1. For **Security**: 

         1. To encrypt your data, you have the option to enable **Encryption in\-transit**\. This enables encryption of data on the wire\. For more information, see [Encryption in transit](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/in-transit-encryption.html)\.

         1. For **Selected security groups** list, choose the security groups that you want to use for this cluster or create a new one\. For this exercise, accept the default security group\.

            For more information, see [Amazon VPCs and ElastiCache security](VPCs.md)\.

      1. 

         1. The **Maintenance window** is the time, generally an hour, each week where ElastiCache schedules system maintenance on your cluster\. You have two options\.
            + **No preference**—ElastiCache chooses the day of the week and time of day for the cluster's maintenance window\.
            + **Specify maintenance window**—You choose the day of the week, start time, and duration for the cluster's maintenance window\.

         1. After the cluster is created, you can modify it to specify a difference maintenance window\. For more information, see [Managing maintenance](maintenance-window.md)\.

      1. For **Tags** to help you manage your clusters and other ElastiCache resources, you can assign your own metadata to each resource in the form of tags\. For mor information, see [Tagging your ElastiCache resources](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/Tagging-Resources.html)\.

      1. Choose **Next**\.

      1. Review your configuration and when satisfied, choose **Create**\.

------
#### [ On premises ]

   To finish creating the cluster, follow the steps at [Using Outposts](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/ElastiCache-Outposts.html)\.

------