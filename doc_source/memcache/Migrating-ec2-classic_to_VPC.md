# Migrating an EC2\-Classic cluster into a VPC<a name="Migrating-ec2-classic_to_VPC"></a>

**Note**  
We are retiring EC2\-Classic on August 15, 2022\.

Some legacy ElastiCache clusters on the EC2\-Classic platform are not in an Amazon VPC\. You can use the AWS Management Console to move it into an Amazon VPC using the following steps:

## Migrating an EC2\-Classic cluster into a VPC<a name="Moving_to_VPC"></a>

**Note**  
To determine if your cluster is on the EC2\-Classic platform, see [Determine the cluster's platform](accessing-elasticache.md#authorize-access-vpc-or-classic)\.

1. You first need to create a Virtual Private Cloud \(VPC\)\. Follow the steps to create a VPC at [Creating a Virtual Private Cloud \(VPC\)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/VPCs.CreatingVPC.html)\.

1. Once you have created the VPC, follow the steps to create a subnet group at [Subnets and Subnet Groups](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/SubnetGroups.html)\.

1. Your VPC comes with a default security group\. You can choose to use it or you can create one by following the steps at [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the Amazon VPC documentation\.

1. Create a Memcached cluster\. Under **Advanced Memcached settings**, **Subnet group**, choose the subnet group that belongs to the VPC you created previously\. For more information on creating a Memcached cluster in a VPC, see [Create a Memcached cluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/GettingStarted.CreateCluster.html)\.

1. To link an EC2\-Classic instance to an ElastiCache for Memcached cluster in a VPC, you must first enable the VPC for ClassicLink\. You cannot enable a VPC for ClassicLink if the VPC has routing that conflicts with the EC2\-Classic private IP address range\. For more information, see [Routing for ClassicLink](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-classiclink.html#classiclink-routing)\.

    To enable a VPC for ClassicLink
   + Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.
   + In the navigation pane, choose **Your VPCs**\.
   + Select the VPC\. 
   + Choose **Actions**, **Enable ClassicLink**\. 
   + When prompted for confirmation, choose **Enable ClassicLink**\. 
   + \(Optional\) If you want the public DNS hostname to resolve to the private IP address, enable ClassicLink DNS support for the VPC before you link any instances\. For more information, see [Enable ClassicLink DNS support](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-classiclink.html#classiclink-enable-dns-support)\.

1. Finally, you connect to an ElastiCache cluster from your EC2 instance following the steps at [Connect to a cluster's node](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/GettingStarted.ConnectToCacheNode.html)\.

**Note**  
Previous generation node types might not be supported on the VPC platform\. For a full list of VPC supported node types, see [Supported node types by AWS Region](CacheNodes.SupportedTypes.md#CacheNodes.SupportedTypesByRegion)\.