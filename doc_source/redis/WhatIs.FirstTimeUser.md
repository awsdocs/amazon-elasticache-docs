# Amazon ElastiCache resources<a name="WhatIs.FirstTimeUser"></a>

We recommend that you begin by reading the following sections, and refer to them as you need them:
+ **Service highlights and pricing** – The [product detail page](http://aws.amazon.com/elasticache/) provides a general product overview of ElastiCache, service highlights, and pricing\.
+ **ElastiCache videos** – The [ElastiCache Videos](Tutorials.md#tutorial-videos) section has videos that introduce you to Amazon ElastiCache\. The videos cover common use cases for ElastiCache and demo how to use ElastiCache to reduce latency and improve throughput for your applications\.
+ **Getting started** – The [Getting started with Amazon ElastiCache for Redis](GettingStarted.md) section includes information on creating a cache cluster\. It also includes how to authorize access to the cache cluster, connect to a cache node, and delete the cache cluster\.
+ **Performance at scale** – The [Performance at scale with Amazon ElastiCache](https://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf) whitepaper addresses caching strategies that help your application to perform well at scale\.

After you complete the preceding sections, read these sections:
+ [Choosing your node size](nodes-select-size.md#CacheNodes.SelectSize)

  You want your nodes to be large enough to accommodate all the data you want to cache\. At the same time, you don't want to pay for more cache than you need\. You can use this topic to help select the best node size\.
+ [Caching strategies and best practices](BestPractices.md)

  Identify and address issues that can impact the efficiency of your cluster\.

If you want to use the AWS Command Line Interface \(AWS CLI\), you can use these documents to help you get started:
+ [AWS Command Line Interface documentation](https://docs.aws.amazon.com/cli/)

  This section provides information on downloading the AWS CLI, getting the AWS CLI working on your system, and providing your AWS credentials\.
+ [AWS CLI documentation for ElastiCache](https://docs.aws.amazon.com/cli/latest/reference/elasticache/index.html)

  This separate document covers all of the AWS CLI for ElastiCache commands, including syntax and examples\.

You can write application programs to use the ElastiCache API with a variety of popular programming languages\. Here are some resources:
+ [Tools for AWSlong;](http://aws.amazon.com/tools/)

  AWSlong; provides a number of software development kits \(SDKs\) with support for ElastiCache\. You can code for ElastiCache using Java, \.NET, PHP, Ruby, and other languages\. These SDKs can greatly simplify your application development by formatting your requests to ElastiCache, parsing responses, and providing retry logic and error handling\. 
+ [Using the ElastiCache API](ProgrammingGuide.md)

  If you don't want to use the AWS SDKs, you can interact with ElastiCache directly using the Query API\. You can find troubleshooting tips and information on creating and authenticating requests and handling responses in this section\. 
+ [Amazon ElastiCache API Reference](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/)

  This separate document covers all of the ElastiCache API operations, including syntax and examples\.