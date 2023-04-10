# Getting started with JSON in ElastiCache for Redis<a name="json-gs"></a>

ElastiCache for Redis supports the native JavaScript Object Notation \(JSON\) format, which is a simple, schemaless way to encode complex datasets inside Redis clusters\. You can natively store and access data using the JavaScript Object Notation \(JSON\) format inside Redis clusters, and update JSON data stored in those clusters—without needing to manage custom code to serialize and deserialize it\.

In addition to using Redis API operations for applications that operate over JSON, you can now efficiently retrieve and update specific portions of a JSON document without needing to manipulate the entire object\. This can improve performance and reduce cost\. You can also search your JSON document contents using the [Goessner\-style](https://goessner.net/articles/JsonPath/) `JSONPath` query\. 

After you create a cluster with a supported engine version, the JSON data type and associated commands are automatically available\. This is API compatible and RDB compatible with version 2 of the RedisJSON module, so you can easily migrate existing JSON\-based Redis applications into ElastiCache for Redis\. For more information on the supported Redis commands, see [Supported Redis JSON commands](json-list-commands.md)\.

The JSON\-related metrics `JsonBasedCmds` and `JsonBasedCmdsLatency` are incorporated into CloudWatch to monitor the usage of this data type\. For more information, see [Metrics for Redis](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/CacheMetrics.Redis.html)\.

**Note**  
To use JSON, you must be running Redis engine version 6\.2\.6 or later\.

**Topics**
+ [Redis JSON data type overview](json-document-overview.md)
+ [Supported Redis JSON commands](json-list-commands.md)