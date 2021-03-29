# Applying the self\-service updates<a name="applying-updates"></a>

You can start applying the service updates to your Redis fleet from the time that the updates have an **available** status until they have an **expired** status\. Service updates of the type **security** are cumulative\. In other words, any nonexpired updates that you haven't applied yet are included with your latest update\.

**Note**  
You can apply only those service updates that have an **available** status, even if the recommended apply by date is past due\. 

For more information about reviewing your Redis fleet and applying any service\-specific updates to applicable Redis clusters, see [Applying the service updates using the console for Redis](#applying-updates-console-redis-console)\.

When a new service update is available for one or more of your Redis clusters, you can use the ElastiCache console, API, or AWS CLI to apply the update\. The following sections explain the options that you can use to apply updates\.

## Applying the service updates using the console<a name="applying-updates-console"></a>

You can apply the service updates using one of the following console options\. 

**Topics**
+ [Applying the service updates using the console for Redis](#applying-updates-console-redis-console)
+ [Applying the service updates using the service updates list](#applying-updates-elasticache-update-console-redis)

### Applying the service updates using the console for Redis<a name="applying-updates-console-redis-console"></a>

Choose this to view **Update Status** for individual Redis clusters, and then choose **Apply**, **View**, or **Stop** for the service updates\. If a service update is available, the console displays a banner at the top of the **Redis** page, as shown following\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/redis-self-service-sample-start.png)

If you choose **Apply Now**, you can choose to apply the service update to all or a subset of the applicable clusters in this workflow, as shown following\. 

**Note**  
If you choose **Dismiss**, the console stops displaying the banner for that console session\. However, the banner reappears the next time that you refresh your session\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/service-update-apply-now.png)

On the **Apply Updates Now** page, you can use these options:
+ **Auto\-Update after Due Date**: If this attribute is **yes**, after the **Recommended apply by Date** has passed, ElastiCache schedules clusters yet to be updated in the appropriate maintenance window\. Updates are applied along with other applicable updates\. You can continue to apply the updates until the update expiration date\. 

  If this attribute is **no** and you don't apply the self\-service update before it expires, ElastiCache doesn't automatically apply the service update for you\. If your cluster is part of one or more ElastiCache\-supported compliance programs and isn't updated, it remains out of compliance until the next cumulative update becomes available\. 
+ The **Nodes Updated** ratio value for your Redis cluster and the **Estimated Update Time** value help you plan your maintenance schedule\. If service updates exceed the estimated time constraints for your business flows, you can stop updates and reapply them at a later date\. For more information, see [Stopping the self\-service updates](stopping-self-service-updates.md)\.
+ If you choose to apply the service updates to any or all available Redis clusters, choose **Confirm**\. If you choose this, you then view the **Service Updates** page, where you can monitor the status of your service update\.
+ If you choose **Cancel**, you can explore further options, as explained following\. 

On the ElastiCache dashboard, you can check **Update Status** for each of your Redis clusters, as shown following\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/redis-self-service-sample.png)

**Update Status** displays one of the following:
+ **update available**: An update is available to apply to this cluster\.
+ **not\-applied**: An update is available but not yet applied\.
+ **scheduling**: The update date is being scheduled\.
+ **scheduled**: The update date has been scheduled\.
+ **waiting\-to\-start**: The update process will soon begin\.
+ **in\-progress**: The update is being applied to this cluster, rendering it unavailable for the duration of the value in **Estimated Update Time**\.
+ **stopping**: An in\-progress update has been interrupted before completion\.
+ **stopped**: The update has been terminated\.

  If you stop an in\-progress update on a Redis cluster, some nodes might be updated while others are not\. The **stopping** process doesn't roll back any changes to already updated nodes\. You can reapply the update to those nodes that still have an **available** status at your convenience if the update doesn't have an **Expired** status\.
+ **complete**: The update has been successfully applied\.
+ **up to date**: The cluster doesn't have any outstanding active service updates and your cluster is compliant\. For more information about compliance, see [Self\-service security updates for compliance](elasticache-compliance.md#elasticache-compliance-self-service)\.

### Applying the service updates using the service updates list<a name="applying-updates-elasticache-update-console-redis"></a>

To view the list of individual service updates and their status, along with other information, choose the **Service Updates List** tab\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/service-update-available.png)

In **Service Updates List**, you can view the following:
+ **Service Update Name**: A unique identifier for the service update\.
+ **Status**: The status of the update, which is one of the following:
  + **available:** The update is available for requisite Redis clusters\.
  + **complete:** The update has been applied and all Redis clusters are compliant\. For more information, see [Self\-service security updates for compliance](elasticache-compliance.md#elasticache-compliance-self-service)\.
  + **cancelled:** The update has been canceled and is no longer necessary\.
  + **expired:** The update is no longer available to apply\.
+ **Severity**: The priority of applying the update:
  + **critical:** We recommend that you apply this update immediately \(within 14 days or less\)\.
  + **important:** We recommend that you apply this update as soon as your business flow allows \(within 30 days or less\)\.
  + **medium:** We recommend that you apply this update as soon as you can \(within 60 days or less\)\.
  + **low:** We recommend that you apply this update as soon as you can \(within 90 days or less\)\.
+ ** Update Type**: For this version, only security updates are supported\.
+ ** Release Date**: When the update is released and available to apply on your Redis fleet\.
+ ** Recommended Apply By Date**: ElastiCache guidance date to apply the updates by\.

Choosing an individual update provides additional details, including the following:
+ **Update Description:** Provides details on the service update\.
+ **Update Expiration Date:** The date when the service update expires and no longer is available\. Any updates that aren't applied before their expiration date are cumulatively rolled into the next update\.

**Important**  
We strongly recommend that you apply updates of type **security** as soon as you can\. Doing this helps ensure that your Redis clusters are always up\-to\-date with the latest security patches and are compliant\. For more information, see [Self\-service security updates for compliance](elasticache-compliance.md#elasticache-compliance-self-service)\.

To view the list of individual service updates in relation to the applicable Redis clusters, choose the **Service Update Status** tab\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/service-update-available-status.png)

In the **Service Updates Status** list, you can view the following:
+ **Service Update Name**: Detailed information about the service update\.
+ **Cluster Name**: The list of your Redis clusters that are eligible for the update\.
+ **Nodes Updated:** The ratio of individual nodes within a specific cluster that were updated or remain available for the specific service update\.
+ **Update Severity**: The priority of applying the update:
  + **critical:** We recommend that you apply this update immediately \(within 14 days or less\)\.
  + **important:** We recommend that you apply this update as soon as you can \(within 30 days or less\)\.
  + **medium:** We recommend that you apply this update as soon as you can \(within 60 days or less\)\.
  + **low:** We recommend that you apply this update as soon as you can \(within 90 days or less\)\.
+ ** Update Type**: For this version, only security updates are supported\.
+ **Service Update Status**: The status of the update, which is one of the following:
  + **available:** The update is available for requisite Redis clusters\.
  + **complete:** The update has been applied and all Redis clusters are [compliant](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/elasticache-compliance-self-service.html)\.
  + **canceled:** The update has been canceled and is no longer necessary\.
  + **expired:** The update is no longer available to apply\.
+ **Service Update SLA Met**: This reflects whether your cluster is [compliant](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/elasticache-compliance-self-service.html)\.
  + **yes**: All available updates have been applied to this cluster and available nodes by the apply\-by date\. 
  + **no**: The service update might have been applied successfully to one or more nodes, but other nodes within the cluster still have an **available** status\. This typically happens when a service update is applied and then stopped\. 
**Note**  
If you stop the progress of a service update on a cluster, any nodes that are already updated have a **complete** status\. Any nodes that have an **In Progress** or **Stopping** status revert to a **Stopped** status, and the **Service Update SLA Met** status changes to **no**\. 
  + **N/A**: The replication group was created after the recommended apply\-by date\. 
+ **Cluster Status Modified Date**: The latest date that the cluster was modified with a service update\.

**Note**  
The **Show Previous Updates** check box, if selected, displays a list of previous updates that are no longer available\.

## Applying the service updates using the AWS CLI<a name="applying-updates-cli-redis"></a>

After you receive notification that service updates are available, you can inspect and apply them using the AWS CLI:
+ To retrieve a description of the service updates that are available, run the following command:

  `aws elasticache describe-service-updates --service-update-status available`

  For more information, see [DescribeServiceUpdates](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeServiceUpdates.html)\. 
+ To view update actions that have a `not-applied` or `stopped` status, run the following command:

  `aws elasticache describe-update-actions --service-update-name sample-service-update --update-action-status not-applied stopped`

  For more information, see [DescribeUpdateActions](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeUpdateActions.html)\. 
+ To apply a service update on a list of replication groups, run the following command:

  `aws elasticache batch-apply-update-action --service-update-name sample-service-update --replication-group-ids my-replication-group-1 my-replication-group-2`

  For more information, see [BatchApplyUpdateAction](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_BatchApplyUpdateAction.html)\. 