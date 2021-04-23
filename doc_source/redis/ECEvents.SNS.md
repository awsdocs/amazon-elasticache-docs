# Managing ElastiCache Amazon SNS notifications<a name="ECEvents.SNS"></a>

You can configure ElastiCache to send notifications for important cluster events using Amazon Simple Notification Service \(Amazon SNS\)\. In these examples, you will configure a cluster with the Amazon Resource Name \(ARN\) of an Amazon SNS topic to receive notifications\. 

**Note**  
This topic assumes that you've signed up for Amazon SNS and have set up and subscribed to an Amazon SNS topic\. For information on how to do this, see the [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/)\. 

## Adding an Amazon SNS topic<a name="ECEvents.SNS.Adding"></a>

The following sections show you how to add an Amazon SNS topic using the AWS Console, the AWS CLI, or the ElastiCache API\.

### Adding an Amazon SNS topic \(Console\)<a name="ECEvents.SNS.Adding.Console"></a>

 The following procedure shows you how to add an Amazon SNS topic for a cluster\. To add an Amazon SNS topic for a replication group, in step 2, instead of choosing a cluster, choose a replication group then follow the same remaining steps\.

**Note**  
 This process can also be used to modify the Amazon SNS topic\. 

**To add or modify an Amazon SNS topic for a cluster \(Console\)**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In ** Clusters**, choose the cluster for which you want to add or modify an Amazon SNS topic ARN\.

1. Choose **Modify**\.

1. In **Modify Cluster** under **Topic for SNS Notification**, choose the SNS topic you want to add, or choose **Manual ARN input** and type the ARN of the Amazon SNS topic\. 

1. Choose **Modify**\.

### Adding an Amazon SNS topic \(AWS CLI\)<a name="ECEvents.SNS.Adding.CLI"></a>

To add or modify an Amazon SNS topic for a cluster, use the AWS CLI command `modify-cache-cluster`\.

The following code example adds an Amazon SNS topic arn to *my\-cluster*\.

For Linux, macOS, or Unix:

```
aws elasticache modify-cache-cluster \
    --cache-cluster-id my-cluster \
    --notification-topic-arn arn:aws:sns:us-west-2:123456789xxx:ElastiCacheNotifications
```

For Windows:

```
aws elasticache modify-cache-cluster ^
    --cache-cluster-id my-cluster ^
    --notification-topic-arn arn:aws:sns:us-west-2:123456789xx:ElastiCacheNotifications
```

For more information, see [modify\-cache\-cluster](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-cluster.html)\.

### Adding an Amazon SNS topic \(ElastiCache API\)<a name="ECEvents.SNS.Adding.API"></a>

To add or modify an Amazon SNS topic for a cluster, call the `ModifyCacheCluster` action with the following parameters:
+ `CacheClusterId``=my-cluster`
+ `TopicArn``=arn%3Aaws%3Asns%3Aus-west-2%3A565419523791%3AElastiCacheNotifications`

**Example**  

```
 1. https://elasticache.amazon.com/
 2.     ?Action=ModifyCacheCluster
 3.     &ApplyImmediately=false
 4.     &CacheClusterId=my-cluster
 5.     &NotificationTopicArn=arn%3Aaws%3Asns%3Aus-west-2%3A565419523791%3AElastiCacheNotifications
 6.     &Version=2014-12-01
 7.     &SignatureVersion=4
 8.     &SignatureMethod=HmacSHA256
 9.     &Timestamp=20141201T220302Z
10.     &X-Amz-Algorithm=AWS4-HMAC-SHA256
11.     &X-Amz-Date=20141201T220302Z
12.     &X-Amz-SignedHeaders=Host
13.     &X-Amz-Expires=20141201T220302Z
14.     &X-Amz-Credential=<credential>
15.     &X-Amz-Signature=<signature>
```

For more information, see [ModifyCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html)\.

## Enabling and disabling Amazon SNS notifications<a name="ECEvents.SNS.Disabling"></a>

 You can turn notifications on or off for a cluster\. The following procedures show you how to disable Amazon SNS notifications\. 

### Enabling and disabling Amazon SNS notifications \(Console\)<a name="ECEvents.SNS.Disabling.Console"></a>

**To disable Amazon SNS notifications using the AWS Management Console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. To see a list of your clusters running Redis, in the navigation pane choose **Redis**\.

1. Choose the box to the left of the cluster you want to modify notification for\.

1. Choose **Modify**\.

1. In **Modify Cluster** under **Topic for SNS Notification**, choose *Disable Notifications*\.

1. Choose **Modify**\.

### Enabling and disabling Amazon SNS notifications \(AWS CLI\)<a name="ECEvents.SNS.Disabling.CLI"></a>

To disable Amazon SNS notifications, use the command `modify-cache-cluster` with the following parameters:

For Linux, macOS, or Unix:

```
aws elasticache modify-cache-cluster \
    --cache-cluster-id my-cluster \
    --notification-topic-status inactive
```

For Windows:

```
aws elasticache modify-cache-cluster ^
    --cache-cluster-id my-cluster ^
    --notification-topic-status inactive
```

### Enabling and disabling Amazon SNS notifications \(ElastiCache API\)<a name="ECEvents.SNS.Disabling.API"></a>

To disable Amazon SNS notifications, call the `ModifyCacheCluster` action with the following parameters:
+ `CacheClusterId``=my-cluster`
+ `NotificationTopicStatus``=inactive`

This call returns output similar to the following:

**Example**  

```
 1. https://elasticache.us-west-2.amazonaws.com/
 2.     ?Action=ModifyCacheCluster
 3.     &ApplyImmediately=false
 4.     &CacheClusterId=my-cluster
 5.     &NotificationTopicStatus=inactive
 6.     &Version=2014-12-01
 7.     &SignatureVersion=4
 8.     &SignatureMethod=HmacSHA256
 9.     &Timestamp=20141201T220302Z
10.     &X-Amz-Algorithm=AWS4-HMAC-SHA256
11.     &X-Amz-Date=20141201T220302Z
12.     &X-Amz-SignedHeaders=Host
13.     &X-Amz-Expires=20141201T220302Z
14.     &X-Amz-Credential=<credential>
15.     &X-Amz-Signature=<signature>
```