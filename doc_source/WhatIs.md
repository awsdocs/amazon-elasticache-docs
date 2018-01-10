# What Is Amazon ElastiCache?<a name="WhatIs"></a>

Welcome to the *Amazon ElastiCache User Guide*\. ElastiCache is a web service that makes it easy to set up, manage, and scale a distributed in\-memory data store or cache environment in the cloud\. It provides a high\-performance, scalable, and cost\-effective caching solution, while removing the complexity associated with deploying and managing a distributed cache environment\.

With ElastiCache, you can quickly deploy your cache environment, without having to provision hardware or install software\. You can choose from Memcached or Redis protocol\-compliant cache engine software, and let ElastiCache perform software upgrades and patch management for you\. For enhanced security, ElastiCache can be run in the Amazon Virtual Private Cloud \(Amazon VPC\) environment, giving you complete control over network access to your clusters\. With just a few clicks in the AWS Management Console, you can add or remove resources such as nodes, clusters, or read replicas to your ElastiCache environment to meet your business needs and application requirements\. 

Existing applications that use Memcached or Redis can use ElastiCache with almost no modification\. Your applications simply need to know the host names and port numbers of the ElastiCache nodes that you have deployed\. The ElastiCache Auto Discovery feature for Memcached lets your applications identify all of the nodes in a cache cluster and connect to them, rather than having to maintain a list of available host names and port numbers\. In this way, your applications are effectively insulated from changes to node membership in a cluster\.

ElastiCache has multiple features to enhance reliability for critical production deployments:

+ Automatic detection and recovery from cache node failures\.

+ Multi\-AZ with Automatic Failover of a failed primary cluster to a read replica in Redis clusters that support replication \(called *replication groups* in the ElastiCache API and AWS CLI\.

+ Flexible Availability Zone placement of nodes and clusters\.

+ Integration with other AWS services such as Amazon EC2, Amazon CloudWatch, AWS CloudTrail, and Amazon SNS to provide a secure, high\-performance, managed in\-memory caching solution\.


+ [ElastiCache Use Cases](UseCases.md)
+ [Amazon ElastiCache Resources](WhatIs.FirstTimeUser.md)
+ [ElastiCache Tutorial Videos](WhatIs.Videos.md)
+ [ElastiCache Components and Features](WhatIs.Components.md)
+ [ElastiCache for Redis Terminology](WhatIs.Terms.md)
+ [Accessing Amazon ElastiCache](WhatIs.Accessing.md)
+ [Managing ElastiCache](WhatIs.Managing.md)