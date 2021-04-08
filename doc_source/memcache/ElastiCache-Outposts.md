# Using Outposts<a name="ElastiCache-Outposts"></a>

AWS Outposts is a fully managed service that extends AWS infrastructure, services, APIs, and tools to customer premises\. By providing local access to AWS managed infrastructure, AWS Outposts enables customers to build and run applications on premises using the same programming interfaces as in AWS Regions, while using local compute and storage resources for lower latency and local data processing needs\. An Outpost is a pool of AWS compute and storage capacity deployed at a customer site\. AWS operates, monitors, and manages this capacity as part of an AWS Region\. You can create subnets on your Outpost and specify them when you create AWS resources such as ElastiCache clusters\.

**Note**  
In this version, the following limitations apply:   
ElastiCache for Outposts only supports M5 and R5 node families\.
Multi\-AZ \(cross Outpost replication is not supported\)\.
ElastiCache for Outposts is not supported in the following regions: cn\-north\-1, cn\-northwest\-1 and ap\-northeast\-3\.

## Using Outposts with the Memcached console<a name="Outposts.Details-memcached"></a>

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. On the navigation pane, choose **Memcached**\. 

1. In **Cluster Engine**, select **Memcached**\. 

1. Under **Location**, select **On\-Premises \- Create your ElastiCache instances on AWS Outposts**\. 

### Configure on\-premises options<a name="Outposts.Creating.Console.Memcached.Details"></a>

 You can select either an available Outpost to add your cache cluster or, if there are no available Outposts, create a new one using the following steps:

**Under **On\-Premises options**:**

1. Under **Memcached settings**:

   1. **Name**: Enter a name for the Memcached cluster

   1. **Description**: Enter a description for the Memcached cluster\.

   1. **Engine version compatilbility**: Engine version is based on the AWS Outpost region 

   1. **Port**: Accept the default port, 11211\. If you have a reason to use a different port, type the port number\. 

   1. **Parameter group**: Use the drop\-down to select a default or custom parameter group\. 

   1. **Node Type**: Available instances are based on Outposts availability\. From the drop down list, select **Outposts** and then select an available node type you want to use for this cluster\. Then select **Save**\. 

   1. **Number of nodes**: Enter the number of nodes you want in your cluster\.

1. Under **Advanced Memcached settings**:

   1. **Subnet Group**: From the list, select **Create new**\.
      + **Name**: Enter a name for the subnet group
      + **Description**: Enter a description for the subnet group
      + **VPC ID**: The VPC ID should match the Outpost VPC\.
      + **Availability Zone or Outpost**: Select the Outpost you are using\.
      + **Subnet ID**: Select a subnet ID that is available for the Outpost\. If there are no subnet IDs available, you need to create them\. For more information, see [Create a Subnet](https://docs.aws.amazon.com/outposts/latest/userguide/launch-instance.html#create-subnet)\.

   1. Select **Create**\.

### Viewing Outpost cluster details<a name="Outposts.Creating.Console.Outpost-Details"></a>

On the Memcached list page, select a cluster that belongs to an AWS Outpost and note the following when viewing the **Cluster details**:
+ **Availability Zone**: This will represent the Outpost, using an ARN \(Amazon Resource Name\) and the AWS Resource Number\.
+ **Outpost name**: The name of the AWS Outpost\. 

## Using Outposts with the AWS CLI<a name="Outposts.Using.CLI"></a>

You can use the AWS Command Line Interface \(AWS CLI\) to control multiple AWS services from the command line and automate them through scripts\. You can use the AWS CLI for ad hoc \(one\-time\) operations\. 

### Downloading and configuring the AWS CLI<a name="Redis-Global-Clusters-Downloading-CLI"></a>

The AWS CLI runs on Windows, macOS, or Linux\. Use the following procedure to download and configure it\.

**To download, install, and configure the CLI**

1. Download the AWS CLI on the [AWS Command Line Interface](http://aws.amazon.com/cli) webpage\.

1. Follow the instructions for [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) and [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html) in the *AWS Command Line Interface User Guide*\.

### Using the AWS CLI with Outposts<a name="Redis-Outposts-Using-CLI"></a>

Use the following CLI operation to create a cache cluster that uses Outposts: 
+  [create\-cache\-cluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/CommandLineReference/CLIReference-cmd-CreateCacheCluster.html)  – Using this operation, the `outpost-mode` parameter accepts a value that specifies whether the nodes in the cache cluster are created in a single Outpost or across multiple Outposts\. 
**Note**  
At this time, only `single-outpost` mode is supported\.

  ```
  aws elasticache create-cache-cluster \
     --cache-cluster-id cache cluster id \
     --outpost-mode single-outpost \
  ```