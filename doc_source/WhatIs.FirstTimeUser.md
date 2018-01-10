# Amazon ElastiCache Resources<a name="WhatIs.FirstTimeUser"></a>

We recommend that you begin by reading the following sections, and refer to them as you need them:

+ **Service Highlights and Pricing** – The [product detail page](https://aws.amazon.com/elasticache/) provides a general product overview of ElastiCache, service highlights, and pricing\.

+ **ElastiCache Videos** – The [ElastiCache Tutorial Videos](WhatIs.Videos.md) section has videos that introduce you to Amazon ElastiCache, cover common use cases for ElastiCache, and demo how to use ElastiCache to reduce latency and improve throughput of your applications\.

+ **Getting Started** – The [Getting Started with Amazon ElastiCache](GettingStarted.md) section includes an example that walks you through the process of creating a cache cluster, authorizing access to the cache cluster, connecting to a cache node, and deleting the cache cluster\.

+ **Performance at Scale** – The [Performance at Scale with Amazon ElastiCache](https://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf) white paper addresses caching strategies that enable your application to perform well at scale\.

After you complete the preceding sections, read these sections:

+ [Engines and Versions](SelectEngine.md)

  ElastiCache supports two engines—Memcached and Redis\. This topic helps you determine which engine is best for your scenario\.

+ [Choosing Your Node Size](CacheNodes.SelectSize.md)

  You want your nodes to be large enough to accommodate all the data you want to cache\. At the same time, you don't want to pay for more cache than you need\. You can use this topic to help select the best node size\.

+ [Best Practices for Amazon ElastiCache](BestPractices.md)

  Identify and address issues that can impact the efficiency of your cluster\.

If you want to use the AWS Command Line Interface \(AWS CLI\), you can use these documents to help you get started:

+ [AWS Command Line Interface Documentation](http://docs.aws.amazon.com/cli/)

  This section provides information on downloading the AWS CLI, getting the AWS CLI working on your system, and providing your AWS credentials\.

+ [AWS CLI Documentation for ElastiCache](http://docs.aws.amazon.com/cli/latest/reference/elasticache/index.html)

  This separate document covers all of the AWS CLI for ElastiCache commands, including syntax and examples\.

You can write application programs to use the ElastiCache API with a variety of popular programming languages\. Here are some resources:

+ [Tools for Amazon Web Services](https://aws.amazon.com/tools/)

  Amazon Web Services provides a number of software development kits \(SDKs\) with support for ElastiCache\. You can code for ElastiCache using Java, \.NET, PHP, Ruby, and other languages\. These SDKs can greatly simplify your application development by formatting your requests to ElastiCache, parsing responses, and providing retry logic and error handling\. 

+ [Using the ElastiCache API](ProgrammingGuide.md)

  If you don't want to use the AWS SDKs, you can interact with ElastiCache directly using the Query API\. You can find troubleshooting tips and information on creating and authenticating requests and handling responses in this section\. 

+ [Amazon ElastiCache API Reference](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/)

  This separate document covers all of the ElastiCache API operations, including syntax and examples\.