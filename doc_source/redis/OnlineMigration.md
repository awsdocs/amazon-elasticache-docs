# Online migration to ElastiCache<a name="OnlineMigration"></a>

 By using Online Migration, you can migrate your data from self\-hosted Redis on Amazon EC2 to Amazon ElastiCache\.

**Note**  
Online migration is not supported for clusters running on the r6gd node type\.

## Overview<a name="Migration-Overview"></a>

To migrate your data from Redis running on Amazon EC2 to Amazon ElastiCache requires an existing or newly created Amazon ElastiCache deployment\. The deployment must have a configuration that is ready for migration\. It also should be in line with the configuration that you want, including attributes like instance type and number of replicas\. 

Online migration is designed for data migration from hosted Redis on Amazon EC2 or on\-premise self\-hosted Redis to ElastiCache for Redis and not between ElastiCache for Redis clusters\.

**Important**  
We strongly recommend you read the following sections in their entirety before beginning the online migration process\.

The migration begins when you call the `StartMigration` API operation or AWS CLI command\. The migration process makes the primary node of the ElastiCache for Redis cluster a replica to your source Redis cluster on EC2\. Using Redis replication, data is synced between your source Redis and ElastiCache\. After the data is in sync, you are nearly ready to cut over to ElastiCache\. At this point, you make changes on the application side so your application can call ElastiCache post\-migration\. 

After the client\-side changes are ready, call the `CompleteMigration` API operation\. This API operation promotes your ElastiCache deployment to your primary Redis deployment with primary and replica nodes \(as applicable\)\. Now you can redirect your client application to start writing data to ElastiCache\. Throughout the migration, you can check the status of replication by running the [redis\-cli INFO](https://redis.io/commands/info) command on your Redis on EC2 nodes and on the ElastiCache primary node\. 

## Migration steps<a name="Migration-Steps"></a>

The following topics outline the process for migrating your data:
+ [Preparing your source and target Redis nodes for migration](Migration-Prepare.md)
+ [Starting migration](Migration-Initiate.md)
+ [Verifying the data migration progress](Migration-Verify.md)
+ [Completing the data migration](Migration-Complete.md)