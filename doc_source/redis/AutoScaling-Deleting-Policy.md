# Deleting a scaling policy<a name="AutoScaling-Deleting-Policy"></a>

You can delete a scaling policy using the AWS Management Console, the AWS CLI or the Application Auto Scaling API

**Deleting a scaling policy using the AWS Management Console**

You can only edit policies with type Predefined metrics by using the AWS Management Console

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Redis**

1. Choose the cluster whose auto scaling policy you want to delete\.

1. Choose the **Auto Scaling policies** tab\. 

1. In the **Auto scaling policies** section, choose the auto scaling policy, and then choose **Delete** from the **Actions** dialog box\. 

**Deleting a scaling policy using the AWS CLI or the Application Auto Scaling API **

You can use the AWS CLI or the Application Auto Scaling API to delete a scaling policy from an Elasticache cluster\. 

**CLI**

To delete a scaling policy from your ElastiCache for Redis cluster, use the [delete\-scaling\-policy](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scaling-policy.html) command with the following parameters: 
+ \-\-policy\-name – The name of the scaling policy\. 
+ \-\-resource\-id – The resource identifier for the ElastiCache for Redis cluster\. For this parameter, the resource type is ReplicationGroup and the unique identifier is the name of the Elasticache cluster, for example `replication-group/myscalablecluster`\. 
+ \-\-service\-namespace – Set this value to elasticache\. 
+ \-\-scalable\-dimension – Set this value to `elasticache:replication-group:Replicas`\. 

**Example**  
In the following example, you delete a target\-tracking scaling policy named `myscalablepolicy` from an ELC; cluster named `myscalablecluster`\. 

For Linux, macOS, or Unix:

```
aws application-autoscaling delete-scaling-policy \
    --policy-name myscalablepolicy \
    --resource-id replication-group/myscalablecluster \
    --service-namespace elasticache \
    --scalable-dimension elasticache:replication-group:Replicas \
```

For Windows:

```
aws application-autoscaling delete-scaling-policy ^
    --policy-name myscalablepolicy ^
    --resource-id replication-group/myscalablecluster ^
    --service-namespace elasticache ^
    --scalable-dimension elasticache:replication-group:Replicas ^
```

**API**

To delete a scaling policy from your ElastiCache for Redis cluster, use the [DeleteScalingPolicy](https://docs.aws.amazon.com/ApplicationAutoScaling/latest/APIReference/API_DeleteScalingPolicy.html) Application Auto Scaling API operation with the following parameters: 
+ PolicyName – The name of the scaling policy\. 
+ ResourceID – The resource identifier for the ElastiCache for Redis cluster\. For this parameter, the resource type is ReplicationGroup and the unique identifier is the name of the Elasticache cluster, for example `replication-group/myscalablecluster`\. 
+ ServiceNamespace – Set this value to elasticache\. 
+ ScalableDimension – Set this value to `elasticache:replication-group:Replicas`\. 

In the following example, you delete a target\-tracking scaling policy named `myscalablepolicy` from an ElastiCache for Redis cluster named `myscalablecluster` with the Application Auto Scaling API\. 

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
    "ScalableDimension": "elasticache:replication-group:Replicas"
}
```