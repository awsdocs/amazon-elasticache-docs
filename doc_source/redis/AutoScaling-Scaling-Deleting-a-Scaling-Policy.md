# Deleting a scaling policy<a name="AutoScaling-Scaling-Deleting-a-Scaling-Policy"></a>

You can delete a scaling policy using the AWS Management Console, the AWS CLI, or the Application Auto Scaling API\. 

## Deleting a scaling policy using the AWS Management Console<a name="AutoScaling-Scaling-Editing-a-Scaling-Policy-CON"></a>

**To delete an Auto Scaling policy for an ElastiCache for Redis cluster**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Redis**\. 

1. Choose the cluster whose Auto Scaling policy you want to edit \(choose the cluster name, not the button to its left\)\. 

1. Choose the **Auto Scaling policies** tab\. 

1. Under **Scaling policies**, choose the Auto Scaling policy, and then choose **Delete**\. 

## Deleting a scaling policy using the AWS CLI<a name="AutoScaling-Scaling-Deleting-a-Scaling-Policy-CLI"></a>

To delete a scaling policy to your ElastiCache for Redis cluster, use the [delete\-scaling\-policy](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/delete-scaling-policy.html) AWS CLI command with the following parameters: 
+ **\-\-policy\-name** – The name of the scaling policy\. 
+ **\-\-resource\-id** – The resource identifier for the ElastiCache for Redis\. For this parameter, the resource type is `ReplicationGroup` and the unique identifier is the name of the ElastiCache for Redis cluster, for example `replication-group/myscalablecluster`\. 
+ **\-\-service\-namespace** – Set this value to `elasticache`\. 
+ **\-\-scalable\-dimension** – Set this value to `elasticache:replication-group:NodeGroups`\. 

In the following example, you delete a target\-tracking scaling policy named `myscalablepolicy` from an ElastiCache for Redis cluster named `myscalablecluster`\. 

For Linux, macOS, or Unix:

```
aws application-autoscaling delete-scaling-policy \
    --policy-name myscalablepolicy \
    --resource-id replication-group/myscalablecluster \
    --service-namespace elasticache \
    --scalable-dimension elasticache:replication-group:NodeGroups
```

For Windows:

```
aws application-autoscaling delete-scaling-policy ^
    --policy-name myscalablepolicy ^
    --resource-id replication-group/myscalablecluster ^
    --service-namespace elasticache ^
    --scalable-dimension elasticache:replication-group:NodeGroups
```

## Deleting a scaling policy using the API<a name="AutoScaling-Scaling-Deleting-a-Scaling-Policy-API"></a>

To delete a scaling policy to your ElastiCache for Redis cluster, use the [DeleteScalingPolicy](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/delete-scaling-policy.html) AWS CLI command with the following parameters: 
+ **\-\-policy\-name** – The name of the scaling policy\. 
+ **\-\-resource\-id** – The resource identifier for the ElastiCache for Redis\. For this parameter, the resource type is `ReplicationGroup` and the unique identifier is the name of the ElastiCache for Redis cluster, for example `replication-group/myscalablecluster`\. 
+ **\-\-service\-namespace** – Set this value to `elasticache`\. 
+ **\-\-scalable\-dimension** – Set this value to `elasticache:replication-group:NodeGroups`\. 

In the following example, you delete a target\-tracking scaling policy named `myscalablepolicy` from an ElastiCache for Redis cluster named `myscalablecluster`\. 

```
POST / HTTP/1.1
Host: autoscaling.us-east-2.amazonaws.com
Accept-Encoding: identity
Content-Length: 219
X-Amz-Target: AnyScaleFrontendService.DeleteScalingPolicy
X-Amz-Date: 20160506T182145Z
User-Agent: aws-cli/1.10.23 Python/2.7.11 Darwin/15.4.0 botocore/1.4.8
Content-Type: application/x-amz-json-1.1
Authorization: AUTHPARAMS
{
    "PolicyName": "myscalablepolicy",
    "ServiceNamespace": "elasticache",
    "ResourceId": "replication-group/myscalablecluster",
    "ScalableDimension": "elasticache:replication-group:NodeGroups"
}
```