# Applying the Service Updates Using the Console<a name="applying-updates-console"></a>

You can apply the service updates using one of the following console options\. ElastiCache provides you two different perspectives to help you decide how and when to apply the updates:

**Topics**
+ [Applying the Service Updates Using the Console for Memcached](#applying-updates-console-memcached-console)
+ [Applying the Service Updates Using the Service Updates List](#applying-updates-elasticache-update-console-memcached)

## Applying the Service Updates Using the Console for Memcached<a name="applying-updates-console-memcached-console"></a>

Choose this to review the **Update Status** of individual Memcached clusters, and then choose **Apply**, **View**, or **Stop** for the service updates\. If a service update is available, the console displays a banner at the top of the **Memcached** page, as shown following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/mem-update-available.png)
+ If you choose **Apply Now**, you can choose to apply the service update to all or a subset of the applicable clusters in this workflow, as shown following: 
**Note**  
If you choose **Dismiss**, the console stops displaying the banner for that console session\. However, the banner reappears the next time that you refresh your session\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/mem-self-service.png)

  Be aware of the following about the **Apply Updates Now** page:
  + **Auto\-Update after Due Date**: If you choose not to apply the self\-service update before it expires, any clusters or individual nodes that aren't updated remain out of compliance until the next cumulative update is available\. ElastiCache doesn't automatically apply the service update on your behalf\.
  + The ratio of **Nodes Updated** on your Memcached cluster and the **Estimated Update Time** allow you to plan your maintenance schedule\. If service updates exceed the estimated time constraints for your business flows, you have the option to stop them and re\-apply them at a later date\. For more information, see [Stopping the Self\-Service Updates](stopping-self-service-updates.md)\.
  + If you choose to apply the service updates to any or all available Memcached clusters, choose **Confirm**\. If you choose this, you can then view the **Service Updates** page, where you can monitor the status of your service update\.
  + If you choose **Cancel**, you can explore further options, as explained following:

You can inspect your Memcached clusters on an individual basis to determine their **Update Status**\. The following lets you know the compliance status of your clusters with regard to available service updates\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/mem-update-status.png)

**Update Status** displays one of the following:
+ **update available**: An update is available to apply to this cluster\.
+ **in\-progress**: The update is being applied to this cluster, rendering it unavailable for business flows\.
+ **stopping**: An in\-progress update has been interrupted before completion\.
+ **stopped**: The update has been terminated\.
**Note**  
If you stop an in\-progress update on a Memcacheed cluster, some nodes might be updated while others are not\. The **stopping** process doesn't roll back any changes to already updated nodes\. You can re\-apply the update to those nodes that still have an **available** status at your convenience, as long as the update doesn't have an **Expired** status\.
+ **up to date**: The update has been applied and your cluster is up to date\. 

## Applying the Service Updates Using the Service Updates List<a name="applying-updates-elasticache-update-console-memcached"></a>

To review the list of individual service updates and their status, along with other relevant information, choose the **Service Updates List** tab\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/mem-update-available-list.png)

When viewing the **Service Updates List**, note the following:
+ **Service Update Name** A unique identifier for the service update\.
+ **Status**: The status of the update, which will be one of the following:
  + **available:** The update is available for requisite Memcached clusters\.
  + **complete:** The update has been applied and all Memcached clusters are up to date\. 
  + **cancelled:** The update has been cancelled and is no longer necessary\.
  + **expired:** The update is no longer available to apply\.
+ **Severity**: The priority of applying the update:
  + **critical:** Recommended to apply immediately \(within 14 days or less\)\.
  + **important:** Recommended to apply as soon as your business flow allows \(within 30 days or less\)\.
  + **medium:** Recommended to apply as soon as possible as your business flow allows \(within 60 days or less\)\.
  + **low:** Recommended to apply as soon as possible as your business flow allows \(within 90 days or less\)\.
+ ** Update Type**: For this version, only security updates are supported\.
+ ** Release Date**: When the update is released and available to apply on your Redis fleet\.
+ ** Recommended Apply By Date**: ElastiCache guidance date to apply the updates by\.

Choosing an individual update provides additional details, including the following:
+ **Update Description:** Provides details on the service update\.
+ **Update Expiration Date:** The date when the service update expires and no longer is available\. Any updates that aren't applied before their expiration date are cumulatively rolled into the next update\.

To review the list of individual service updates in relation to the applicable Memcached clusters, choose the **Service Update Status** tab\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/mem-update-available-list-applied.png)

When viewing the **Service Updates Status** list, note the following:
+ **Service Update Name**: Provides detailed information about the service update\.
+ **Cluster Name**: The list of your Memcached clusters that are eligible for the update\.
+ **Nodes Updated:** The ratio of individual nodes within a specific cluster that were updated or remain available for the specific service update\.
+ **Update Severity**: The priority of applying the update:
  + **critical:** Recommended to apply immediately \(within 14 days or less\)\.
  + **important:** Recommended to apply as soon as your business flow allows \(within 30 days or less\)\.
  + **medium:** Recommended to apply as soon as possible as your business flow allows \(within 60 days or less\)\.
  + **low:** Recommended to apply as soon as possible as your business flow allows \(within 90 days or less\)\.
+ ** Update Type**: For this version, only security updates are supported\.
+ **Service Update Status**: The status of the update, which will be one of the following:
  + **available:** The update is available for requisite Memcached clusters\.
  + **complete:** The update has been applied and all Memcached clusters are updated\.
  + **canceled:** The update has been canceled and is no longer necessary\.
  + **expired:** The update is no longer available to apply\.
+ **Service Update SLA Met**: This reflects whether your cluster is compliant\.
  + **yes**: All available updates have been applied to this cluster and available nodes\. 
  + **no**: The service update might have been applied successfully to one or more nodes, but other nodes within the cluster still have an **available** status\. This typically happens when a service update is applied and then stopped\. 
**Note**  
If you stop the progress of a service update on a cluster, any nodes that are already updated have a **complete** status\. Any nodes that have an **In Progress** or **Stopping** status revert to a **Stopped** status, and the **Service Update SLA Met** status changes to **no**\. 
+ **Cluster Status Modified Date**: The latest date that the cluster was modified with a service update\.

**Note**  
The **Show Previous Updates** check box, if selected, displays a list of previous updates that are no longer available\.