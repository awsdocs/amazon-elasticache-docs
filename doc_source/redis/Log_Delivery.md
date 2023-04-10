# Log delivery<a name="Log_Delivery"></a>

**Note**  
Redis Slow Log is supported for Redis cache clusters and replication groups using engine version 6\.0 onward\.   
Redis Engine Log is supported for Redis cache clusters and replication groups using engine version 6\.2 onward\.

Log delivery lets you stream Redis [SLOWLOG](https://redis.io/commands/slowlog) or **Redis Engine Log** to one of two destinations:
+ Amazon Kinesis Data Firehose
+ Amazon CloudWatch Logs

You enable and configure log delivery when you create or modify a cluster using ElastiCache APIs\. Each log entry will be delivered to the specified destination in one of two formats: *JSON* or *TEXT*\.

A fixed number of Slow log entries are retrieved from the Redis engine periodically\. Depending on the value specified for engine parameter `slowlog-max-len`, additional slow log entries might not be delivered to the destination\.

You can choose to change the delivery configurations or disable log delivery at any time using the AWS console or one of the modify APIs, either [modify\-cache\-cluster](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-cluster.html) or [modify\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group.html)\. 

You must set the `apply-immediately` parameter for all log delivery modifications\.

**Note**  
Amazon CloudWatch Logs charges apply when log delivery is enabled, even when logs are delivered directly to Amazon Kinesis Data Firehose\. For more information, see Vended Logs section in [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

## Contents of a slow log entry<a name="Log_contents"></a>

The ElastiCache for Redis Slow Log contains the following information: 
+ **CacheClusterId** – The ID of the cache cluster
+ **CacheNodeId** – The ID of the cache node
+ **Id** – A unique progressive identifier for every slow log entry
+ **Timestamp** – The Unix timestamp at which the logged command was processed
+ **Duration** – The amount of time needed for its execution, in microseconds
+ **Command** – The command used by the client\. For example, `set foo bar` where `foo` is the key and `bar` is the value\. ElastiCache for Redis replaces the actual key name and value with `(2 more arguments)` to avoid exposing sensitive data\.
+ **ClientAddress** – Client IP address and port
+ **ClientName** – Client name if set via the `CLIENT SETNAME` command 

## Contents of an engine log entry<a name="Log_contents-engine-log"></a>

The ElastiCache for Redis Engine Log contains the following information: 
+ **CacheClusterId** – The ID of the cache cluster
+ **CacheNodeId** – The ID of the cache node
+ **Log level** – LogLevel can one of the following: `VERBOSE("-")`, `NOTICE("*")`, `WARNING("#")`\.
+ **Time** – The UTC time of the logged message\. Time is in following format: `"DD MMM YYYY hh:mm:ss.ms UTC"`
+ **Role** – Role of the node from where the log is emitted\. It can be one of the following: “M” for Primary, “S” for replica, "C" for writer child process working on RDB/AOF or "X" for sentinel\.
+ **Message** – Redis Engine log message\.

## Permissions to configure logging<a name="Log_permissions"></a>

You need to include the following IAM permissions in your IAM user/role policy: 
+ `logs:CreateLogDelivery`
+ `logs:UpdateLogDelivery`
+ `logs:DeleteLogDelivery`
+ `logs:GetLogDelivery`
+ `logs:ListLogDeliveries`

For more information, see [Overview of access management: Permissions and policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_access-management.html)\.

## Log type and log format specifications<a name="Destination_Formats"></a>

### Slow log<a name="Destination_Formats-slowlog"></a>

Slow log supports both JSON and TEXT

The following shows a JSON format example:

```
{
  "CacheClusterId": "logslowxxxxmsxj", 
  "CacheNodeId": "0001", 
  "Id": 296, 
  "Timestamp": 1605631822, 
  "Duration (us)": 0, 
  "Command": "GET ... (1 more arguments)", 
  "ClientAddress": "192.168.12.104:55452", 
  "ClientName": "logslowxxxxmsxj##" 
}
```

The following shows a TEXT format example:

```
logslowxxxxmsxj,0001,1605631822,30,GET ... (1 more arguments),192.168.12.104:55452,logslowxxxxmsxj## 
```

### Engine log<a name="Destination_Formats-engine-log"></a>

Engine log supports both JSON and TEXT

The following shows a JSON format example:

```
{ 
  "CacheClusterId": "xxxxxxxxxzy-engine-log-test", 
  "CacheNodeId": "0001", 
  "LogLevel": "VERBOSE", 
  "Role": "M", 
  "Time": "12 Nov 2020 01:28:57.994 UTC", 
  "Message": "Replica is waiting for next BGSAVE before synchronizing with the primary. Check back later" 
}
```

The following shows a TEXT format example:

```
xxxxxxxxxxxzy-engine-log-test/0001:M 29 Oct 2020 20:12:20.499 UTC * A slow-running Lua script detected that is still in execution after 10000 milliseconds.
```