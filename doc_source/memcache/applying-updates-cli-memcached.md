# Applying the Service Updates Using the AWS CLI<a name="applying-updates-cli-memcached"></a>

After you receive notification that service updates are available, you can inspect and apply them using the AWS CLI:
+ To retrieve a description of the service updates that are available, run the following command:

  `aws elasticache describe-service-updates --service-update-status available`

  For more information, see [DescribeServiceUpdates](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeServiceUpdates.html)\. 
+ To view update actions that have a `not-applied` or `stopped` status, run the following command: 

  `aws elasticache describe-update-actions --service-update-name sample-service-update --update-action-status not-applied stopped`

  For more information, see [DescribeUpdateActions](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeUpdateActions.html)\. 
+ To apply a service update on a list of replication groups, run the following command: 

  `aws elasticache batch-apply-update-action --service-update-name sample-service-update --cache-cluster-ids my-cachecluster-1 my-cache-cluster-2`

  For more information, see [BatchApplyUpdateAction](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_BatchApplyUpdateAction.html)\. 