# Auto Scaling ElastiCache for Redis clusters<a name="AutoScaling"></a>

## Prerequisites<a name="AutoScaling-Prerequisites"></a>

ElastiCache for Redis Auto Scaling is limited to the following:
+ Redis \(cluster mode enabled\) clusters running Redis engine version 6\.0 onwards
+ Instance type families \- R5, R6g, M5, M6g
+ Instance sizes \- Large, XLarge, 2XLarge
+ Auto Scaling in ElastiCache for Redis is not supported for clusters running in Global datastores, Outposts or Local Zones\.
+ AWS Auto Scaling for ElastiCache for Redis is not available in the following regions: China \(Beijing\), China \(Ningxia\), AWS GovCloud \(US\-West\) and AWS GovCloud \(US\-East\)\.

## Managing Capacity Automatically with ElastiCache for Redis Auto Scaling<a name="AutoScaling-Managing"></a>

ElastiCache for Redis auto scaling is the ability to increase or decrease the desired shards or replicas in your ElastiCache for Redis service automatically\. ElastiCache for Redis leverages the Application Auto Scaling service to provide this functionality\. For more information, see [Application Auto Scaling](https://docs.aws.amazon.com/autoscaling/application/userguide/what-is-application-auto-scaling.html)\. To use automatic scaling, you define and apply a scaling policy that uses CloudWatch metrics and target values that you assign\. ElastiCache for Redis auto scaling uses the policy to increase or decrease the number of instances in response to actual workloads\. 

You can use the AWS Management Console to apply a scaling policy based on a predefined metric\. A `predefined metric` is defined in an enumeration so that you can specify it by name in code or use it in the AWS Management Console\. Custom metrics are not available for selection using the AWS Management Console\. Alternatively, you can use either the AWS CLI or the Application Auto Scaling API to apply a scaling policy based on a predefined or custom metric\. 

ElastiCache for Redis supports scaling for the following dimensions:
+ **Shards** – Automatically add/remove shards in the cluster similar to manual online resharding\. In this case, ElastiCache for Redis auto scaling triggers scaling on your behalf\.
+ **Replicas** – Automatically add/remove replicas in the cluster similar to manual Increase/Decrease replica operations\. ElastiCache for Redis auto scaling adds/removes replicas uniformly across all shards in the cluster\.

ElastiCache for Redis supports the following types of automatic scaling policies:
+ [Target tracking scaling policies](AutoScaling-Scaling-Policies-Target.md) – Increase or decrease the number of shards/replicas that your service runs based on a target value for a specific metric\. This is similar to the way that your thermostat maintains the temperature of your home\. You select a temperature and the thermostat does the rest\.
+ [Scheduled scaling for Application ElastiCache for Redis auto scaling ](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-scheduled-scaling.html) – Increase or decrease the number of shards/replicas that your service runs based on the date and time\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/Auto-scaling.png)

The following steps summarize the ElastiCache for Redis auto scaling process as shown in the previous diagram: 

1. You create an ElastiCache for Redis auto scaling policy for your ElastiCache for Redis Replication Group\.

1. ElastiCache for Redis auto scaling creates a pair of CloudWatch alarms on your behalf\. Each pair represents your upper and lower boundaries for metrics\. These CloudWatch alarms are triggered when the cluster's actual utilization deviates from your target utilization for a sustained period of time\. You can view the alarms in the console\.

1. If the configured metric value exceeds your target utilization \(or falls below the target\) for a specific length of time, CloudWatch triggers an alarm that invokes ElastiCache for Redis auto scaling to evaluate your scaling policy\.

1. ElastiCache for Redis auto scaling issues a Modify request to adjust your cluster capacity\. 

1. ElastiCache for Redis processes the Modify request, dynamically increasing \(or decreasing\) the cluster Shards/Replicas capacity so that it approaches your target utilization\. 

 To understand how ElastiCache for Redis Auto Scaling works, suppose that you have a cluster named `UsersCluster`\. By monitoring the CloudWatch metrics for `UsersCluster`, you determine the Max shards that the cluster requires when traffic is at its peak and Min Shards when traffic is at its lowest point\. You also decide a target value for CPU utilization for the `UsersCluster` cluster\. ElastiCache for Redis auto scaling uses its target tracking algorithm to ensure that the provisioned shards of `UsersCluster` is adjusted as required so that utilization remains at or near to the target value\. 

**Note**  
Scaling may take noticeable time and will require extra cluster resources for shards to rebalance\. ElastiCache for Redis Auto Scaling modifies resource settings only when the actual workload stays elevated \(or depressed\) for a sustained period of several minutes\. The ElastiCache for Redis auto scaling target\-tracking algorithm seeks to keep the target utilization at or near your chosen value over the long term\. 

## IAM Permissions Required for ElastiCache for Redis Auto Scaling<a name="AutoScaling-IAM-permissions"></a>

ElastiCache for Redis Auto Scaling is made possible by a combination of the ElastiCache for Redis, CloudWatch, and Application Auto Scaling APIs\. Clusters are created and updated with ElastiCache for Redis, alarms are created with CloudWatch, and scaling policies are created with Application Auto Scaling\. In addition to the standard IAM permissions for creating and updating clusters, the IAM user that accesses ElastiCache for Redis Auto Scaling settings must have the appropriate permissions for the services that support dynamic scaling\. IAM users must have permissions to use the actions shown in the following example policy: 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "application-autoscaling:*",
                "elasticache:DescribeReplicationGroups",
                "elasticache:ModifyReplicationGroupShardConfiguration",
                "elasticache:IncreaseReplicaCount",
                "elasticache:DecreaseReplicaCount",
                "elasticache:DescribeCacheClusters",
                "elasticache:DescribeCacheParameters",
                "cloudwatch:DeleteAlarms",
                "cloudwatch:DescribeAlarmHistory",
                "cloudwatch:DescribeAlarms",
                "cloudwatch:DescribeAlarmsForMetric",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:ListMetrics",
                "cloudwatch:PutMetricAlarm",
                "cloudwatch:DisableAlarmActions",
                "cloudwatch:EnableAlarmActions",
                "iam:CreateServiceLinkedRole",
                "sns:CreateTopic",
                "sns:Subscribe",
                "sns:Get*",
                "sns:List*"
            ],
            "Resource": "arn:aws:iam::123456789012:role/autoscaling-roles-for-cluster"
        }
    ]
}
```

## Service\-linked role<a name="AutoScaling-SLR"></a>

The ElastiCache for Redis auto scaling service also needs permission to describe your clusters and CloudWatch alarms, and permissions to modify your ElastiCache for Redis target capacity on your behalf\. If you enable Auto Scaling for your ElastiCache for Redis cluster, it creates a service\-linked role named `AWSServiceRoleForApplicationAutoScaling_ElastiCacheRG`\. This service\-linked role grants ElastiCache for Redis auto scaling permission to describe the alarms for your policies, to monitor the current capacity of the fleet, and to modify the capacity of the fleet\. The service\-linked role is the default role for ElastiCache for Redis auto scaling\. For more information, see [Service\-linked roles for ElastiCache for Redis auto scaling ](https://docs.aws.amazon.com/autoscaling/application/userguide/application-auto-scaling-service-linked-roles.html) in the Application Auto Scaling User Guide\.

## Auto Scaling Best Practices<a name="AutoScaling-best-practices"></a>

Before registering to Auto Scaling, we recommend the following:

1. **Use just one tracking metric** – Identify if your cluster has CPU\- or memory\-intensive workloads and use a corresponding predefined metric to define Scaling Policy\. We recommend you avoid multiple policies per dimension on the cluster\. ElastiCache for Redis Auto scaling will scale out the scalable target if any of the target tracking policies are ready for scale out, but will scale in only if all of the target tracking policies \(with the scale\-in portion enabled\) are ready to scale in\. If multiple policies instruct the scalable target to scale out or in at the same time, it scales based on the policy that provides the largest capacity for both scale in and scale out\.

1. **Use just one dimension** – Identify if your cluster has a write\- or read\-heavy workload and use corresponding dimension\(shards/replicas\) to define scaling policy\. Having policies on multiple dimensions for the same cluster can have repercussions on scaling actions\. For example, if you create scaling policies on engine CPU for both shards and replicas, and if a scale\-out action is triggered on a shard dimension which adds new shards along with their replicas, this increase in new replicas can impact scaling policy of replica dimension that can trigger scale\-in of replicas and vise versa\. `Avg` metric is used across cluster nodes for the Predefined Metrics\. 

1. **Customized Metrics for Target Tracking** – Be cautious when using customized metrics for Target Tracking as Auto scaling is best suited to scale\-out/in proportional to change in metrics choosen for the policy\. If such metrics that don't change proprtionally to the scaling actions are used for policy creation, it might lead to continous scale\-out or scale\-in actions which might affect availability or cost\. 

1. **Scheduled Scaling** – If you identify that your workload is deterministic \(reaches high/low at a specific time\), we recommend using Scheduled Scaling and configure your target capacity according to the need\. Target Tracking is best suitable for non\-deterministic workloads and for the cluster to operate at the required target metric by scaling out when you need more resources and scaling in when you need less\. 

1. **Disable Scale\-In** – Auto scaling on Target Tracking is best suited for clusters with gradual increase/decrease of workloads as spikes/dip in metrics can trigger consecutive scale\-out/in oscillations\. In order to avoid such oscillations, you can start with scale\-in disabled and later you can always manually scale\-in to your need\. 

1. **Test your application** – We recommend you test your application with your estimated Min/Max workloads to determine absolute Min,Max shards/replicas required for the cluster while creating Scaling policies to avoid availability issues\. Auto scaling can scale out to the Max and scale\-in to the Min threshold configured for the target\.

1. **Defining Target Value** – You can analyze corresponding CloudWatch metrics for cluster utilization over a four\-week period to determine the target value threshold\. If you are still not sure of of what value to choose, we recommend starting with a minimum supported predefined metric value\.

1. AutoScaling on Target Tracking is best suited for clusters with uniform distribution of workloads across shards/replicas dimension\. Having non\-uniform distribution can lead to:
   + Scaling when not required due to workload spike/dip on a few hot shards/replicas\.
   + Not scaling when required due to overall avg close to target even though having hot shards/replicas\.

**Note**  
When scaling out your cluster, ElastiCache will automatically replicate the Functions loaded in one of the existing nodes \(selected at random\) to the new node\(s\)\. If your cluster has Redis 7\.0 or above and your application uses [Redis Functions](https://redis.io/docs/manual/programmability/functions-intro/), we recommend loading all of your functions to all the shards before scaling out so that your cluster does not end up with different functions on different shards\.

After registering to AutoScaling, note the following:
+ There are limitations on Auto scaling Supported Configurations, so we recommend you not change configuration of a replication group that is registered for Auto scaling\. The following are examples:
  + Manually modifying instance type to unsupported types\.
  + Associating the replication group to a Global datastore\.
  + Changing `ReservedMemoryPercent` parameter\.
  + Manually increasing/decreasing shards/replicas beyond the Min/Max capacity configured during policy creation\.