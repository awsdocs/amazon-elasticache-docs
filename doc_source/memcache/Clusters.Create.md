# Creating a cluster<a name="Clusters.Create"></a>

The following examples show how to create a cluster using the AWS Management Console, AWS CLI and ElastiCache API\.

## Creating a Memcached cluster \(console\)<a name="Clusters.Create.CON.Memcached"></a>

When you use the Memcached engine, Amazon ElastiCache supports horizontally partitioning your data over multiple nodes\. Memcached enables auto discovery so you don't need to keep track of the endpoints for each node\. Memcached tracks each node's endpoint, updating the endpoint list as nodes are added and removed\. All your application needs to interact with the cluster is the configuration endpoint\. For more information on auto discovery, see [Automatically identify nodes in your cluster](AutoDiscovery.md)\.

**To create a Memcached cluster using the ElastiCache console:**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the list in the upper\-right corner, choose the AWS Region that you want to launch this cluster in\.

1. Choose **Memcached** from the navigation pane\.

1. Choose **Create**\.

1. For **Cluster engine**, choose **Memcached**\. Choosing Memcached creates a Memcached cluster that looks something like this\.   
![\[Image: Memcached cluster with data partitioning\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-Cluster-Memcached.png)

   *Memcached cluster with data partitioning*

1. Complete the **Memcached settings** section\.

   1. For **Name**, enter a name for your cluster\.

      Cluster naming constraints are as follows:
      + Must contain 1–40 alphanumeric characters or hyphens\.
      + Must begin with a letter\.
      + Can't contain two consecutive hyphens\.
      + Can't end with a hyphen\.

   1. For **Engine version compatibility**, choose the Memcached engine version that you want this cluster to run\. Unless you have a specific reason to run an older version, we recommend that you choose the latest version\.
**Important**  
You can upgrade to newer engine versions\. For more information, see [Upgrading engine versions](VersionManagement.md)\. Any change in Memcached engine versions is a disruptive process in which you lose your cluster data\.

   1. In **Port**, accept the default port, 11211\. If you have a reason to use a different port, enter the port number\.

   1. For **Parameter group**, choose the default parameter group, choose the parameter group that you want to use with this cluster, or choose *Create new* to create a new parameter group to use with this cluster\.

      Parameter groups control the run\-time parameters of your cluster\. For more information on parameter groups, see [Memcached specific parameters](ParameterGroups.Memcached.md) and [Creating a parameter group](ParameterGroups.Creating.md)\.

   1. For **Node type**, click the down arrow \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-DnArrow.png)\)\. In the **Change node type** dialog box, choose the *Instance family* of the node type that you want, choose the node type that you want to use for this cluster, and then choose **Save**\.

      For more information, see [Choosing your Memcached node size](nodes-select-size.md#CacheNodes.SelectSize)\.

   1. For **Number of nodes**, choose the number of nodes that you want for this cluster\. You partition your data across the cluster's nodes\.

      If you need to change the number of nodes later, scaling horizontally is quite easy with Memcached\. For more information, see [Scaling ElastiCache for Memcached clusters](Scaling.md)

1. Click **Advanced Memcached settings** and complete the section\.

   1. For **Subnet group**, choose the subnet that you want to apply to this cluster\.

      If you are [Using local zones with ElastiCache ](Local_zones.md), you must create or choose a subnet that is in the local zone\. 

      For more information, see [Subnets and subnet groups](SubnetGroups.md)\.

   1. For **Availability zone\(s\)**, you have two options:
      + **No preference** – ElastiCache chooses the Availability Zone for each node in your cluster\.
      + **Specify availability zones** – Specify the Availability Zone for each node in your cluster\.

        If you choose to specify the Availability Zones, for each node choose an Availability Zone at the right of the node name\.

        We recommend locating your nodes in multiple Availability Zones for improved fault tolerance\. For more information, see [Mitigating Availability Zone Failures](FaultTolerance.md#FaultTolerance.Memcached.AZ)\.

      For more information, see [Choosing regions and availability zones](RegionsAndAZs.md)\.

   1. For **Security groups**, choose the security groups that you want to apply to this cluster\.

      For more information, see [Amazon VPCs and ElastiCache security](VPCs.md)\.

   1. The **Maintenance window** is the time, generally an hour in length, each week when ElastiCache schedules system maintenance for your cluster\. You can allow ElastiCache choose the day and time for your maintenance window \(**No preference**\), or you can choose the day, time, and duration yourself \(**Specify maintenance window**\)\. If you choose **Specify maintenance window**, choose the **Start day**, **Start time**, and **Duration** \(in hours\) for your maintenance window\. All times are UCT times\.

      For more information, see [Managing maintenance](maintenance-window.md)\.

   1. For **Notifications**, choose an existing Amazon Simple Notification Service \(Amazon SNS\) topic, or choose manual ARN input and enter the topic Amazon Resource Name \(ARN\)\. Amazon SNS allows you to push notifications to Internet\-connected smart devices\. The default is to disable notifications\. For more information, see [http://aws\.amazon\.com/sns/](http://aws.amazon.com/sns/)\.

1. Review all your entries and choices, then go back and make any needed corrections\. When you're ready, choose **Create** to launch your cluster\.

As soon as your cluster's status is *available*, you can grant Amazon EC2 access to it, connect to it, and begin using it\. For more information, see [Step 3: Authorize access to the cluster](GettingStarted.AuthorizeAccess.md) and [Step 4: Connect to the cluster's node](GettingStarted.ConnectToCacheNode.md)\.

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not actively using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a cluster](Clusters.Delete.md)\. 

## Creating a cluster \(AWS CLI\)<a name="Clusters.Create.CLI"></a>

To create a cluster using the AWS CLI, use the `create-cache-cluster` command\.

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not actively using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a cluster](Clusters.Delete.md)\. 

### Creating a Memcached Cache Cluster \(AWS CLI\)<a name="Clusters.Create.CLI.Memcached"></a>

The following CLI code creates a Memcached cache cluster with 3 nodes\.

For Linux, OS X, or Unix:

```
aws elasticache create-cache-cluster \
--cache-cluster-id my-cluster \
--cache-node-type cache.r4.large \
--engine memcached \
--engine-version 1.4.24 \
--cache-parameter-group default.memcached1.4 \
--num-cache-nodes 3
```

For Windows:

```
aws elasticache create-cache-cluster ^
--cache-cluster-id my-cluster ^
--cache-node-type cache.r4.large ^
--engine memcached ^
--engine-version 1.4.24 ^
--cache-parameter-group default.memcached1.4 ^
--num-cache-nodes 3
```

## Creating a cluster \(ElastiCache API\)<a name="Clusters.Create.API"></a>

To create a cluster using the ElastiCache API, use the `CreateCacheCluster` action\. 

**Important**  
As soon as your cluster becomes available, you're billed for each hour or partial hour that the cluster is active, even if you're not using it\. To stop incurring charges for this cluster, you must delete it\. See [Deleting a cluster](Clusters.Delete.md)\. 

**Topics**

### Creating a Memcached cache cluster \(ElastiCache API\)<a name="Clusters.Create.API.Memcached"></a>

The following code creates a Memcached cluster with 3 nodes \(ElastiCache API\)\.

Line breaks are added for ease of reading\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=CreateCacheCluster
    &CacheClusterId=my-cluster
    &CacheNodeType=cache.r4.large
    &Engine=memcached
    &NumCacheNodes=3
    &SignatureVersion=4       
    &SignatureMethod=HmacSHA256
    &Timestamp=20150508T220302Z
    &Version=2015-02-02
    &X-Amz-Algorithm=&AWS;4-HMAC-SHA256
    &X-Amz-Credential=<credential>
    &X-Amz-Date=20150508T220302Z
    &X-Amz-Expires=20150508T220302Z
    &X-Amz-SignedHeaders=Host
    &X-Amz-Signature=<signature>
```