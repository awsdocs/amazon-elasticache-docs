# Monitoring CloudWatch Cache Cluster and Cache Node Metrics<a name="CloudWatchMetrics"></a>

ElastiCache and CloudWatch are integrated so you can gather a variety of metrics\. You can monitor these metrics using CloudWatch\. 

**Note**  
The following examples require the CloudWatch command line tools\. For more information about CloudWatch and to download the developer tools, go to the [ CloudWatch product page](https://aws.amazon.com/cloudwatch)\. 

The following procedures show you how to use CloudWatch to gather storage space statistics for an cache cluster for the past hour\. 

**Note**  
The `StartTime` and `EndTime` values supplied in the examples below are for illustrative purposes\. You must substitute appropriate start and end time values for your cache nodes\.

For information on ElastiCache limits, see [AWS Service Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_elasticache) for ElastiCache\.

## Monitoring CloudWatch Cache Cluster and Cache Node Metrics \(Console\)<a name="CloudWatchMetrics.CON"></a>

 **To gather CPU utilization statistics for a cache cluster** 

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. Select the cache nodes you want to view metrics for\. 
**Note**  
Selecting more than 20 nodes disables viewing metrics on the console\.

   1. On the **Cache Clusters** page of the AWS Management Console, click the name of one or more cache clusters\.

      The detail page for the cache cluster appears\. 

   1. Click the **Nodes** tab at the top of the window\.

   1. On the **Nodes** tab of the detail window, select the cache nodes that you want to view metrics for\.

      A list of available CloudWatch Metrics appears at the bottom of the console window\. 

   1. Click on the **CPU Utilization** metric\. 

      The CloudWatch console will open, displaying your selected metrics\. You can use the **Statistic** and **Period** drop\-down list boxes and **Time Range** tab to change the metrics being displayed\. 

## Monitoring CloudWatch Cache Cluster and Cache Node Metrics Using the CloudWatch CLI<a name="CloudWatchMetrics.CLI"></a>

 **To gather CPU utilization statistics for a cache cluster** 

+ Use the CloudWatch command mon\-get\-stats with the following parameters \(note that the start and end times are shown as examples only; you will need to substitute your own appropriate start and end times\):

  For Linux, macOS, or Unix:

  ```
  1. mon-get-stats CPUUtilization \
  2.     --dimensions="CacheClusterId=mycachecluster,CacheNodeId=0002" \
  3.     --statistics=Average \
  4.     --namespace="AWS/ElastiCache" \
  5.     --start-time 2013-07-05T00:00:00 \
  6.     --end-time 2013-07-06T00:00:00 \
  7.     --period=60
  ```

  For Windows:

  ```
  1. mon-get-stats CPUUtilization ^
  2.     --dimensions="CacheClusterId=mycachecluster,CacheNodeId=0002" ^
  3.     --statistics=Average ^
  4.     --namespace="AWS/ElastiCache" ^
  5.     --start-time 2013-07-05T00:00:00 ^
  6.     --end-time 2013-07-06T00:00:00 ^
  7.     --period=60
  ```

## Monitoring CloudWatch Cache Cluster and Cache Node Metrics Using the CloudWatch API<a name="CloudWatchMetrics.API"></a>

 **To gather CPU utilization statistics for a cache cluster** 

+ Call the CloudWatch API `GetMetricStatistics` with the following parameters \(note that the start and end times are shown as examples only; you will need to substitute your own appropriate start and end times\):

  + `Statistics.member.1``=Average`

  + `Namespace``=AWS/ElastiCache`

  + `StartTime``=2013-07-05T00:00:00`

  + `EndTime``=2013-07-06T00:00:00`

  + `Period``=60`

  + `MeasureName``=CPUUtilization`

  + `Dimensions``=CacheClusterId=mycachecluster,CacheNodeId=0002`  
**Example**  

  ```
   1. http://monitoring.amazonaws.com/
   2.     ?SignatureVersion=4
   3.     &Action=GetMetricStatistics
   4.     &Version=2014-12-01
   5.     &StartTime=2013-07-16T00:00:00
   6.     &EndTime=2013-07-16T00:02:00
   7.     &Period=60
   8.     &Statistics.member.1=Average
   9.     &Dimensions.member.1="CacheClusterId=mycachecluster"
  10.     &Dimensions.member.2="CacheNodeId=0002"
  11.     &Namespace=AWS/ElastiCache
  12.     &MeasureName=CPUUtilization						
  13.     &Timestamp=2013-07-07T17%3A48%3A21.746Z
  14.     &AWSAccessKeyId=<AWS Access Key ID>
  15.     &Signature=<Signature>
  ```