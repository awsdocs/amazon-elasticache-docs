# What Is Amazon ElastiCache for Redis?<a name="WhatIs"></a>

Welcome to the *Amazon ElastiCache for Redis User Guide*\. Amazon ElastiCache is a web service that makes it easy to set up, manage, and scale a distributed in\-memory data store or cache environment in the cloud\. It provides a high\-performance, scalable, and cost\-effective caching solution\. At the same time, it helps remove the complexity associated with deploying and managing a distributed cache environment\.

**Note**  
Amazon ElastiCache works with both the Redis and Memcached engines\. Use the guide for the engine that you're interested in\. If you're unsure which engine you want to use, see [Comparing Memcached and Redis](SelectEngine.md) in this guide\.

Existing applications that use Redis can use ElastiCache with almost no modification\. Your applications simply need information about the host names and port numbers of the ElastiCache nodes that you have deployed\. 

ElastiCache for Redis has multiple features that help make the service more reliable for critical production deployments:
+ Automatic detection of and recovery from cache node failures\.
+ Multi\-AZ for a failed primary cluster to a read replica, in Redis clusters that support replication\.
+ Redis \(cluster mode enabled\) supports partitioning your data across up to 90 shards\.
+ For Redis version 3\.2\.and later, all versions support encryption in transit and encryption at rest encryption with authentication\. This support helps you build HIPAA\-compliant applications\. 
+ Flexible Availability Zone placement of nodes and clusters for increased fault tolerance\.
+ Integration with other AWS services such as Amazon EC2, Amazon CloudWatch, AWS CloudTrail, and Amazon SNS\. This integration helps provide a managed in\-memory caching solution that is high\-performance and highly secure\.

**Topics**
+ [Common ElastiCache Use Cases and How ElastiCache Can Help](elasticache-use-cases.md)
+ [Amazon ElastiCache Resources](WhatIs.FirstTimeUser.md)
+ [ElastiCache for Redis Components and Features](WhatIs.Components.md)
+ [ElastiCache for Redis Terminology](WhatIs.Terms.md)
+ [Tools for Managing Your Implementation](WhatIs.Managing.md)