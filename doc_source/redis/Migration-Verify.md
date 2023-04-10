# Verifying the data migration progress<a name="Migration-Verify"></a>

After the data migration has begun, you can do the following to track its progress:
+ Verify that Redis `master_link_status` is `up` in the `INFO` command on ElastiCache primary node\. You can also find this information in the ElastiCache console\. Select the cluster and under **CloudWatch metrics**, observe **Primary Link Health Status**\. After the value reaches 1, the data is in sync\. 
+ You can check that for the ElastiCache replica has an **online** state by running the `INFO` command on your Redis on EC2 instance\. Doing this also provides information about replication lag\.
+ Verify low client output buffer by using the [CLIENT LIST](https://redis.io/commands/client-list) Redis command on your Redis on EC2 instance\.

After the data migration is complete, the data is in sync with any new writes coming to the primary node of your Redis instance\.