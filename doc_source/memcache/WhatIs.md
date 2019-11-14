# What Is Amazon ElastiCache for Memcached?<a name="WhatIs"></a>

Welcome to the *Amazon ElastiCache for Memcached User Guide*\. Amazon ElastiCache is a web service that makes it easy to set up, manage, and scale a distributed in\-memory data store or cache environment in the cloud\. It provides a high\-performance, scalable, and cost\-effective caching solution\. At the same time, it helps remove the complexity associated with deploying and managing a distributed cache environment\.

**Note**  
Amazon ElastiCache works with both the Redis and Memcached engines\. Use the guide for the engine that you're interested in\. If you're unsure which engine you want to use, see [Comparing Memcached and Redis](SelectEngine.md) in this guide\.

Existing applications that use Memcached can use ElastiCache with almost no modification\. Your applications simply need information about the host names and port numbers of the ElastiCache nodes that you have deployed\. The ElastiCache Auto Discovery feature for Memcached lets your applications identify all of the nodes in a cache cluster and connect to them\. This means that you don't have to maintain a list of available host names and port numbers\. In this way, your applications are effectively insulated from changes to node membership in a cluster\.

ElastiCache for Memcached has multiple features to enhance reliability for critical production deployments:
+ Automatic detection and recovery from cache node failures\.
+ Automatic discovery of nodes within a cluster enabled for automatic discovery, so that no changes need to be made to your application when you add or remove nodes\.
+ Flexible Availability Zone placement of nodes and clusters\.
+ Integration with other AWS services such as Amazon EC2, Amazon CloudWatch, AWS CloudTrail, and Amazon SNS to provide a secure, high\-performance, managed in\-memory caching solution\.

**Topics**
+ [Common ElastiCache Use Cases and How ElastiCache Can Help](elasticache-use-cases.md)
+ [ElastiCache for Memcached Resources](WhatIs.FirstTimeUser.md)
+ [ElastiCache for Memcached Components and Features](WhatIs.Components.md)
+ [Tools for Managing Your Implementation](WhatIs.Managing.md)