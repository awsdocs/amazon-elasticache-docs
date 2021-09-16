# Auto Scaling policies<a name="AutoScaling-Policies"></a>

A scaling policy has the following components:
+ A target metric – The CloudWatch metric that ElastiCache for Redis Auto Scaling uses to determine when and how much to scale\. 
+ Minimum and maximum capacity – The minimum and maximum number of shards or replicas to use for scaling\. 
**Important**  
While creating Auto scaling policy , if current capacity is higher than max capacity configured, we scaleIn to the MaxCapacity during policy creation\. Similarly if current capacity is lower than min capacity configured, we scaleOut to the MinCapacity\. 
+ A cooldown period – The amount of time, in seconds, after a scale\-in or scale\-out activity completes before another scale\-out activity can start\. 
+ A service\-linked role – An AWS Identity and Access Management \(IAM\) role that is linked to a specific AWS service\. A service\-linked role includes all of the permissions that the service requires to call other AWS services on your behalf\. ElastiCache for Redis Auto Scaling automatically generates this role, `AWSServiceRoleForApplicationAutoScaling_ElastiCacheRG`, for you\. 
+ Enable or disable scale\-in activities \- Ability to enable or disable scale\-in activities for a policy\.

**Topics**
+ [Target metric for Auto Scaling](#AutoScaling-TargetMetric)
+ [Minimum and maximum capacity](#AutoScaling-MinMax)
+ [Cool down period](#AutoScaling-Cooldown)
+ [Enable or disable scale\-in activities](#AutoScaling-enable-disable-scale-in)

## Target metric for Auto Scaling<a name="AutoScaling-TargetMetric"></a>

In this type of policy, a predefined or custom metric and a target value for the metric is specified in a target\-tracking scaling policy configuration\. ElastiCache for Redis Auto Scaling creates and manages CloudWatch alarms that trigger the scaling policy and calculates the scaling adjustment based on the metric and target value\. The scaling policy adds or removes shards/replicas as required to keep the metric at, or close to, the specified target value\. In addition to keeping the metric close to the target value, a target\-tracking scaling policy also adjusts to fluctuations in the metric due to a changing workload\. Such a policy also minimizes rapid fluctuations in the number of available shards/replicas for your cluster\. 

For example, consider a scaling policy that uses the predefined average `ElastiCachePrimaryEngineCPUUtilization` metric\. Such a policy can keep CPU utilization at, or close to, a specified percentage of utilization, such as 70 percent\. 

**Note**  
For each cluster, you can create only one Auto Scaling policy for each target metric\. 

## Minimum and maximum capacity<a name="AutoScaling-MinMax"></a>

**Shards**

You can specify the maximum number of shards that can be scaled to by ElastiCache for Redis auto scaling\. This value must be less than or equal to 250 with a minimum of 1\. You can also specify the minimum number of shards to be managed by ElastiCache for Redis auto scaling\. This value must be at least 1, and equal to or less than the value specified for the maximum shards 250\. 

**Replicas**

You can specify the maximum number of replicas to be managed by ElastiCache for Redis auto scaling\. This value must be less than or equal to 5\. You can also specify the minimum number of replicas to be managed by ElastiCache for Redis auto scaling\. This value must be at least 1, and equal to or less than the value specified for the maximum replicas 5\.

To determine the minimum and maximum number of shards/replicas that you need for typical traffic, test your Auto Scaling configuration with the expected rate of traffic to your model\. 

**Note**  
ElastiCache for Redis auto scaling policies increase cluster capacity until it reaches your defined maximum size or until service limits apply\. To request a limit increase, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) and choose the limit type **Nodes per cluster per instance type**\. 

**Important**  
Scaling\-in occurs when there is no traffic\. If a variant’s traffic becomes zero, ElastiCache for Redis automatically scales in to the minimum number of instances specified\.

## Cool down period<a name="AutoScaling-Cooldown"></a>

You can tune the responsiveness of a target\-tracking scaling policy by adding cooldown periods that affect scaling your cluster\. A cooldown period blocks subsequent scale\-in or scale\-out requests until the period expires\. This slows the deletions of shards/replicas in your ElastiCache for Redis cluster for scale\-in requests, and the creation of shards/replicas for scale\-out requests\. You can specify the following cooldown periods:
+ A scale\-in activity reduces the number of shards/replicas in your ElastiCache for Redis cluster\. A scale\-in cooldown period specifies the amount of time, in seconds, after a scale\-in activity completes before another scale\-in activity can start\.
+ A scale\-out activity increases the number of shards/replicas in your ElastiCache for Redis cluster\. A scale\-out cooldown period specifies the amount of time, in seconds, after a scale\-out activity completes before another scale\-out activity can start\. 

When a scale\-in or a scale\-out cooldown period is not specified, the default for scale\-out is 600 seconds and for scale\-in 900 seconds\. 

## Enable or disable scale\-in activities<a name="AutoScaling-enable-disable-scale-in"></a>

You can enable or disable scale\-in activities for a policy\. Enabling scale\-in activities allows the scaling policy to delete shards/replicas\. When scale\-in activities are enabled, the scale\-in cooldown period in the scaling policy applies to scale\-in activities\. Disabling scale\-in activities prevents the scaling policy from deleting shards/replicas\. 

**Note**  
Scale\-out activities are always enabled so that the scaling policy can create ElastiCache for Redis shards/replicas as needed\.