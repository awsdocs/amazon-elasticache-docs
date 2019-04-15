# What Is Amazon ElastiCache for Redis?<a name="WhatIs"></a>

Welcome to the *Amazon ElastiCache for Redis User Guide*\. ElastiCache is a web service that makes it easy to set up, manage, and scale a distributed in\-memory data store or cache environment in the cloud\. It provides a high\-performance, scalable, and cost\-effective caching solution, while removing the complexity associated with deploying and managing a distributed cache environment\.


|  | 
| --- |
|    The Amazon ElastiCache documentation has been reorganized so that the information for the Memcached and Redis engines is in separate guides\. Your former links might no longer work\. If you know which guide you need, go directly there by choosing the link to the guide you need\. If you're unsure which engine you want to use, see [Comparing Memcached and Redis](SelectEngine.md) in this guide\.  | 
|  ![\[\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/memcached-icon.png) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/WhatIs.html) |  ![\[\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/redis-icon.png) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/WhatIs.html) | 

Existing applications that use Redis can use ElastiCache with almost no modification\. Your applications simply need information about the host names and port numbers of the ElastiCache nodes that you have deployed\. 

ElastiCache for Redis has multiple features to enhance reliability for critical production deployments:
+ Automatic detection and recovery from cache node failures\.
+ Multi\-AZ with automatic failover of a failed primary cluster to a read replica in Redis clusters that support replication\.
+ Redis \(cluster mode enabled\) supports partitioning your data across up to 90 shards\.
+ Starting with Redis version 3\.2\.6, all subsuquent supported versions support in\-transit and at\-rest encryption with authentication so you can build HIPAA\-compliant applications\. 
+ Flexible Availability Zone placement of nodes and clusters for increased fault tolerance\.
+ Integration with other AWS services such as Amazon EC2, Amazon CloudWatch, AWS CloudTrail, and Amazon SNS to provide a secure, high\-performance, managed in\-memory caching solution\.

**Topics**
+ [Use Cases and How ElastiCache Can Help](elasticache-use-cases.md)
+ [Amazon ElastiCache Resources](WhatIs.FirstTimeUser.md)
+ [ElastiCache for Redis Components and Features](WhatIs.Components.md)
+ [ElastiCache for Redis Terminology](WhatIs.Terms.md)
+ [Tools for Managing Your Implementation](WhatIs.Managing.md)