# Registering a Scalable Target<a name="AutoScaling-Register-Policy"></a>

You can apply a scaling policy based on either a predefined or custom metric\. To do so, you can use the AWS CLI or the Application Auto Scaling API\. The first step is to register your ElastiCache for Redis replication group with ElastiCache for Redis auto scaling\. 

Before you can use ElastiCache for Redis auto scaling with an ElastiCache for Redis cluster, you register your cluster with ElastiCache for Redis auto scaling\. You do so to define the scaling dimension and limits to be applied to that cluster\. ElastiCache for Redis auto scaling dynamically scales the ElastiCache for Redis cluster along the `elasticache:replication-group:Replicas` scalable dimension, which represents the number of cluster replicas per shard\. 

**Using the CLI** 

To register your Elasticache cluster, use the [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command with the following parameters: 
+ \-\-service\-namespace – Set this value to elasticache\. 
+ \-\-resource\-id – The resource identifier for the Elasticache cluster\. For this parameter, the resource type is ReplicationGroup and the unique identifier is the name of the ElastiCache for Redis cluster, for example `replication-group/myscalablecluster`\. 
+ \-\-scalable\-dimension – Set this value to `elasticache:replication-group:Replicas`\. 
+ \-\-min\-capacity – The minimum number of replicas to be managed by ElastiCache for Redis auto scaling\. For information about the relationship between \-\-min\-capacity, \-\-max\-capacity, and the number of replicas in your cluster, see [Minimum and maximum capacity](AutoScaling-Policies.md#AutoScaling-MinMax)\.
+ \-\-max\-capacity – The maximum number of replicas to be managed by ElastiCache for Redis auto scaling\. For information about the relationship between \-\-min\-capacity, \-\-max\-capacity, and the number of replicas in your cluster, see [Minimum and maximum capacity](AutoScaling-Policies.md#AutoScaling-MinMax)\.

**Example**  
In the following example, you register an ElastiCache for Redis cluster named `myscalablecluster`\. The registration indicates that the cluster should be dynamically scaled to have from one to 5 replicas\.   
For Linux, macOS, or Unix:  

```
aws application-autoscaling register-scalable-target \
    --service-namespace elasticache \
    --resource-id replication-group/myscalablecluster \
    --scalable-dimension elasticache:replication-group:Replicas \
    --min-capacity 1 \
    --max-capacity 5 \
```
For Windows:  

```
aws application-autoscaling register-scalable-target ^
    --service-namespace elasticache ^
    --resource-id replication-group/myscalablecluster ^
    --scalable-dimension elasticache:replication-group:Replicas ^
    --min-capacity 1 ^
    --max-capacity 5 ^
```

**Using the API**

To register your Elasticache cluster, use the [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command with the following parameters: 
+ ServiceNamespace – Set this value to elasticache\. 
+ ResourceID – The resource identifier for the Elasticache cluster\. For this parameter, the resource type is ReplicationGroup and the unique identifier is the name of the ElastiCache for Redis cluster, for example `replication-group/myscalablecluster`\. 
+ ScalableDimension – Set this value to `elasticache:replication-group:Replicas`\. 
+ MinCapacity – The minimum number of replicas to be managed by ElastiCache for Redis auto scaling\. For information about the relationship between \-\-min\-capacity, \-\-max\-capacity, and the number of replicas in your cluster, see [Minimum and maximum capacity](AutoScaling-Policies.md#AutoScaling-MinMax)\.
+ MaxCapacity – The maximum number of replicas to be managed by ElastiCache for Redis auto scaling\. For information about the relationship between \-\-min\-capacity, \-\-max\-capacity, and the number of replicas in your cluster, see [Minimum and maximum capacity](AutoScaling-Policies.md#AutoScaling-MinMax)\.

**Example**  
In the following example, you register an ElastiCache for Redis cluster named `myscalablecluster` with the Application Auto Scaling API\. This registration indicates that the cluster should be dynamically scaled to have from one to 5 replicas\. 

```
POST / HTTP/1.1
Host: autoscaling.us-east-2.amazonaws.com
Accept-Encoding: identity
Content-Length: 219
X-Amz-Target: AnyScaleFrontendService.RegisterScalableTarget
X-Amz-Date: 20160506T182145Z
User-Agent: aws-cli/1.10.23 Python/2.7.11 Darwin/15.4.0 botocore/1.4.8
Content-Type: application/x-amz-json-1.1
Authorization: AUTHPARAMS
{
    "ServiceNamespace": "elasticache",
    "ResourceId": "replication-group/myscalablecluster",
    "ScalableDimension": "elasticache:replication-group:Replicas",
    "MinCapacity": 1,
    "MaxCapacity": 5
}
```