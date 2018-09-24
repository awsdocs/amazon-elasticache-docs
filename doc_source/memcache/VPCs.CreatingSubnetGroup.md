# Creating a Cache Subnet Group<a name="VPCs.CreatingSubnetGroup"></a>

A *cache subnet group* is a collection of subnets that you may want to designate for your cache clusters in an Amazon VPC\. When launching a cache cluster in an Amazon VPC, you need to select a cache subnet group\. Then ElastiCache uses that cache subnet group to assign IP addresses within that subnet to each cache node in the cluster\.

For guidance on how to create a subnet group using the ElastiCache Management Console, the AWS CLI, or the ElastiCache API, see [Creating a Subnet Group](SubnetGroups.Creating.md)\.

After you create a cache subnet group, you can launch a cache cluster to run in your Amazon VPC\. Continue to the next topic [Creating a Cache Cluster in an Amazon VPC](VPCs.CreatingCacheCluster.md)\. 