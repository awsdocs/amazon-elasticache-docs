# Migrating an EC2\-Classic cluster into a VPC<a name="Migrating-ec2-classic_to_VPC"></a>

Some legacy ElastiCache clusters on the EC2\-Classic platform are not in an Amazon VPC\. You can use the AWS Management Console to move it into an Amazon VPC using the following steps:

## Migrating an EC2\-Classic Redis cluster into a VPC<a name="Moving_to_VPC"></a>

**Note**  
To determine if your cluster is on the EC2\-Classic platform, see [Determine the cluster's platform](accessing-elasticache.md#authorize-access-vpc-or-classic)\.

1. You first need to create a Virtual Private Cloud \(VPC\)\. Follow the steps to create a VPC at [Creating a Virtual Private Cloud \(VPC\)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/VPCs.CreatingVPC.html)\.

1. Once you have created the VPC, follow the steps to create a subnet group at [Subnets and Subnet Groups](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/SubnetGroups.html)\.

1. Your VPC comes with a default security group\. You can choose to use it or you can create one by following the steps at [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the Amazon VPC documentation\.

1. In order to add your cluster to a VPC, you must first create a backup of it and then restore the backup into the VPC\. Go to the ElastiCache for Redis console and choose your cluster\. Choose **Actions** and then choose **Backup**\. Enter a **Backup Name** and then choose **Create Backup**\.

1. Once the backup is created, you add it to the VPC you created previously\. Return to the ElastiCache for Redis console and choose **Backups**\. Choose the backup you created and then choose **Restore**\.

1. In **Cluster name**, enter a name for your cluster\.

1. In **Choose a Subnet group**, select the subnet group you created previously and then choose **Continue**\.

Once the restoration process is complete, your cluster will be available in the VPC\.

**Note**  
Previous generation node types might not be supported on the VPC platform\. For a full list of VPC supported node types, see [Supported node types by AWS Region](CacheNodes.SupportedTypes.md#CacheNodes.SupportedTypesByRegion)\.