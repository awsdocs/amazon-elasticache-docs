# Starting Migration<a name="Migration-Initiate"></a>

After all prerequisites are complete, you can begin data migration using the AWS Management Console, ElastiCache API, or AWS CLI\. The following example shows using the CLI\.

Start migration by calling the `start-migration` command with the following parameters:
+ `--replication-group-id` – Identifier of the target ElastiCache replication group
+ `--customer-node-endpoint-list` – A list of endpoints with either DNS or IP addresses and the port where your source Redis on EC2 cluster is running\. Because ElastiCache currently only supports cluster\-mode disabled configuration, this list should contain one entry\. If you have enabled chained replication, the endpoint can point to a replica instead of the primary node in your Redis cluster\. 

The following is an example using the CLI\.

```
aws elasticache start-migration --replication-group-id test-cluster --customer-node-endpoint-list "Address='10.0.0.241',Port=6379"
```

As you run this command, the ElastiCache primary node configures itself to become a replica of your Redis on EC2 instance\. The status of ElastiCache cluster changes to **migrating** and data starts migrating from your Redis on EC2 instance to the ElastiCache primary node\. Depending on the size of the data and load on your Redis instance, the migration can take a while to complete\. You can check the progress of the migration by running the [redis\-cli INFO](https://redis.io/commands/info) command on your Redis on EC2 instance and ElastiCache primary node\.

After successful replication, all writes to your Redis on EC2 instance propagate to the ElastiCache cluster\. You can use ElastiCache nodes for reads\. However, you can't write to the ElastiCache cluster\. If the ElastiCache primary node has other replica nodes connected to it, these replica nodes continue to replicate from the ElastiCache primary node\. This way, all the data from your Redis on EC2 instance gets replicated to all the nodes in ElastiCache cluster\.

If the ElastiCache primary node can't become a replica of your Redis on EC2 instance, it retries several times before eventually promoting itself back to primary\. The status of ElastiCache cluster then changes to **available**, and a replication group event about the failure to initiate the migration is sent\. To troubleshoot such a failure, check the following:
+ Look at the replication group event\. Use any specific information from the event to fix the migration failure\.
+ If the event doesn’t provide any specific information, make sure that you have followed the guidelines in [Preparing Your Source and Target Redis Nodes for Migration](Migration-Prepare.md)\.
+ Ensure that the routing configuration for your VPC and subnets allows traffic between ElastiCache nodes and your Redis on EC2 instance\.
+ Ensure the security group attached to your Redis on EC2 instance allows input bound traffic from ElastiCache nodes\.
+ Check Redis logs for your Redis on EC2 instance for more information about failures specific to replication\.