# ElastiCache logging destinations<a name="Logging-destinations"></a>

This section describes the logging destinations that you can choose for your ElastiCache logs\. Each section provides guidance for configuring logging for the destination type and information about any behavior that's specific to the destination type\. After you've configured your logging destination, you can provide its specifications to the ElastiCache logging configuration to start logging to it\.

**Topics**
+ [Amazon CloudWatch Logs](#Destination_Specs_CloudWatch_Logs)
+ [Amazon Kinesis Data Firehose](#Destination_Specs_Kinesis_Firehose_Stream)

## Amazon CloudWatch Logs<a name="Destination_Specs_CloudWatch_Logs"></a>
+ You specify a CloudWatch Logs log group where the logs will be delivered\. 
+ Logs from multiple Redis clusters and replication groups can be delivered to the same log group\. 
+ A new log stream will be created for each node within a cache cluster or replication group and the logs will be delivered to the respective log streams\. The log stream name will use the following format: `elasticache/${engine-name}/${cache-cluster-id}/${cache-node-id}/${log-type}`

**Permissions to publish logs to CloudWatch Logs** 

You must have the following permissions settings to configure ElastiCache for Redis to send logs to a CloudWatch Logs log group:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "logs:CreateLogDelivery",
                "logs:GetLogDelivery",
                "logs:UpdateLogDelivery",
                "logs:DeleteLogDelivery",
                "logs:ListLogDeliveries"
            ],
            "Resource": [
                "*"
            ],
            "Effect": "Allow",
            "Sid": "ElastiCacheLogging"
        },
        {
            "Sid": "ElastiCacheLoggingCWL",
            "Action": [
                "logs:PutResourcePolicy",
                "logs:DescribeResourcePolicies",
                "logs:DescribeLogGroups"
            ],
            "Resource": [
                "*"
            ],
            "Effect": "Allow"
        }
    ]
}
```

## Amazon Kinesis Data Firehose<a name="Destination_Specs_Kinesis_Firehose_Stream"></a>
+ You specify a Kinesis Data Firehose delivery stream where the logs will be delivered\. 
+ Logs from multiple Redis clusters and replication groups can be delivered to the same delivery stream\. 
+ Logs from each node within a cache cluster or replication group will be delivered to the same delivery stream\. You can distinguish log messages from different cache nodes based on the `cache-cluster-id` and `cache-node-id` included in each log message\. 
+ Log delivery to Kinesis Data Firehose is currently not available in the Asia Pacific \(Osaka\) Region\. 

**Permissions to publish logs to Kinesis Data Firehose** 

You must have the following permissions to configure ElastiCache for Redis to send logs to an Amazon Kinesis Data Firehose delivery stream\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "logs:CreateLogDelivery",
                "logs:GetLogDelivery",
                "logs:UpdateLogDelivery",
                "logs:DeleteLogDelivery",
                "logs:ListLogDeliveries"
            ],
            "Resource": [
                "*"
            ],
            "Effect": "Allow",
            "Sid": "ElastiCacheLogging"
        },
        {
            "Sid": "ElastiCacheLoggingFHSLR",
            "Action": [
                "iam:CreateServiceLinkedRole"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Sid": "ElastiCacheLoggingFH",
            "Action": [
                "firehose:TagDeliveryStream"
            ],
            "Resource": "Amazon Kinesis Data Firehose delivery stream ARN",
            "Effect": "Allow"
        }
    ]
}
```