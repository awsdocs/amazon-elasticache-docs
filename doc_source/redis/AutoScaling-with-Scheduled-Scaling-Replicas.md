# Scheduled scaling<a name="AutoScaling-with-Scheduled-Scaling-Replicas"></a>

Scaling based on a schedule enables you to scale your application in response to predictable changes in demand\. To use scheduled scaling, you create scheduled actions, which tell ElastiCache for Redis to perform scaling activities at specific times\. When you create a scheduled action, you specify an existing ElastiCache for Redis cluster, when the scaling activity should occur, minimum capacity, and maximum capacity\. You can create scheduled actions that scale one time only or that scale on a recurring schedule\. 

 You can only create a scheduled action for ElastiCache for Redis clusters that already exist\. You can't create a scheduled action at the same time that you create a cluster\.

For more information on terminology for scheduled action creation, management, and deletion, see [ Commonly used commands for scheduled action creation, management, and deletion ](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-scheduled-scaling.html#scheduled-scaling-commonly-used-commands) 

**To create a one\-time scheduled action:**

Similar to Shard dimension\. See [Scheduled scaling ](AutoScaling-with-Scheduled-Scaling-Shards.md)\.

**To delete a scheduled action**

Similar to Shard dimension\. See [Scheduled scaling ](AutoScaling-with-Scheduled-Scaling-Shards.md)\.

**To manage scheduled scaling using the AWS CLI **

Use the following application\-autoscaling APIs:
+ [put\-scheduled\-action](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html) 
+ [describe\-scheduled\-actions](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scheduled-actions.html) 
+ [delete\-scheduled\-action](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scheduled-action.html) 

## Use AWS CloudFormation to create Auto Scaling policies<a name="AutoScaling-with-Cloudformation-Update-Action"></a>

This snippet shows how to create a scheduled action and apply it to an [AWS::ElastiCache::ReplicationGroup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-elasticache-replicationgroup.html) resource using the [AWS::ApplicationAutoScaling::ScalableTarget](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-applicationautoscaling-scalabletarget.html) resource\. It uses the [Fn::Join](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-join.html) and [Ref](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-ref.html) intrinsic functions to construct the `ResourceId` property with the logical name of the `AWS::ElastiCache::ReplicationGroup` resource that is specified in the same template\. 

```
ScalingTarget:
   Type: 'AWS::ApplicationAutoScaling::ScalableTarget'
   Properties:
     MaxCapacity: 0
     MinCapacity: 0
     ResourceId: !Sub replication-group/${logicalName}
     ScalableDimension: 'elasticache:replication-group:Replicas'
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