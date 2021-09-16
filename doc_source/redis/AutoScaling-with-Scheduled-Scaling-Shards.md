# Scheduled scaling<a name="AutoScaling-with-Scheduled-Scaling-Shards"></a>

Scaling based on a schedule enables you to scale your application in response to predictable changes in demand\. To use scheduled scaling, you create scheduled actions, which tell ElastiCache for Redis to perform scaling activities at specific times\. When you create a scheduled action, you specify an existing ElastiCache for Redis cluster, when the scaling activity should occur, minimum capacity, and maximum capacity\. You can create scheduled actions that scale one time only or that scale on a recurring schedule\. 

 You can only create a scheduled action for ElastiCache for Redis clusters that already exist\. You can't create a scheduled action at the same time that you create a cluster\.

For more information on terminology for scheduled action creation, management, and deletion, see [ Commonly used commands for scheduled action creation, management, and deletion ](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-scheduled-scaling.html#scheduled-scaling-commonly-used-commands) 

**To create on a recurring schedule:**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Redis**\. 

1. Choose the cluster that you want to add a policy for\. 

1. Choose the **Manage Auto Scaling policie** from the **Actions** dropdown\. 

1. Choose the **Auto Scaling policies** tab\.

1. In the **Auto scaling policies** section, the **Add Scaling policy** dialog box appears\. Choose **Scheduled scaling**\.

1. For **Policy Name**, enter the policy name\. 

1. For **Scalable Dimension**, choose **Shards**\. 

1. For **Target Shards**, choose the value\. 

1. For **Recurrence**, choose **Recurring**\. 

1. For **Frequency**, choose the respective value\. 

1. For **Start Date** and **Start time**, choose the time from when the policy will go into effect\. 

1. Choose **Add Policy**\. 

**To create a one\-time scheduled action:**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Redis**\. 

1. Choose the cluster that you want to add a policy for\. 

1. Choose the **Manage Auto Scaling policie** from the **Actions** dropdown\. 

1. Choose the **Auto Scaling policies** tab\.

1. In the **Auto scaling policies** section, the **Add Scaling policy** dialog box appears\. Choose **Scheduled scaling**\.

1. For **Policy Name**, enter the policy name\. 

1. For **Scalable Dimension**, choose **Shards**\. 

1. For **Target Shards**, choose the value\. 

1. For **Recurrence**, choose **One Time**\. 

1. For **Start Date** and **Start time**, choose the time from when the policy will go into effect\. 

1. For **End Date** choose the date until when the policy would be in effect\. 

1. Choose **Add Policy**\. 

**To delete a scheduled action**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Redis**\. 

1. Choose the cluster that you want to add a policy for\. 

1. Choose the **Manage Auto Scaling policie** from the **Actions** dropdown\. 

1. Choose the **Auto Scaling policies** tab\.

1. In the **Auto scaling policies** section, choose the auto scaling policy, and then choose **Delete** from the **Actions** dialog\.

**To manage scheduled scaling using the AWS CLI **

Use the following application\-autoscaling APIs:
+ [put\-scheduled\-action](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/put-scheduled-action.html) 
+ [describe\-scheduled\-actions](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/describe-scheduled-actions.html) 
+ [delete\-scheduled\-action](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/delete-scheduled-action.html) 

## Use AWS CloudFormation to create a scheduled action<a name="AutoScaling-with-Cloudformation-Declare-Scheduled-Action"></a>

This snippet shows how to create a target tracking policy and apply it to an [AWS::ElastiCache::ReplicationGroup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-elasticache-replicationgroup.html) resource using the [AWS::ApplicationAutoScaling::ScalableTarget](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-applicationautoscaling-scalabletarget.html) resource\. It uses the [Fn::Join](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-join.html) and [Ref](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-ref.html) intrinsic functions to construct the `ResourceId` property with the logical name of the `AWS::ElastiCache::ReplicationGroup` resource that is specified in the same template\. 

```
ScalingTarget:
   Type: 'AWS::ApplicationAutoScaling::ScalableTarget'
   Properties:
     MaxCapacity: 3
     MinCapacity: 1
     ResourceId: !Sub replication-group/${logicalName}
     ScalableDimension: 'elasticache:replication-group:NodeGroups'
     ServiceNamespace: elasticache
     RoleARN: !Sub "arn:aws:iam::${AWS::AccountId}:role/aws-service-role/elasticache.application-autoscaling.amazonaws.com/AWSServiceRoleForApplicationAutoScaling_ElastiCacheRG"
     ScheduledActions:
       - EndTime: '2020-12-31T12:00:00.000Z'
         ScalableTargetAction:
           MaxCapacity: '5'
           MinCapacity: '2'
         ScheduledActionName: First
         Schedule: 'cron(0 18 * * ? *)'
```