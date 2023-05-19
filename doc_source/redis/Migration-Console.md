# Performing online data migration using the Console<a name="Migration-Console"></a>

You can use the AWS Management Console to migrate your data from the EC2 instance to your Redis cluster\. 

**To perform online data migration using the console**

1. Sign in to the console and open the ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/home/home)\.

1. Either create a new Redis cluster or choose an existing cluster\. Make sure that the cluster meets the following requirements:
   + Your Redis engine version should be 5\.0\.6 or higher\. 
   + Your Redis cluster should be in cluster\-mode disabled configuration\.
   + Your Redis on EC2 instance should not have Redis AUTH enabled\.
   + Redis config `protected-mode` should be set to `no`\.
   + If you have `bind` configuration in your Redis config, then it should be updated to allow requests from ElastiCache nodes\.
   + The number of databases should be the same between the ElastiCache node and your Redis on EC2 instance\. This value is set using `databases` in the Redis config\.
   + Redis commands that perform data modification should not be renamed to allow replication of the data to succeed\.
   + To replicate the data from your Redis cluster to ElastiCache, make sure that there is sufficient CPU and memory to handle this additional load\. This load comes from the RDB file created by your Redis cluster and transferred over the network to ElastiCache node\.
   + The cluster is in **available** status\.

1. With your cluster selected, choose **Migrate Data from Endpoint** for **Actions**\. 

1. In the **Migrate Data from Endpoint** dialog box, enter either the IP address or the name of the EC2 instance, and the port where your Redis on EC2 instance is available\.
**Important**  
The IP address must be exact\. If you enter the address incorrectly, the migration fails\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/Migrate-1.png)

1. Choose **Start Migration**\.

   As the cluster begins migration, it changes to **Modifying** and then **Migrating** status\.

1. Monitor the migration progress by choosing **Events** on the navigation pane\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/Migrate-2.png)

At any point during the migration process, you can stop migration\. To do so, choose your cluster and choose **Stop Data Migration** for **Actions**\. The cluster then goes to **Available** status\.

If the migration succeeds, the cluster goes to **Available** status and the event log shows the following:

`Migration operation succeeded for replication group ElastiCacheClusterName.`

If the migration fails, the cluster goes to **Available** status and the event log shows the following:

`Migration operation failed for replication group ElastiCacheClusterName.`