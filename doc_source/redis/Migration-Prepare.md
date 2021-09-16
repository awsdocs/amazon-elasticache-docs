# Preparing your source and target Redis nodes for migration<a name="Migration-Prepare"></a>

You must ensure that all four of the prerequisites mentioned following are satisfied before you start the migration from ElastiCache console, API or AWS CLI\.

**To prepare your source and target Redis Nodes for migration**

1. Identify the target ElastiCache deployment and make sure that you can migrate data to it\. 

   An existing or newly created ElastiCache deployment should meet the following requirements for migration: 
   + It's cluster\-mode disabled using Redis engine version 5\.0\.5 or higher\.
   + It doesn't have either encryption in\-transit or encryption at\-rest enabled\.
   + It has Multi\-AZ enabled\.
   + It has sufficient memory available to fit the data from your Redis on EC2 instance\. To configure the right reserved memory settings, see [Managing Reserved Memory](redis-memory-management.md)\.
   + You can migrate directly from Redis versions 2\.8\.21 onward to Redis version 5\.0\.5 onward if are using the CLI or Redis versions 5\.0\.6 onward using the CLI or console\. We don’t recommend migrating to Redis version 5\.0\.5\. Redis version 5\.0\.6 offers enhanced stability and security\.

1. Make sure that the configurations of your Redis on EC2 and the ElastiCache for Redis deployment are compatible\. 

   At a minimum, all the following in the target ElastiCache deployment should be compatible with your Redis configuration for Redis replication: 
   + Your Redis cluster should be in cluster\-mode disabled configuration\.
   + Your Redis on EC2 instance should not have Redis AUTH enabled\.
   + Redis config `protected-mode` should be set to `no`\.
   + If you have `bind` configuration in your Redis config, then it should be updated to allow requests from ElastiCache nodes\.
   + The number of logical databases should be the same on the ElastiCache node and your Redis on EC2 instance\. This value is set using `databases` in the Redis config\.
   + Redis commands that perform data modification should not be renamed to allow replication of the data to succeed\.
   + To replicate the data from your Redis cluster to ElastiCache, make sure that there is sufficient CPU and memory to handle this additional load\. This load comes from the RDB file created by your Redis cluster and transferred over the network to ElastiCache node\.

1. Make sure that your EC2 instance can connect with ElastiCache by doing the following:
   + Ensure that your EC2 instance's IP address is private\.
   + Assign or create the ElastiCache deployment in the same virtual private cloud \(VPC\) as your Redis on your EC2 instance \(recommended\)\.
   + If the VPCs are different, set up VPC peering to allow access between the nodes\. For more information on VPC peering, see [Access Patterns for Accessing an ElastiCache Cluster in an Amazon VPC](elasticache-vpc-accessing.md)\.
   + The security group attached to your Redis on EC2 instance should allow inbound traffic from ElastiCache nodes\.

1. Make sure that your application can direct traffic to ElastiCache nodes after migration of data is complete\. For more information, see [Access Patterns for Accessing an ElastiCache Cluster in an Amazon VPC](elasticache-vpc-accessing.md)\. 