# Applying the service updates using the console<a name="applying-updates-console"></a>

You can apply the service updates using one of the following console options\. 

**Topics**
+ [Applying the service updates using the console for Memcached](#applying-updates-console-memcached-console)
+ [Applying the service updates using the service updates list](#applying-updates-elasticache-update-console-memcached)

## Applying the service updates using the console for Memcached<a name="applying-updates-console-memcached-console"></a>

Choose this to view **Update Status** for individual Memcached clusters, and then choose **Apply**, **View**, or **Stop** for the service updates\. If a service update is available, the console displays a banner at the top of the **Memcached** page, as shown following\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/mem-update-available.png)

If you choose **Apply Now**, you can choose to apply the service update to all or a subset of the applicable clusters in this workflow, as shown following\. 

**Note**  
If you choose **Dismiss**, the console stops displaying the banner for that console session\. However, the banner reappears the next time that you refresh your session\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/mem-self-service.png)

On the **Apply Updates Now** page, you can use these options:
+ **Auto\-Update after Due Date**: If this attribute is **yes**, after the **Recommended apply by Date** has passed, ElastiCache schedules clusters yet to be updated in the appropriate maintenance window\. Updates are applied along with other applicable updates\. You can continue to apply the updates until the update expiration date\. 

  If this attribute is **no** and you don't apply the self\-service update before it expires, ElastiCache doesn't automatically apply the service update for you\. Those clusters must wait until the next cumulative update becomes available\.
+ The **Nodes Updated** ratio value for your Memcached cluster and the **Estimated Update Time** value help you plan your maintenance schedule\. If service updates exceed the estimated time constraints for your business flows, you can stop updates and reapply them at a later date\. For more information, see [Stopping the self\-service updates](stopping-self-service-updates.md)\.
+ If you choose to apply the service updates to any or all available Memcached clusters, choose **Confirm**\. If you choose this, you then view the **Service Updates** page, where you can monitor the status of your service update\.
+ If you choose **Cancel**, you can explore further options, as explained following\.

On the ElastiCache dashboard, you can check **Update Status** for each of your Redis clusters, as shown following\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/mem-update-status.png)

**Update Status** displays one of the following:
+ **update available**: An update is available to apply to this cluster\.
+ **not\-applied**: An update is available but not yet applied\.
+ **scheduling**: The update date is being scheduled\.
+ **scheduled**: The update date has been scheduled\.
+ **waiting\-to\-start**: The update process will soon begin\.
+ **in\-progress**: The update is being applied to this cluster, rendering it unavailable for the duration of the **Estimated Update Time**\.
+ **stopping**: An in\-progress update has been interrupted before completion\.
+ **stopped**: The update has been terminated\.

  If you stop an in\-progress update on a Memcached cluster, some nodes might be updated while others are not\. The **stopping** process doesn't roll back any changes to already updated nodes\. You can reapply the update to those nodes that still have an **available** status at your convenience if the update doesn't have an **Expired** status\.
+ **complete**: The update has been successfully applied\.
+ **up to date**: The cluster doesn't have any outstanding active service updates\.

## Applying the service updates using the service updates list<a name="applying-updates-elasticache-update-console-memcached"></a>

To view the list of individual service updates and their status, along with other information, choose the **Service Updates List** tab\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/mem-update-available-list.png)

In **Service Updates List**, you can view the following:
+ **Service Update Name** A unique identifier for the service update\.
+ **Status**: The status of the update, which is one of the following:
  + **available:** The update is available for requisite Memcached clusters\.
  + **complete:** The update has been applied and all Memcached clusters are up to date\. 
  + **cancelled:** The update has been canceled and is no longer necessary\.
  + **expired:** The update is no longer available to apply\.
+ **Severity**: The priority of applying the update:
  + **critical:** We recommend that you apply this update immediately \(within 14 days or less\)\.
  + **important:** We recommend that you apply this update as soon as you can \(within 30 days or less\)\.
  + **medium:** We recommend that you apply this update as soon as you can \(within 60 days or less\)\.
  + **low:** We recommend that you apply this update as soon as you can \(within 90 days or less\)\.
+ ** Update Type**: For this version, only security updates are supported\.
+ ** Release Date**: When the update is released and available to apply on your Memcached fleet\.
+ ** Recommended Apply By Date**: ElastiCache guidance date to apply the updates by\.

Choosing an individual update provides additional details, including the following:
+ **Update Description:** Provides details on the service update\.
+ **Update Expiration Date:** The date when the service update expires and no longer is available\. Any updates that aren't applied before their expiration date are cumulatively rolled into the next update\.

To review the list of individual service updates in relation to the applicable Memcached clusters, choose the **Service Update Status** tab\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/mem-update-available-list-applied.png)

In the **Service Updates Status** list, you can view the following:
+ **Service Update Name**: Detailed information about the service update\.
+ **Cluster Name**: The list of your Memcached clusters that are eligible for the update\.
+ **Nodes Updated:** The ratio of individual nodes within a specific cluster that were updated or remain available for the specific service update\.
+ **Update Severity**: The priority of applying the update:
  + **critical:** We recommend that you apply this update immediately \(within 14 days or less\)\.
  + **important:** We recommend that you apply this update as soon as you can \(within 30 days or less\)\.
  + **medium:** We recommend that you apply this update as soon as you can \(within 60 days or less\)\.
  + **low:** We recommend that you apply this update as soon as you can \(within 90 days or less\)\.
+ ** Update Type**: For this version, only security updates are supported\.
+ **Service Update Status**: The status of the update, which is one of the following:
  + **available:** The update is available for requisite Memcached clusters\.
  + **complete:** The update has been applied and all Memcached clusters are updated\.
  + **canceled:** The update has been canceled and is no longer necessary\.
  + **expired:** The update is no longer available to apply\.
+ **Service Update SLA Met**: This reflects whether your cluster is compliant\.
  + **yes**: All available updates have been applied to this cluster and available nodes\. 
  + **no**: The service update might have been applied successfully to one or more nodes, but other nodes within the cluster still have an **available** status\. This typically happens when a service update is applied and then stopped\. 
**Note**  
If you stop the progress of a service update on a cluster, any nodes that are already updated have a **complete** status\. Any nodes that have an **In Progress** or **Stopping** status revert to a **Stopped** status, and the **Service Update SLA Met** status changes to **no**\. 
+ **Cluster Status Modified Date**: The latest date that the update status was changed for a cluster\.

**Note**  
The **Show Previous Updates** check box, if selected, displays a list of previous updates that are no longer available\.