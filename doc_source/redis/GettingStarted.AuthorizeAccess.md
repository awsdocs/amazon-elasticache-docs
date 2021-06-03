# Step 3: Authorize access to the cluster<a name="GettingStarted.AuthorizeAccess"></a>

 This section assumes that you are familiar with launching and connecting to Amazon EC2 instances\. For more information, see the *[Amazon EC2 Getting Started Guide](https://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)*\. 

All ElastiCache clusters are designed to be accessed from an Amazon EC2 instance\. The most common scenario is to access an ElastiCache cluster from an Amazon EC2 instance in the same Amazon Virtual Private Cloud \(Amazon VPC\), which will be the case for this exercise\. 

By default, network access to your cluster is limited to the user account that was used to create it\. Before you can connect to a cluster from an EC2 instance, you must authorize the EC2 instance to access the cluster\. The steps required depend upon whether you launched your cluster into EC2\-VPC or EC2\-Classic\.

The most common use case is when an application deployed on an EC2 instance needs to connect to a cluster in the same VPC\. The simplest way to manage access between EC2 instances and clusters in the same VPC is to do the following:

1. Create a VPC security group for your cluster\. This security group can be used to restrict access to the cluster instances\. For example, you can create a custom rule for this security group that allows TCP access using the port you assigned to the cluster when you created it and an IP address you will use to access the cluster\. 

   The default port for Redis clusters and replication groups is `6379`\.

1. Create a VPC security group for your EC2 instances \(web and application servers\)\. This security group can, if needed, allow access to the EC2 instance from the Internet via the VPC's routing table\. For example, you can set rules on this security group to allow TCP access to the EC2 instance over port 22\.

1. Create custom rules in the security group for your Cluster that allow connections from the security group you created for your EC2 instances\. This would allow any member of the security group to access the clusters\.

**Note**  
If you are planning to use [Local Zones](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Local_zones.html), ensure that you have enabled them\. When you create a subnet group in that local zone, your VPC is extended to that Local Zone and your VPC will treat the subnet as any subnet in any other Availability Zone\. All relevant gateways and route tables will be automatically adjusted\.

**To create a rule in a VPC security group that allows connections from another security group**

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc](https://console.aws.amazon.com/vpc)\.

1. In the navigation pane, choose **Security Groups**\.

1. Select or create a security group that you will use for your Cluster instances\. Under **Inbound Rules**, select **Edit Inbound Rules** and then select **Add Rule**\. This security group will allow access to members of another security group\.

1. From **Type** choose **Custom TCP Rule**\.

   1. For **Port Range**, specify the port you used when you created your cluster\.

      The default port for Redis clusters and replication groups is `6379`\.

   1. In the **Source** box, start typing the ID of the security group\. From the list select the security group you will use for your Amazon EC2 instances\.

1. Choose **Save** when you finish\.  
![\[\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/VPC-Rules.png)

Once you have enabled access, you are now ready to connect to the node, as discussed in the next section\.

For information on accessing your ElastiCache cluster from a different Amazon VPC, a different AWS Region, or even your corporate network, see the following:
+ [Access Patterns for Accessing an ElastiCache Cluster in an Amazon VPC](elasticache-vpc-accessing.md)
+ [Accessing ElastiCache resources from outside AWS](accessing-elasticache.md#access-from-outside-aws)