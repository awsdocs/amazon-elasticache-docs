# Choosing Metric Statistics and Periods<a name="CacheMetrics.ChoosingStatisticsAndPeriods"></a>

While CloudWatch will allow you to choose any statistic and period for each metric, not all combinations will be useful\. For example, the Average, Minimum, and Maximum statistics for CPUUtilization are useful, but the Sum statistic is not\.

All ElastiCache samples are published for a 60 second duration for each individual cache node\. For any 60 second period, a cache node metric will only contain a single sample\.

For further information on how to retrieve metrics for your cache nodes, see [Monitoring CloudWatch Cluster and Node Metrics](CloudWatchMetrics.md)\.