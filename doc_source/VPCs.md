# ElastiCache and Security Groups<a name="VPCs"></a>

Because data security is important, ElastiCache provides means for you to control who has access to your data\. How you control access to your data is dependent upon whether or not you launched your clusters in an Amazon Virtual Private Cloud \(Amazon VPC\) or Amazon EC2\-Classic\.


+ [Amazon Virtual Private Cloud: Amazon VPC Security Groups](#AccessControl.AmazonVPC)
+ [Amazon EC2\-Classic: ElastiCache Security Groups](#AccessControl.NotVPC)

## Amazon Virtual Private Cloud: Amazon VPC Security Groups<a name="AccessControl.AmazonVPC"></a>

When running your clusters in an Amazon Virtual Private Cloud, you configure your Amazon VPC by choosing its IP address range, creating subnets, and configuring route tables, network gateways, and security settings\. You can also add a cache cluster to the virtual network, and control access to the cache cluster by using Amazon VPC security groups, which should not be confused with Amazon ElastiCache security groups\. For more information, see [Amazon Virtual Private Cloud \(Amazon VPC\) with ElastiCache](AmazonVPC.md)\.

## Amazon EC2\-Classic: ElastiCache Security Groups<a name="AccessControl.NotVPC"></a>

Amazon ElastiCache allows you to control access to your clusters using ElastiCache cache security groups\. An ElastiCache cache security group acts like a firewall, controlling network access to your cluster\. By default, network access is turned off to your clusters\. If you want your applications to access your cluster, you must explicitly enable access from hosts in specific Amazon EC2 security groups\. For more information, see [Security Groups \[EC2\-Classic\]](SecurityGroups.md) \.