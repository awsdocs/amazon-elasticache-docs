# Defining a scaling policy<a name="AutoScaling-Scaling-Defining-Policy-API"></a>

A target\-tracking scaling policy configuration is represented by a JSON block that the metrics and target values are defined in\. You can save a scaling policy configuration as a JSON block in a text file\. You use that text file when invoking the AWS CLI or the Application Auto Scaling API\. For more information about policy configuration syntax, see [TargetTrackingScalingPolicyConfiguration](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html) in the Application Auto Scaling API Reference\. 

The following options are available for defining a target\-tracking scaling policy configuration: 

**Topics**
+ [Using a predefined metric](#AutoScaling-Scaling-Predefined-Metric)
+ [Using a custom metric](#AutoScaling-Scaling-Custom-Metric)
+ [Using cooldown periods](#AutoScaling-Scaling-Cooldown-periods)
+ [Disabling scale\-in activity](#AutoScaling-Scaling-Disabling-Scale-in)
+ [Applying a scaling policy](#AutoScaling-Scaling-Applying-a-Scaling-Policy)

## Using a predefined metric<a name="AutoScaling-Scaling-Predefined-Metric"></a>

By using predefined metrics, you can quickly define a target\-tracking scaling policy for an ElastiCache for Redis cluster that works with target tracking in ElastiCache for Redis Auto Scaling\. 

Currently, ElastiCache for Redis supports the following predefined metrics in ElastiCache for Redis NodeGroup Auto Scaling: 
+ **ElastiCachePrimaryEngineCPUUtilization** – The average value of the `EngineCPUUtilization` metric in CloudWatch across all primary nodes in the ElastiCache for Redis cluster\.
+ **ElastiCacheDatabaseMemoryUsageCountedForEvictPercentage** – The average value of the `DatabaseMemory` metric in CloudWatch across all primary nodes in the ElastiCache for Redis cluster\.

For more information about the `EngineCPUUtilization` and `DatabaseMemory` metrics, see [Monitoring use with CloudWatch Metrics](CacheMetrics.md)\. To use a predefined metric in your scaling policy, you create a target tracking configuration for your scaling policy\. This configuration must include a `PredefinedMetricSpecification` for the predefined metric and a `TargetValue` for the target value of that metric\. 

**Example**  
The following example describes a typical policy configuration for target\-tracking scaling for an ElastiCache for Redis cluster\. In this configuration, the `ElastiCachePrimaryEngineCPUUtilization` predefined metric is used to adjust the ElastiCache for Redis cluster based on an average CPU utilization of 40 percent across all primary nodes in the cluster\.   

```
{
    "TargetValue": 40.0,
    "PredefinedMetricSpecification":
    {
        "PredefinedMetricType": "ElastiCachePrimaryEngineCPUUtilization"
    }
}
```

## Using a custom metric<a name="AutoScaling-Scaling-Custom-Metric"></a>

 By using custom metrics, you can define a target\-tracking scaling policy that meets your custom requirements\. You can define a custom metric based on any Elasticache metric that changes in proportion to scaling\. Not all ElastiCache metrics work for target tracking\. The metric must be a valid utilization metric and describe how busy an instance is\. The value of the metric must increase or decrease in proportion to the number of Shards in the cluster\. This proportional increase or decrease is necessary to use the metric data to proportionally scale out or in the number of shards\. 

**Example**  
The following example describes a target\-tracking configuration for a scaling policy\. In this configuration, a custom metric adjusts an ElastiCache for Redis cluster based on an average CPU utilization of 50 percent across all shards in an cluster named `my-db-cluster`\. 

```
{
    "TargetValue": 50,
    "CustomizedMetricSpecification":
    {
        "MetricName": "EngineCPUUtilization",
        "Namespace": "AWS/ElastiCache",
        "Dimensions": [
            {
                "Name": "RelicationGroup","Value": "my-db-cluster"
            },
            {
                "Name": "Role","Value": "PRIMARY"
            }
        ],
        "Statistic": "Average",
        "Unit": "Percent"
    }
}
```

## Using cooldown periods<a name="AutoScaling-Scaling-Cooldown-periods"></a>

You can specify a value, in seconds, for `ScaleOutCooldown` to add a cooldown period for scaling out your cluster\. Similarly, you can add a value, in seconds, for `ScaleInCooldown` to add a cooldown period for scaling in your cluster\. For more information, see [TargetTrackingScalingPolicyConfiguration](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html) in the Application Auto Scaling API Reference\. 

 The following example describes a target\-tracking configuration for a scaling policy\. In this configuration, the `ElastiCachePrimaryEngineCPUUtilization` predefined metric is used to adjust an ElastiCache for Redis cluster based on an average CPU utilization of 40 percent across all primary nodes in that cluster\. The configuration provides a scale\-in cooldown period of 10 minutes and a scale\-out cooldown period of 5 minutes\. 

```
{
    "TargetValue": 40.0,
    "PredefinedMetricSpecification":
    {
        "PredefinedMetricType": "ElastiCachePrimaryEngineCPUUtilization"
    },
    "ScaleInCooldown": 600,
    "ScaleOutCooldown": 300
}
```

## Disabling scale\-in activity<a name="AutoScaling-Scaling-Disabling-Scale-in"></a>

You can prevent the target\-tracking scaling policy configuration from scaling in your ElastiCache for Redis cluster by disabling scale\-in activity\. Disabling scale\-in activity prevents the scaling policy from deleting shards, while still allowing the scaling policy to create them as needed\. 

You can specify a Boolean value for `DisableScaleIn` to enable or disable scale in activity for your cluster\. For more information, see [TargetTrackingScalingPolicyConfiguration](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html) in the Application Auto Scaling API Reference\. 

The following example describes a target\-tracking configuration for a scaling policy\. In this configuration, the `ElastiCachePrimaryEngineCPUUtilization` predefined metric adjusts an ElastiCache for Redis cluster based on an average CPU utilization of 40 percent across all primary nodes in that cluster\. The configuration disables scale\-in activity for the scaling policy\. 

```
{
    "TargetValue": 40.0,
    "PredefinedMetricSpecification":
    {
        "PredefinedMetricType": "ElastiCachePrimaryEngineCPUUtilization"
    },
    "DisableScaleIn": true
}
```

## Applying a scaling policy<a name="AutoScaling-Scaling-Applying-a-Scaling-Policy"></a>

After registering your cluster with ElastiCache for Redis auto scaling and defining a scaling policy, you apply the scaling policy to the registered cluster\. To apply a scaling policy to an ElastiCache for Redis cluster, you can use the AWS CLI or the Application Auto Scaling API\. 

### Applying a scaling policy using the AWS CLI<a name="AutoScaling-Scaling-Applying-a-Scaling-Policy-CLI"></a>

To apply a scaling policy to your ElastiCache for Redis cluster, use the [put\-scaling\-policy](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html) command with the following parameters: 
+ **\-\-policy\-name** – The name of the scaling policy\. 
+ **\-\-policy\-type** – Set this value to `TargetTrackingScaling`\. 
+ **\-\-resource\-id** – The resource identifier for the ElastiCache for Redis\. For this parameter, the resource type is `ReplicationGroup` and the unique identifier is the name of the ElastiCache for Redis cluster, for example `replication-group/myscalablecluster`\. 
+ **\-\-service\-namespace** – Set this value to `elasticache`\. 
+ **\-\-scalable\-dimension** – Set this value to `elasticache:replication-group:NodeGroups`\. 
+ **\-\-target\-tracking\-scaling\-policy\-configuration** – The target\-tracking scaling policy configuration to use for the ElastiCache for Redis cluster\. 

In the following example, you apply a target\-tracking scaling policy named `myscalablepolicy` to an ElastiCache for Redis cluster named `myscalablecluster` with ElastiCache for Redis auto scaling\. To do so, you use a policy configuration saved in a file named `config.json`\. 

For Linux, macOS, or Unix:

```
aws application-autoscaling put-scaling-policy \
    --policy-name myscalablepolicy \
    --policy-type TargetTrackingScaling \
    --resource-id replication-group/myscalablecluster \
    --service-namespace elasticache \
    --scalable-dimension elasticache:replication-group:NodeGroups \
    --target-tracking-scaling-policy-configuration file://config.json
```

For Windows:

```
aws application-autoscaling put-scaling-policy ^
    --policy-name myscalablepolicy ^
    --policy-type TargetTrackingScaling ^
    --resource-id replication-group/myscalablecluster ^
    --service-namespace elasticache ^
    --scalable-dimension elasticache:replication-group:NodeGroups ^
    --target-tracking-scaling-policy-configuration file://config.json
```

### Applying a scaling policy using the API<a name="AutoScaling-Scaling-Applying-a-Scaling-Policy-API"></a>

To apply a scaling policy to your ElastiCache for Redis cluster, use the [PutScalingPolicy](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html) AWS CLI command with the following parameters: 
+ **\-\-policy\-name** – The name of the scaling policy\. 
+ **\-\-resource\-id** – The resource identifier for the ElastiCache for Redis\. For this parameter, the resource type is `ReplicationGroup` and the unique identifier is the name of the ElastiCache for Redis cluster, for example `replication-group/myscalablecluster`\. 
+ **\-\-service\-namespace** – Set this value to `elasticache`\. 
+ **\-\-scalable\-dimension** – Set this value to `elasticache:replication-group:NodeGroups`\. 
+ **\-\-target\-tracking\-scaling\-policy\-configuration** – The target\-tracking scaling policy configuration to use for the ElastiCache for Redis cluster\. 

In the following example, you apply a target\-tracking scaling policy named `myscalablepolicy` to an ElastiCache for Redis cluster named `myscalablecluster` with ElastiCache for Redis auto scaling\. You use a policy configuration based on the `ElastiCachePrimaryEngineCPUUtilization` predefined metric\. 

```
POST / HTTP/1.1
Host: autoscaling.us-east-2.amazonaws.com
Accept-Encoding: identity
Content-Length: 219
X-Amz-Target: AnyScaleFrontendService.PutScalingPolicy
X-Amz-Date: 20160506T182145Z
User-Agent: aws-cli/1.10.23 Python/2.7.11 Darwin/15.4.0 botocore/1.4.8
Content-Type: application/x-amz-json-1.1
Authorization: AUTHPARAMS
{
    "PolicyName": "myscalablepolicy",
    "ServiceNamespace": "elasticache",
    "ResourceId": "replication-group/myscalablecluster",
    "ScalableDimension": "elasticache:replication-group:NodeGroups",
    "PolicyType": "TargetTrackingScaling",
    "TargetTrackingScalingPolicyConfiguration": {
        "TargetValue": 40.0,
        "PredefinedMetricSpecification":
        {
            "PredefinedMetricType": "ElastiCachePrimaryEngineCPUUtilization"
        }
    }
}
```