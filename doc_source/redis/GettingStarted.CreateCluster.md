# Step 2: Create a cluster<a name="GettingStarted.CreateCluster"></a>

Before creating a cluster for production use, you obviously need to consider how you will configure the cluster to meet your business needs\. Those issues are addressed in the [Preparing a cluster](Clusters.Prepare.md) section\. For the purposes of this Getting Started exercise, you will create a cluster with cluster mode disabled and you can accept the default configuration values where they apply\.

The cluster you create will be live, and not running in a sandbox\. You will incur the standard ElastiCache usage fees for the instance until you delete it\. The total charges will be minimal \(typically less than a dollar\) if you complete the exercise described here in one sitting and delete your cluster when you are finished\. For more information about ElastiCache usage rates, see [Amazon ElastiCache](https://aws.amazon.com/elasticache/)\.

Your cluster is launched in a virtual private cloud \(VPC\) based on the Amazon VPC service\. 

## Creating a Redis \(cluster mode disabled\) cluster \(Console\)<a name="Clusters.Create.CON.Redis-gs"></a>

**To create a Redis \(cluster mode disabled\) cluster using the ElastiCache console**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the list in the upper\-right corner, choose the AWS Region that you want to launch this cluster in\.

1. Choose **Get started** from the navigation pane\.

1. Choose **Create VPC** and follow the steps outlined at [Creating a Virtual Private Cloud \(VPC\)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/VPCs.CreatingVPC.html)\.

1. On the ElastiCache dashboard page, choose **Create cluster** and then choose **Create Redis cluster**\.

1. Under **Cluster settings**, do the following:

   1. Choose **Configure and create a new cluster**\.

   1. For **Cluster mode**, choose **Disabled**\.

   1. For **Cluster info** enter a value for **Name**\. 

   1. \(Optional\) Enter a value for **Description**\.

1. Under **Location**:

------
#### [ AWS Cloud  ]

   1. For **AWS Cloud**, we recommend you accept the default settings for **Multi\-AZ** and **Auto\-failover**\. For more information, see [Minimizing downtime in ElastiCache for Redis with Multi\-AZ](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/AutoFailover.html)\.

   1. Under **Cluster settings**

      1. For **Engine version**, choose an available version\.

      1. For **Port**, use the default port, 6379\. If you have a reason to use a different port, enter the port number\.

      1. For **Parameter group**, choose a parameter group or create a new one\. Parameter groups control the runtime parameters of your cluster\. For more information on parameter groups, see [Redis\-specific parameters](ParameterGroups.Redis.md) and [Creating a parameter group](ParameterGroups.Creating.md)\.
**Note**  
When you select a parameter group to set the engine configuration values, that parameter group is applied to all clusters in the global datastore\. On the **Parameter Groups** page, the yes/no **Global** attribute indicates whether a parameter group is part of a global datastore\.

      1. For **Node type**, choose the down arrow \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-DnArrow.png)\)\. In the **Change node type** dialog box, choose a value for **Instance family** for the node type that you want\. Then choose the node type that you want to use for this cluster, and then choose **Save**\.

         For more information, see [Choosing your node size](nodes-select-size.md#CacheNodes.SelectSize)\.

         If you choose an r6gd node type, data\-tiering is automatically enabled\. For more information, see [Data tiering](data-tiering.md)\.

      1. For **Number of replicas**, choose the number of read replicas you want\. If you enabled Multi\-AZ, the number must be between 1\-5\.

   1. Under **Connectivity**

      1. For **Network type**, choose the IP version\(s\) this cluster will support\. 

      1. For **Subnet groups**, choose the subnet that you want to apply to this cluster\. ElastiCache uses that subnet group to choose a subnet and IP addresses within that subnet to associate with your nodes\. ElastiCache clusters require a dual\-stack subnet with both IPv4 and IPv6 addresses assigned to them to operate in dual\-stack mode and an IPv6\-only subnet to operate as IPv6\-only\.

         When creating a new subnet group, enter the **VPC ID** to which it belongs\.

         For more information, see:
         + [Choosing a network type](network-type.md)\.
         + [Create a subnet in your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#AddaSubnet)\.

         If you are [Using local zones with ElastiCache ](Local_zones.md), you must create or choose a subnet that is in the local zone\. 

         For more information, see [Subnets and subnet groups](SubnetGroups.md)\.

   1. For **Availability zone placements**, you have two options:
      + **No preference** – ElastiCache chooses the Availability Zone\.
      + **Specify availability zones** – You specify the Availability Zone for each cluster\.

        If you chose to specify the Availability Zones, for each cluster in each shard, choose the Availability Zone from the list\.

      For more information, see [Choosing regions and availability zones](RegionsAndAZs.md)\.

   1. Choose **Next**

   1. Under **Advanced Redis settings**

      1. For **Security**: 

        1. To encrypt your data, you have the following options:
           + **Encryption at rest** – Enables encryption of data stored on disk\. For more information, see [Encryption at Rest](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/at-rest-encryption.html)\.
**Note**  
You have the option to supply a different encryption key by choosing **Customer Managed AWS KMS key** and choosing the key\. For more information, see [Using customer managed keys from AWS KMS](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/at-rest-encryption.html#using-customer-managed-keys-for-elasticache-security)\.
           + **Encryption in\-transit** – Enables encryption of data on the wire\. For more information, see [encryption in transit](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/in-transit-encryption.html)\. For Redis engine version 6\.0 and above, if you enable Encryption in\-transit you will be prompted to specify one of the following **Access Control** options:
             + **No Access Control** – This is the default setting\. This indicates no restrictions on user access to the cluster\.
             + **User Group Access Control List** – Select a user group with a defined set of users that can access the cluster\. For more information, see [Managing User Groups with the Console and CLI](Clusters.RBAC.md#User-Groups)\.
             + **Redis AUTH Default User** – An authentication mechanism for Redis server\. For more information, see [Redis AUTH](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/auth.html)\.
           + **Redis AUTH** – An authentication mechanism for Redis server\. For more information, see [Redis AUTH](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/auth.html)\.
**Note**  
For Redis versions between 3\.2\.6 onward, excluding version 3\.2\.10, Redis AUTH is the sole option\.

        1. For **Security groups**, choose the security groups that you want for this cluster\. A *security group* acts as a firewall to control network access to your cluster\. You can use the default security group for your VPC or create a new one\.

           For more information on security groups, see [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide\.*

   1. For regularly scheduled automatic backups, select **Enable automatic backups** and then enter the number of days that you want each automatic backup retained before it is automatically deleted\. If you don't want regularly scheduled automatic backups, clear the **Enable automatic backups** check box\. In either case, you always have the option to create manual backups\.

      For more information on Redis backup and restore, see [Backup and restore for ElastiCache for Redis ](backups.md)\.

   1. \(Optional\) Specify a maintenance window\. The *maintenance window* is the time, generally an hour in length, each week when ElastiCache schedules system maintenance for your cluster\. You can allow ElastiCache to choose the day and time for your maintenance window \(*No preference*\), or you can choose the day, time, and duration yourself \(*Specify maintenance window*\)\. If you choose *Specify maintenance window* from the lists, choose the *Start day*, *Start time*, and *Duration* \(in hours\) for your maintenance window\. All times are UCT times\.

      For more information, see [Managing maintenance](maintenance-window.md)\.

   1. \(Optional\) For **Logs**:
      + Under **Log format**, choose either **Text** or **JSON**\.
      + Under **Destination Type**, choose either **CloudWatch Logs** or **Kinesis Firehose**\.
      + Under **Log destination**, choose either **Create new** and enter either your CloudWatch Logs log group name or your Kinesis Data Firehose stream name, or choose **Select existing** and then choose either your CloudWatch Logs log group name or your Kinesis Data Firehose stream name,

   1. For **Tags**, to help you manage your clusters and other ElastiCache resources, you can assign your own metadata to each resource in the form of tags\. For mor information, see [Tagging your ElastiCache resources](Tagging-Resources.md)\.

   1. Choose **Next**\.

   1. Review all your entries and choices, then make any needed corrections\. When you're ready, choose **Create**\.

------
#### [ On premises ]

   1. For **On premises**, we recommend you leave **Auto\-failover** enabled\. For more information, see [Minimizing downtime in ElastiCache for Redis with Multi\-AZ](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/AutoFailover.html)

   1. To finish creating the cluster, follow the steps at [Using Outposts](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/ElastiCache-Outposts.html)\.

------

As soon as your cluster's status is *available*, you can grant Amazon EC2 access to it, connect to it, and begin using it\. For more information, see [Step 3: Authorize access to the cluster](GettingStarted.AuthorizeAccess.md) and [Step 4: Connect to the cluster's node](GettingStarted.ConnectToCacheNode.md)\.

**Important**  
Once your cluster is available, you're billed for each hour or partial hour that the cluster is active, even if you're not actively using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a cluster](Clusters.Delete.md)\. 

## Creating a Redis \(cluster mode disabled\) cluster \(AWS CLI\)<a name="Clusters.Create.CLI.Redis-gs"></a>

**Example**  
The following CLI code creates a Redis \(cluster mode disabled\) cache cluster with no replicas\.  
For Linux, macOS, or Unix:  

```
aws elasticache create-cache-cluster \
--cache-cluster-id my-cluster \
--cache-node-type cache.r4.large \
--engine redis \
--num-cache-nodes 1 \
--snapshot-arns arn:aws:s3:::my_bucket/snapshot.rdb
```
For Windows:  

```
aws elasticache create-cache-cluster ^
--cache-cluster-id my-cluster ^
--cache-node-type cache.r4.large ^
--engine redis ^
--num-cache-nodes 1 ^
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
    &CacheParameterGroup=default.redis6.x
    &Engine=redis
    &EngineVersion=6.0
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