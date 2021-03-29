# Access authorization<a name="GettingStarted.AuthorizeAccess"></a>

 This section assumes that you are familiar with launching and connecting to Amazon EC2 instances\. For more information, see the *[Amazon EC2 Getting Started Guide](https://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)*\. 

All ElastiCache clusters are designed to be accessed from an Amazon EC2 instance\. The most common scenario is to access an ElastiCache cluster from an Amazon EC2 instance in the same Amazon Virtual Private Cloud \(Amazon VPC\)\. This is the scenario covered in this topic\. For information on accessing your ElastiCache cluster from a different Amazon VPC, a different AWS Region, or even your corporate network, see the following:
+ [Access Patterns for Accessing an ElastiCache Cluster in an Amazon VPC](elasticache-vpc-accessing.md)
+ [Accessing ElastiCache resources from outside AWS](accessing-elasticache.md#access-from-outside-aws)

By default, network access to your cluster is limited to the user account that was used to create it\. Before you can connect to a cluster from an EC2 instance, you must authorize the EC2 instance to access the cluster\. The steps required depend upon whether you launched your cluster into EC2\-VPC or EC2\-Classic\.

For the steps to authorize access to your cluster, see [Accessing your cluster or replication group](accessing-elasticache.md)\.