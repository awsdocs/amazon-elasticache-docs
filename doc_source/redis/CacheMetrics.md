# Monitoring use with CloudWatch Metrics<a name="CacheMetrics"></a>

ElastiCache provides metrics that enable you to monitor your clusters\. You can access these metrics through CloudWatch\. For more information on CloudWatch, see the [CloudWatch documentation\.](https://aws.amazon.com/documentation/cloudwatch/)

ElastiCache provides both host\-level metrics \(for example, CPU usage\) and metrics that are specific to the cache engine software \(for example, cache gets and cache misses\)\. These metrics are measured and published for each Cache node in 60\-second intervals\.

**Important**  
You should consider setting CloudWatch alarms on certain key metrics, so that you will be notified if your cache cluster's performance starts to degrade\. For more information, see [Which Metrics Should I Monitor?](CacheMetrics.WhichShouldIMonitor.md) in this guide\.

**Topics**
+ [Host\-Level Metrics](CacheMetrics.HostLevel.md)
+ [Metrics for Redis](CacheMetrics.Redis.md)
+ [Which Metrics Should I Monitor?](CacheMetrics.WhichShouldIMonitor.md)
+ [Choosing Metric Statistics and Periods](CacheMetrics.ChoosingStatisticsAndPeriods.md)
+ [Monitoring CloudWatch Cluster and Node Metrics](CloudWatchMetrics.md)