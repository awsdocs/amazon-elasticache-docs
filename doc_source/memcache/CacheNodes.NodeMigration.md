# Migrating previous generation nodes<a name="CacheNodes.NodeMigration"></a>

Previous generation nodes are node types that are being phased out\. If you have no existing clusters using a previous generation node type, ElastiCache does not support the creation of new clusters with that node type\. If you have existing clusters, you can continue to use them or create new clusters using the previous generation node type\.

Due to the limited amount of previous generation node types, we cannot guarantee a successful replacement when a node becomes unhealthy in your cluster\(s\)\. In such a scenario, your cluster availability may be negatively impacted\.

 We recommend that you migrate your cluster\(s\) to a new node type for better availability and performance\. For a recommended node type to migrate to, see [Upgrade Paths](https://aws.amazon.com/ec2/previous-generation/)\. For a full list of supported node types and previous generation node types in ElastiCache, see [Supported node types](CacheNodes.SupportedTypes.md)\.

## Migrating nodes on a Memcached cluster<a name="CacheNodes.NodeMigration.Memcached"></a>

To migrate ElastiCache for Memcached to a different node type, you must create a new cluster, which always starts out empty that your application can populate\.

**To migrate your ElastiCache for Memcached cluster node type using the ElastiCache Console:** 
+ Create a new cluster with the new node type\. For more information, see [Creating a Memcached cluster \(console\)](Clusters.Create-mc.md#Clusters.Create.CON.Memcached)\.
+ In your application, update the endpoints to the new cluster's endpoints\. For more information, see [Finding a Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Memcached)
+ Delete the old cluster\. For more information, see [Deleting a cluster](Clusters.Delete.md)

### If nodes are running on EC2\-Classic<a name="CacheNodes.NodeMigration.Memcached.ec2-classic"></a>


|  | 
| --- |
| We are retiring EC2\-Classic on August 15, 2022\. We recommend that you migrate from EC2\-Classic to a VPC\. For more information, see [Migrating an EC2\-Classic cluster into a VPC](Migrating-ec2-classic_to_VPC.md) and the blog [EC2\-Classic Networking is Retiring – Here’s How to Prepare](http://aws.amazon.com/blogs/aws/ec2-classic-is-retiring-heres-how-to-prepare/)\. | 

If your Amazon EC2 nodes are running on EC2\-Classic platform, connect to your Memcached cluster from the Amazon EC2 instance using [ClassicLink](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-classiclink.html)\. 

Follow these steps to verify if your EC2 is running in EC2\-Classic platform \(console\):

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Choose **Instances** in the navigation pane\.

1. Choose your instance and go to **Description**\.

1. If **VPC ID** is blank, the instance is running on the EC2\-Classic platform\.

For information on migrating your EC2\-Classic instance to a VPC, see [Migrating an EC2\-Classic cluster into a VPC](Migrating-ec2-classic_to_VPC.md)\.