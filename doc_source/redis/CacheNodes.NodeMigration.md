# Migrating previous generation nodes<a name="CacheNodes.NodeMigration"></a>

Previous generation nodes are node types that are being phased out\. If you have no existing clusters using a previous generation node type, ElastiCache does not support the creation of new clusters with that node type\. If you have existing clusters, you can continue to use them or create new clusters using the previous generation node type\.

Due to the limited amount of previous generation node types, we cannot guarantee a successful replacement when a node becomes unhealthy in your cluster\(s\)\. In such a scenario, your cluster availability may be negatively impacted\.

 We recommend that you migrate your cluster\(s\) to a new node type for better availability and performance\. For a recommended node type to migrate to, see [Upgrade Paths](https://aws.amazon.com/ec2/previous-generation/)\. For a full list of supported node types and previous generation node types in ElastiCache, see [Supported node types](CacheNodes.SupportedTypes.md)\.

## Migrating nodes on a Redis cluster<a name="CacheNodes.NodeMigration.Redis"></a>

The following procedure describes how to migrate your Redis cluster node type using the ElastiCache Console\. During this process, your Redis cluster will continue to serve requests with minimal downtime\. Depending on your cluster configuration you may see the following downtimes\. The following are estimates and may differ based on your specific configurations:
+ Cluster mode disabled \(single node\) may see approximately 60 seconds, primarily due to DNS propagation\.
+ Cluster mode disabled \(with replica node\) may see approximately 1 second for clusters running Redis 5\.0\.6 and above\. All lower version can experience approximately 10 seconds\.
+ Cluster mode enabled may see approximately 1 second\.

**To modify a Redis cluster node type using the console:**

1. Sign in to the Console and open the ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/home)\.

1. From the navigation pane, choose **Redis clusters**\.

1. From the list of clusters, choose the cluster you want to migrate\.

1. Choose **Actions** and then choose **Modify**\.

1. Choose the new node type from the node type list \(if there is not a node type in the list, validate if your are running on EC2\-Classic\. For more information, see [If nodes are running on EC2\-Classic](#Redis.ec2-classic)\.

1. If you want to perform the migration process right away, choose **Apply immediately**\. If **Apply immediately** is not chosen, the migration process is performed during the cluster's next maintenance window\.

1. Choose **Modify**\. If you chose **Apply immediately** in the previous step, the cluster's status changes to **modifying**\. When the status changes to **available**, the modification is complete and you can begin using the new cluster\.

*To modify a Redis cluster node type using the AWS CLI:*

Use the [modify\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group.html) API as shown following:

For Linux, macOS, or Unix:

```
aws elasticache modify-replication-group /
	--replication-group-id my-replication-group /
	--cache-node-type new-node-type /
	--apply-immediately
```

For Windows:

```
aws elasticache modify-replication-group ^
	--replication-group-id my-replication-group ^
	--cache-node-type new-node-type ^
	--apply-immediately
```

In this scenario, the value of *new\-node\-type* is the node type you are migrating to\. By passing the `--apply-immediately` parameter, the update will be applied immediately when the replication group transitions from **modifying** to **available** status\. If **Apply immediately** is not chosen, the migration process is performed during the cluster's next maintenance window\.

**Note**  
If you are unable to modify the cluster with an `InvalidCacheClusterState` error, you need to remove a restore\-failed node first\. For more information, see [Remove restore\-failed\-node](#remove-restore-failed-node)\.  
Once you have completed that process, you can then modify the node type using the previous steps\.

### Remove restore\-failed\-node<a name="remove-restore-failed-node"></a>

 The following procedure describes how to remove restore\-failed node from your Redis cluster\. To remove restore\-failed node \(console\):

1. Sign in to the Console and open the ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/home)\.

1. From the navigation pane, choose **Redis clusters**\.

1. From the list of clusters, choose the cluster you want to remove a node from\.

1. From the list of shards, choose the shard you want to remove a node from\. Skip this step if cluster mode is disabled for the cluster\.

1. From the list of nodes, choose the node with a status of `restore-failed`\.

1. Choose **Actions** and then choose **Delete node**\.

## If nodes are running on EC2\-Classic<a name="Redis.ec2-classic"></a>


|  | 
| --- |
| We are retiring EC2\-Classic on August 15, 2022\. We recommend that you migrate from EC2\-Classic to a VPC\. For more information, see [Migrating an EC2\-Classic cluster into a VPC](Migrating-ec2-classic_to_VPC.md) and the blog [EC2\-Classic Networking is Retiring – Here’s How to Prepare](http://aws.amazon.com/blogs/aws/ec2-classic-is-retiring-heres-how-to-prepare/)\. | 

Follow these steps to verify if your EC2 is running in EC2\-Classic platform \(console\):

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Choose **Instances** in the navigation pane\.

1. Choose your instance and go to **Description**\.

1. If **VPC ID** is blank, the instance is running on the EC2\-Classic platform\.

For information on migrating your EC2\-Classic instance to a VPC, see [Migrating an EC2\-Classic cluster into a VPC](Migrating-ec2-classic_to_VPC.md)\.