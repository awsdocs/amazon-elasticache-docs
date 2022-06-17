# ElastiCache for Memcached resources<a name="WhatIs.FirstTimeUser"></a>

We recommend that you begin by reading the following sections, and refer to them as you need them:
+ **Service Highlights and Pricing** – The [product detail page](https://aws.amazon.com/elasticache/) provides a general product overview of ElastiCache, service highlights, and pricing\.
+ **ElastiCache Videos** – The [ElastiCache Videos](Tutorials.md#tutorial-videos) section has videos that introduce you to Amazon ElastiCache for Memcached, cover common use cases, and demo how to use ElastiCache for Memcached to reduce latency and improve throughput of your applications\.
+ **Getting Started** – The [Getting started with Amazon ElastiCache for Memcached](GettingStarted.md) section includes an example that walks you through the process of creating a cache cluster, authorizing access to the cache cluster, connecting to a cache node, and deleting the cache cluster\.
+ **Performance at Scale** – The [Performance at scale with Amazon ElastiCache](https://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf) white paper addresses caching strategies that enable your application to perform well at scale\.

After you complete the preceding sections, read these sections:
+ [Choosing your Memcached node size](nodes-select-size.md#CacheNodes.SelectSize)

  You want your nodes to be large enough to accommodate all the data you want to cache\. At the same time, you don't want to pay for more cache than you need\. You can use this topic to help select the best node size\.
+ [Caching strategies and best practices](BestPractices.md)

  Identify and address issues that can impact the efficiency of your cluster\.

If you want to use the AWS Command Line Interface \(AWS CLI\), you can use these documents to help you get started:
+ [AWS Command Line Interface documentation](https://docs.aws.amazon.com/cli/)

  This section provides information on downloading the AWS CLI, getting the AWS CLI working on your system, and providing your AWS credentials\.
+ [AWS CLI documentation for ElastiCache](https://docs.aws.amazon.com/cli/latest/reference/elasticache/index.html)

  This separate document covers all of the AWS CLI for ElastiCache commands, including syntax and examples\.

You can write application programs to use the ElastiCache API with a variety of popular programming languages\. Here are some resources:
+ [Tools for AWSlong;](https://aws.amazon.com/tools/)

  AWSlong; provides a number of software development kits \(SDKs\) with support for ElastiCache for Memcached\. You can code for ElastiCache using Java, \.NET, PHP, Ruby, and other languages\. These SDKs can greatly simplify your application development by formatting your requests to ElastiCache, parsing responses, and providing retry logic and error handling\. 
+ [Using the ElastiCache API](ProgrammingGuide.md)

  If you don't want to use the AWS SDKs, you can interact with ElastiCache directly using the Query API\. You can find troubleshooting tips and information on creating and authenticating requests and handling responses in this section\. 
+ [Amazon ElastiCache API Reference](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/)

  This separate document covers all of the ElastiCache API operations, including syntax and examples\.