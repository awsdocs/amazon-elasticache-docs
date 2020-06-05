# Amazon VPCs and ElastiCache Security<a name="VPCs"></a>

Because data security is important, ElastiCache provides means for you to control who has access to your data\. How you control access to your data is dependent upon whether or not you launched your clusters in an Amazon Virtual Private Cloud \(Amazon VPC\) or Amazon EC2\-Classic\.

**Important**  
We have deprecated the use of Amazon EC2\-Classic for launching ElastiCache clusters\. All current generation nodes are launched in Amazon Virtual Private Cloud only\.

**Topics**
+ [Understanding ElastiCache and Amazon VPCs](VPCs.EC.md)
+ [Access Patterns for Accessing an ElastiCache Cluster in an Amazon VPC](elasticache-vpc-accessing.md)
+ [Creating a Virtual Private Cloud \(VPC\)](VPCs.CreatingVPC.md)
+ [Creating a Replication Group in an Amazon VPC](VPCs.CreatingReplGroup.md)
+ [Connecting to a Cluster or Replication Group Running in an Amazon VPC](VPCs.Connecting.md)

The Amazon Virtual Private Cloud \(Amazon VPC\) service defines a virtual network that closely resembles a traditional data center\. When you configure your Amazon VPC you can select its IP address range, create subnets, and configure route tables, network gateways, and security settings\. You can also add a cache cluster to the virtual network, and control access to the cache cluster by using Amazon VPC security groups\. 

This section explains how to manually configure an ElastiCache cluster in an Amazon VPC\. This information is intended for users who want a deeper understanding of how ElastiCache and Amazon VPC work together\.

**Topics**
+ [Understanding ElastiCache and Amazon VPCs](VPCs.EC.md)
+ [Access Patterns for Accessing an ElastiCache Cluster in an Amazon VPC](elasticache-vpc-accessing.md)
+ [Creating a Virtual Private Cloud \(VPC\)](VPCs.CreatingVPC.md)
+ [Creating a Replication Group in an Amazon VPC](VPCs.CreatingReplGroup.md)
+ [Connecting to a Cluster or Replication Group Running in an Amazon VPC](VPCs.Connecting.md)