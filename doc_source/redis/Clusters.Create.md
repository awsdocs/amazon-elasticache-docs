# Creating a Cluster<a name="Clusters.Create"></a>

Following, you can find instructions on creating a cluster using the ElastiCache console, the AWS CLI, or the ElastiCache API\.

You can also create an ElastiCache cluster using [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)\. For more information, see [ AWS::ElastiCache::CacheCluster](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-elasticache-cache-cluster.html) in the *AWS Cloud Formation User Guide*, which includes guidance on how to implement that approach\.

Whenever you create a cluster or replication group, it is a good idea to do some preparatory work so you won't need to upgrade or make changes right away\.

**Topics**
+ [Determine Your Requirements](cluster-create-determine-requirements.md)
+ [Choosing Your Node Size](nodes-select-size.md)
+ [Creating a Redis \(cluster mode disabled\) Cluster \(Console\)](Clusters.Create.CON.Redis.md)
+ [Creating a Redis \(Cluster Mode Enabled\) Cluster \(Console\)](Clusters.Create.CON.RedisCluster.md)
+ [Creating a Cluster \(AWS CLI\)](Clusters.Create.CLI.md)
+ [Creating a Cluster \(ElastiCache API\)](Clusters.Create.API.md)