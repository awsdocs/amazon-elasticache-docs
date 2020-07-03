# Completing the Data Migration<a name="Migration-Complete"></a>

When you are ready to cut over to the ElastiCache cluster, use the `complete-migration` CLI command with the following parameters:
+ `--replication-group-id` – The identifier for the replication group\.
+ `--force` – A value that forces the migration to stop without ensuring that data is in sync\.

The following is an example\.

```
aws elasticache complete-migration --replication-group-id test-cluster
```

As you run this command, the ElastiCache primary node stops replicating from your Redis instance and promotes it to primary\. This promotion typically completes within minutes\. To confirm the promotion to primary, check for the event `Complete Migration successful for test-cluster`\. At this point, you can direct your application to ElastiCache writes and reads\. ElastiCache cluster status should change from **migrating** to **available**\.

If the promotion to master fails, the ElastiCache primary node continues to replicate from your Redis on EC2 instance\. The ElastiCache cluster continues to be in **migrating** status, and a replication group event message about the failure is sent\. To troubleshoot this failure, look at the following:
+ Check the replication group event\. Use specific information from the event to fix the failure\.
+ You might get an event message about data not in sync\. If so, make sure that the ElastiCache primary can replicate from your Redis on EC2 instance and both are in sync\. If you still want to stop the migration, you can run the preceding command with the `—force` option\.
+ You might get an event message if one of the ElastiCache nodes is undergoing a replacement\. You can retry the complete the migration step after the replacement is complete\.