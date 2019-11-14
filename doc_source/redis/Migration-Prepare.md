# Preparing Your Source and Target Clusters for Migration<a name="Migration-Prepare"></a>

Use the following procedure to prepare for migration\.

**To prepare your source and target clusters for migration**

1. Identify the target ElastiCache cluster and make sure that you can migrate data to it\. 

   An existing or newly created ElastiCache cluster should meet the following requirements for migration: 
   + It's cluster\-mode disabled using Redis engine version 5\.0\.5 or higher\.
   + It doesn't have either encryption in transit or encryption at rest enabled\.
   + It has Multi\-AZ with Auto\-Failover enabled\.
   + It has sufficient memory available to fit the data from your Redis instance\. To configure the right reserved memory settings, see [Managing Reserved Memory](redis-memory-management.md)\.

1. Make sure that the configurations of ElastiCache and your Redis cluster are compatible\. 

   At a minimum, all the following in the target ElastiCache cluster should be compatible with your Redis configuration for Redis replication: 
   + Your Redis engine version should be at least 2\.8\.21 \(or higher\)\.
   + Your Redis cluster should be in cluster\-mode disabled configuration\.
   + You Redis instance should not have Redis AUTH enabled\.
   + Redis config `protected-mode` should be set to `no`\.
   + If you have `bind` configuration in your Redis config, then it should be updated to allow requests from ElastiCache nodes\.
   + The number of databases should be the same between the ElastiCache node and your Redis instance\. This value is set using `databases` in the Redis config\.
   + Redis commands that perform data modification should not be renamed to allow replication of the data to succeed\.
   + To replicate the data from your Redis cluster to ElastiCache, make sure that there is sufficient CPU and memory to handle this additional load\. This load comes from the RDB file created by your Redis cluster and transferred over the network to ElastiCache node\.

1. Make sure that your EC2 instance can connect with ElastiCache by doing the following:
   + Ensure that your EC2 instance's IP address is private\.
   + Assign or create the ElastiCache cluster in the same VPC as your Redis instance \(recommended\)\.
   + If the VPCs are different, set up VPC peering to allow access between the two clusters\. For more information on VPC peering, see [Access Patterns for Accessing an ElastiCache Cluster in an Amazon VPC](elasticache-vpc-accessing.md)\.
   + The security group attached to your Redis instance should allow inbound traffic from ElastiCache nodes\.

1. Make sure that your application can direct traffic to ElastiCache nodes after migration of data is complete\. For more information, see [Access Patterns for Accessing an ElastiCache Cluster in an Amazon VPC](elasticache-vpc-accessing.md)\. 