# Stopping the Self\-Service Updates<a name="stopping-self-service-updates"></a>

You can stop updates to Redis clusters if needed\. For example, you might want to stop updates if you have an unexpected surge to your clusters that are undergoing updates\. Or you might want to stop updates if they're taking too long and interrupting your business flow at a peak time\.

The [Stopping](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_BatchApplyStopAction.html) operation immediately interrupts all updates to those clusters and any nodes that are yet to be updated\. It continues to completion any nodes that have an **in progress** status\. However, it ceases updates to other nodes in the same cluster that have an **update available** status and reverts them to a **Stopping** status\.

When the **Stopping** workflow is complete, the nodes that have a **Stopping** status change to a **Stopped** status\. Depending on the workflow of the update, some clusters won't have any nodes updated\. Other clusters might include some nodes that are updated and others that still have an **update available** status\. 

You can return later to finish the update process as your business flows permit\. In this case, choose the applicable clusters that you want to complete updates on, and then choose **Apply Now**\. For more information, see [Applying the Service Updates Using the Console for Memcached](applying-updates-console.md#applying-updates-console-memcached-console)\. 

**Topics**
+ [Stopping the Service Updates Using the Console](stopping-updates-console-memcached.md)
+ [Stopping the Service Updates Using the AWS CLI](stopping-updates-cli-memcached.md)