# Python and ElastiCache<a name="ElastiCache-Getting-Started-Tutorials-Python"></a>

In this tutorial, you use the AWS SDK for Python \(Boto3\) to write simple programs to perform the following ElastiCache operations:
+ Create ElastiCache clusters \(cluster mode enabled and cluster mode disabled\)
+ Check if users or user groups exist, otherwise create them \(Redis 6\.x onwards only\)
+ Connect to ElastiCache
+ Perform operations such as setting and getting strings, reading from and writing to steams and publishing and subscribing from Pub/Sub channel\.

As you work through this tutorial, you can refer to the AWS SDK for Python \(Boto\) documentation\. The following section is specific to ElastiCache:

[ElastiCache low\-level client](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/elasticache.html)

## Tutorial Prerequisites<a name="ElastiCache-Getting-Started-Tutorials-Prerquisites"></a>
+ Set up an AWS access key to use the AWS SDKs\. For more information, see [Setting up](set-up.md)\.
+ Install Python 2\.6 or later\. For more information, see [https://www\.python\.org/downloads](https://www.python.org/downloads)\. For instructions, see [Quickstart](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html) in the Boto 3 documentation\.