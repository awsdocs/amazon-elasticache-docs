# Step 1: Create a cluster<a name="GettingStarted.CreateCluster"></a>

The cluster you create will be live, and not running in a sandbox\. You will incur the standard ElastiCache usage fees for the instance until you delete it\. The total charges will be minimal \(typically less than a dollar\) if you complete the exercise described here in one sitting and delete your cluster when you are finished\. For more information about ElastiCache usage rates, see [Amazon ElastiCache](https://aws.amazon.com/elasticache/)\.

**Important**  
Your cluster is launched in a virtual private cloud \(VPC\) based on the Amazon VPC service\. Before you create your cluster, make sure that you create a subnet group\. For more information, see [Creating a subnet group](SubnetGroups.Creating.md)\.

To work with cluster mode disabled, see the following topics:
+ To use the console, see [Creating a Redis \(cluster mode disabled\) \(Console\)](Clusters.Create.CON.Redis.md)\.
+ To use the AWS CLI, see [Creating a cache cluster for Redis \(cluster mode disabled\) \(AWS CLI\)](Clusters.Create.CLI.md#Clusters.Create.CLI.Redis)\.
+ To use the ElastiCache API, see[Creating a Redis \(cluster mode disabled\) cache cluster \(ElastiCache API\)](Clusters.Create.API.md#Clusters.Create.API.Redis)\.

To work with cluster mode enabled, see the following topics:
+ To use the console, see [Creating a Redis \(cluster mode enabled\) cluster \(Console\)](Clusters.Create.CON.RedisCluster.md)\.
+ To use the AWS CLI, see [Creating a Redis \(cluster mode enabled\) cluster \(AWS CLI\)](Clusters.Create.CLI.md#Clusters.Create.CLI.RedisCluster)\.
+ To use the ElastiCache API, see [Creating a cache cluster in Redis \(cluster mode enabled\) \(ElastiCache API\)](Clusters.Create.API.md#Clusters.Create.API.RedisCluster)\.

You can also use AWS CloudFormation\. For more information, see [AWS::ElastiCache::CacheCluster](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticache-cache-cluster.html)\.