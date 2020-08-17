# Create a Cluster<a name="GettingStarted.CreateCluster"></a>

The cluster you're about to launch will be live, and not running in a sandbox\. You will incur the standard ElastiCache usage fees for the instance until you delete it\. The total charges will be minimal \(typically less than a dollar\) if you complete the exercise described here in one sitting and delete your cluster when you are finished\. For more information about ElastiCache usage rates, see [Amazon ElastiCache](https://aws.amazon.com/elasticache/)\.

**Important**  
Your cluster is launched in a virtual private cloud \(VPC\) based on the Amazon VPC service\. Before you create your cluster, make sure that you create a subnet group\. For more information, see [Creating a Subnet Group](SubnetGroups.Creating.md)\.

To work with cluster mode disabled, see the following topics:
+ To use the console, see [Creating a Redis \(cluster mode disabled\) Cluster \(Console\)](Clusters.Create.CON.Redis.md)\.
+ To use the AWS CLI, see [Creating a Cache Cluster for Redis \(Cluster Mode Disabled\) \(AWS CLI\)](Clusters.Create.CLI.md#Clusters.Create.CLI.Redis)\.
+ To use the ElastiCache API, see[Creating a Redis \(Cluster Mode Disabled\) Cache Cluster \(ElastiCache API\)](Clusters.Create.API.md#Clusters.Create.API.Redis)\.

To work with cluster mode enabled, see the following topics:
+ To use the console, see [Creating a Redis \(Cluster Mode Enabled\) Cluster \(Console\)](Clusters.Create.CON.RedisCluster.md)\.
+ To use the AWS CLI, see [Creating a Redis \(Cluster Mode Enabled\) Cluster \(AWS CLI\)](Clusters.Create.CLI.md#Clusters.Create.CLI.RedisCluster)\.
+ To use the ElastiCache API, see [Creating a Cache Cluster in Redis \(Cluster Mode Enabled\) \(ElastiCache API\)](Clusters.Create.API.md#Clusters.Create.API.RedisCluster)\.

You can also use AWS CloudFormation\. For more information, see [AWS::ElastiCache::CacheCluster](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticache-cache-cluster.html)\.