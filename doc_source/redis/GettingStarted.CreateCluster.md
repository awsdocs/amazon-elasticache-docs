# Step 2: Create a cluster<a name="GettingStarted.CreateCluster"></a>

Before creating a cluster for production use, you obviously need to consider how you will configure the cluster to meet your business needs\. Those issues are addressed in the [Preparing a cluster](Clusters.Prepare.md) section\. For the purposes of this Getting Started exercise, you will create a cluster with cluster mode disabled and you can accept the default configuration values where they apply\.

The cluster you create will be live, and not running in a sandbox\. You will incur the standard ElastiCache usage fees for the instance until you delete it\. The total charges will be minimal \(typically less than a dollar\) if you complete the exercise described here in one sitting and delete your cluster when you are finished\. For more information about ElastiCache usage rates, see [Amazon ElastiCache](https://aws.amazon.com/elasticache/)\.

Your cluster is launched in a virtual private cloud \(VPC\) based on the Amazon VPC service\. 

## Creating a Redis \(cluster mode disabled\) cluster \(Console\)<a name="Clusters.Create.CON.Redis-gs"></a>

****

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

   1. For **Subnet group**, choose the subnet group you created in the previous section to apply to this cluster\. If you enabled Multi\-AZ, the subnet group must contain at least two subnets that reside in different availability zones\.

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
You have the option to supply a different encryption key by choosing **Customer Managed AWS KMS key** and choosing the key\. For more information, see [Using Customer Managed AWS KMS keys](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/at-rest-encryption.html#using-customer-managed-keys-for-elasticache-security)
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

As soon as your cluster's status is *available*, you can grant Amazon EC2 access to it, connect to it, and begin using it\. For more information, see [Step 3: Authorize access to the cluster](GettingStarted.AuthorizeAccess.md) and [Step 4: Connect to the cluster's node](GettingStarted.ConnectToCacheNode.md)\.

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not actively using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a cluster](Clusters.Delete.md)\. 

## Creating a Redis \(cluster mode disabled\) cluster \(AWS CLI\) \(AWS CLI\)<a name="Clusters.Create.CLI.Redis-gs"></a>

**Example**  
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

## Creating a Redis \(cluster mode disabled\) cluster \(ElastiCache API\)<a name="Clusters.Create.API.Redis-gs"></a>

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
    &X-Amz-Algorithm=&AWS;4-HMAC-SHA256
    &X-Amz-Credential=<credential>
    &X-Amz-Date=20150508T220302Z
    &X-Amz-Expires=20150508T220302Z
    &X-Amz-SignedHeaders=Host
    &X-Amz-Signature=<signature>
```

To work with cluster mode enabled, see the following topics:
+ To use the console, see [Creating a Redis \(cluster mode enabled\) cluster \(Console\)](Clusters.Create.md#Clusters.Create.CON.RedisCluster)\.
+ To use the AWS CLI, see [Creating a Redis \(cluster mode enabled\) cluster \(AWS CLI\)](Clusters.Create.md#Clusters.Create.CLI.RedisCluster)\.
+ To use the ElastiCache API, see[Creating a cache cluster in Redis \(cluster mode enabled\) \(ElastiCache API\)](Clusters.Create.md#Clusters.Create.API.RedisCluster)\.