# Specifying log delivery using the AWS CLI<a name="CLI_Log"></a>

Create a replication group with slow log delivery to CloudWatch Logs\.

For Linux, macOS, or Unix:

```
aws elasticache create-replication-group \
    --replication-group-id test-slow-log \
    --replication-group-description test-slow-log \
    --engine redis \
    --cache-node-type cache.r5.large \
    --num-cache-clusters 2 \
    --log-delivery-configurations '{
        "LogType":"slow-log", 
        "DestinationType":"cloudwatch-logs",  
        "DestinationDetails":{ 
          "CloudWatchLogsDetails":{ 
            "LogGroup":"my-log-group"
          } 
        }, 
        "LogFormat":"json" 
      }'
```

For Windows:

```
aws elasticache create-replication-group ^
    --replication-group-id test-slow-log ^
    --replication-group-description test-slow-log ^
    --engine redis ^
    --cache-node-type cache.r5.large ^
    --num-cache-clusters 2 ^
    --log-delivery-configurations '{
        "LogType":"slow-log", 
        "DestinationType":"cloudwatch-logs", 
        "DestinationDetails":{ 
          "CloudWatchLogsDetails":{ 
            "LogGroup":"my-log-group"
          } 
        }, 
        "LogFormat":"json" 
      }'
```

Modify a replication group to deliver slow log to CloudWatch Logs

For Linux, macOS, or Unix:

```
aws elasticache modify-replication-group \
    --replication-group-id test-slow-log \
    --apply-immediately \
    --log-delivery-configurations '
    {
      "LogType":"slow-log", 
      "DestinationType":"cloudwatch-logs", 
      "DestinationDetails":{ 
        "CloudWatchLogsDetails":{ 

          "LogGroup":"my-log-group"
        } 
      },
      "LogFormat":"json" 
    }'
```

For Windows:

```
aws elasticache modify-replication-group ^
    --replication-group-id test-slow-log ^
    --apply-immediately ^
    --log-delivery-configurations '
    {
      "LogType":"slow-log", 
      "DestinationType":"cloudwatch-logs", 
      "DestinationDetails":{ 
        "CloudWatchLogsDetails":{ 
          "LogGroup":"my-log-group"
        } 
      },
      "LogFormat":"json" 
    }'
```

Modify a replication group to disable slow log delivery

For Linux, macOS, or Unix:

```
aws elasticache modify-replication-group \
    --replication-group-id test-slow-log \
    --apply-immediately \
    --log-delivery-configurations ' 
    {
      "LogType":"slow-log", 
      "Enabled":false 
    }'
```

For Windows:

```
aws elasticache modify-replication-group ^
    --replication-group-id test-slow-log ^
    --apply-immediately ^
    --log-delivery-configurations '  
    {
      "LogType":"slow-log", 
      "Enabled":false 
    }'
```