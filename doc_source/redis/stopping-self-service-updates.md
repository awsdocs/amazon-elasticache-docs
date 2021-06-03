# Stopping the self\-service updates<a name="stopping-self-service-updates"></a>

You can stop updates to Redis clusters if needed\. For example, you might want to stop updates if you have an unexpected surge to your Redis clusters that are undergoing updates\. Or you might want to stop updates if they're taking too long and interrupting your business flow at a peak time\.

The [Stopping](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_BatchApplyStopAction.html) operation immediately interrupts all updates to those clusters and any nodes that are yet to be updated\. It continues to completion any nodes that have an **in progress** status\. However, it ceases updates to other nodes in the same cluster that have an **update available** status and reverts them to a **Stopping** status\.

When the **Stopping** workflow is complete, the nodes that have a **Stopping** status change to a **Stopped** status\. Depending on the workflow of the update, some clusters won't have any nodes updated\. Other clusters might include some nodes that are updated and others that still have an **update available** status\. 

You can return later to finish the update process as your business flows permit\. In this case, choose the applicable clusters that you want to complete updates on, and then choose **Apply Now**\. For more information, see [Applying the service updates using the console for Redis](applying-updates.md#applying-updates-console-redis-console)\. 

## Stopping the service updates using the console<a name="stopping-updates-console-redis"></a>

You can interrupt a service update using the Redis console\. The following demonstrates how to do this:
+ After a service update has progressed on a selected Redis cluster, the ElastiCache console displays the **View/Stop Update** tab at the top of the Redis dashboard\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ssp-view-stop.png)
+ To interrupt the update, choose **Stop Update**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ssp-stop-1.png)
+ When you stop the update, choose the Redis cluster and examine the status\. It reverts to a **Stopping** status, as shown following, and eventually a **Stopped** status\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ssp-stopping.png)

## Stopping the service updates using the AWS CLI<a name="stopping-updates-cli-redis"></a>

You can interrupt a service update using the AWS CLI\. The following code example shows how to do this\.

`aws elasticache batch-stop-update-action --service-update-name sample-service-update --replication-group-ids my-replication-group-1 my-replication-group-2`

For more information, see [BatchStopUpdateAction](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_BatchStopUpdateAction.html)\. 