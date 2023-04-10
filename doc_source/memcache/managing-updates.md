# Managing the service updates<a name="managing-updates"></a>

ElastiCache for Memcached service updates are released on a regular basis\. If you have one or more qualifying clusters for those service updates, you receive notifications through email, SNS, the Personal Health Dashboard \(PHD\), and Amazon CloudWatch events when the updates are released\. The updates are also displayed on the **Service Updates** page on the ElastiCache for Memcached console\. By using this dashboard, you can view all the service updates and their status for your ElastiCache for Memcached fleet\. 

You control when to apply an update before an auto\-update starts\. We strongly recommend that you apply any updates of type **security\-update** as soon as possible to ensure that your ElastiCache for Memcached are always up\-to\-date with current security patches\. 

The following sections explore these options in detail\.

**Topics**
+ [Applying the service updates](#applying-updates-mc)

## Applying the service updates<a name="applying-updates-mc"></a>

You can start applying the service updates to your fleet from the time that the updates have an **available** status\. Service updates are cumulative\. In other words, any updates that you haven't applied yet are included with your latest update\.

If a service update has auto\-update enabled, you can choose to not take any action when it becomes available\. ElastiCache for Memcached will schedule to apply the update during your clusters' maintenance window after the **Auto\-update start date**\. You will receive related notifications for each stage of the update\.

**Note**  
You can apply only those service updates that have an **available** or **scheduled** status\.

For more information about reviewing and applying any service\-specific updates to applicable ElastiCache for Memcached clusters, see [Applying the service updates using the console](#applying-updates-console-APIReferenceconsole)\.

When a new service update is available for one or more of your ElastiCache for Memcached clusters, you can use the ElastiCache for Memcached console, API, or AWS CLI to apply the update\. The following sections explain the options that you can use to apply updates\.

### Applying the service updates using the console<a name="applying-updates-console-APIReferenceconsole"></a>

To view the list of available service updates, along with other information, go to the **Service Updates** page in the console\.

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. On the navigation pane, choose **Service Updates**\.

1. Under **Service updates** you can view the following:
   + **Service update name**: The unique name of the service update
   + **Update type**: The type of the service update, which is one of **security\-update** or **engine\-update**
   + **Update severity**: The priority of applying the update:
     + **critical:** We recommend that you apply this update immediately \(within 14 days or less\)\.
     + **important:** We recommend that you apply this update as soon as your business flow allows \(within 30 days or less\)\.
     + **medium:** We recommend that you apply this update as soon as you can \(within 60 days or less\)\.
     + **low:** We recommend that you apply this update as soon as you can \(within 90 days or less\)\.
   + **Engine version**: If the update type is engine\-update, the engine version that is being updated\.
   + ** Release Date**: When the update is released and available to apply on your Memcached fleet\.
   + ** Recommended Apply By Date**: ElastiCache guidance date to apply the updates by\.
   + **Status**: The status of the update, which is one of the following:
     + **available:** The update is available for requisite Memcached clusters\.
     + **complete:** The update has been applied\.
     + **cancelled:** The update has been canceled and is no longer necessary\.
     + **expired:** The update is no longer available to apply\.

1. Choose an individual update \(not the button to its left\) to view details of the service update\.

   In the **Cluster update status** section, you can view a list of clusters where the service update has not been applied or has just been applied recently\. For each cluster, you can view the following:
   + **Cluster name**: The name of the cluster
   + **Nodes updated:** The ratio of individual nodes within a specific cluster that were updated or remain available for the specific service update\.
   + **Update Type**: The type of the service update, which is one of **security\-update** or **engine\-update**
   + **Status**: The status of the service update on the cluster, which is one of the following:
     + *available*: The update is available for the requisite cluster\.
     + *in\-progres*: The update is being applied to this cluster\.
     + *scheduled*: The update date has been scheduled\.
     + *complete*: The update has been successfully applied\. Cluster with a complete status will be displayed for 7 days after its completion\.

     If you chose any or all of the clusters with the **available** or **scheduled** status, and then chose **Apply now**, the update will start being applied on those clusters\.

### Applying the service updates using the AWS CLI<a name="applying-updates-cli-mc"></a>

After you receive notification that service updates are available, you can inspect and apply them using the AWS CLI:
+ To retrieve a description of the service updates that are available, run the following command:

  `aws elasticache describe-service-updates --status available`

  For more information, see [describe\-service\-updates](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-service-updates.html)\. 
+ To apply a service update on a list of clusters, run the following command:

  `aws elasticache batch-apply-update-action --service-update ServiceUpdateNameToApply=sample-service-update --cluster-names cluster-1 cluster2`

  For more information, see [batch\-apply\-update\-action](https://docs.aws.amazon.com/cli/latest/reference/elasticache/batch-apply-update-action.html)\. 