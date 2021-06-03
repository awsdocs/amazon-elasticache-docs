# Specifying log delivery using the Console<a name="Console_Log"></a>

Using the AWS Management Console you can create a Redis \(cluster mode disabled\) cluster by following the steps at [Creating a Redis \(cluster mode disabled\) cluster \(Console\)](GettingStarted.CreateCluster.md#Clusters.Create.CON.Redis-gs) or create a Redis \(cluster mode enabled\) cluster using the steps at [Creating a Redis \(cluster mode enabled\) cluster \(Console\)](Clusters.Create.md#Clusters.Create.CON.RedisCluster)\. In either case, you configure log delivery by doing the following;

1. Under **Advanced Redis settings**, choose **Logs** and then check **Slow logs**\.

1. Under **Log format**, choose either **Text** or **JSON**\.

1. Under **Destination Type**, choose either **CloudWatch Logs** or **Kinesis Firehose**\.

1. Under **Log destination**, choose either **Create new** and enter either your CloudWatchLogs log group name or your Kinesis Data Firehose stream name, or choose **Select existing** and then choose either your CloudWatchLogs log group name or your Kinesis Data Firehose stream name,

**When modifying a cluster:**

You can choose to either enable/disable log delivery or change either the destination type, format or destination:

1. Sign in to the Console and open the ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Redis**\.

1. From the list of clusters, choose the cluster you want to modify\. Choose the **Cluster name** and not the checkbox beside it\.

1. On the **Cluster name** page, choose the **Logs** tab\.

1. To enable/disable slow logs, choose either **Enable slow logs** or **Disable slow logs**\.

1. To change your configuration, choose **Modify slow logs**:
   + Under **Destination Type**, choose either **CloudWatch Logs** or **Kinesis Firehose**\.
   + Under **Log destination**, choose either **Create new** and enter either your CloudWatchLogs log group name or your Kinesis Data Firehose stream name\. Or choose **Select existing** and then choose either your CloudWatchLogs log group name or your Kinesis Data Firehose stream name\.