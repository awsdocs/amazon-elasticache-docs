# Stopping the Service Updates Using the AWS CLI<a name="stopping-updates-cli-memcached"></a>

You can interrupt a service update using the AWS CLI\. The following code example shows how to do this\.

`aws elasticache batch-stop-update-action --service-update-name sample-service-update --cache-group-ids my-cache-group-1 my-cache-group-2`

For more information, see [BatchStopUpdateAction](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_BatchStopUpdateAction.html)\. 