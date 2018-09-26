# Tools for Managing Your Implementation<a name="WhatIs.Managing"></a>

Once you have granted your Amazon EC2 instance access to your ElastiCache cluster, you have four means by which you can manage your ElastiCache cluster: the AWS Management Console, the AWS CLI for ElastiCache, the AWS SDK for ElastiCache, and the ElastiCache API\.

## Using the AWS Management Console<a name="WhatIs.Managing.Means.CON"></a>

The AWS Management Console is the easiest way to manage Amazon ElastiCache for Memcached\. The console lets you create cache clusters, add and remove cache nodes, and perform other administrative tasks without having to write any code\. The console also provides cache node performance graphs from CloudWatch, showing cache engine activity, memory and CPU utilization, as well as other metrics\. For more information, see specific topics in this *User Guide*\.

## Using the AWS CLI<a name="WhatIs.Managing.Means.CLI"></a>

You can also use the AWS Command Line Interface \(AWS CLI\) for ElastiCache\. The AWS CLI makes it easy to perform one\-at\-a\-time operations, such as starting or stopping your cache cluster\. You can also invoke AWS CLI for ElastiCache commands from a scripting language of your choice, letting you automate repeating tasks\. For more information about the AWS CLI, see the *User Guide* and the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)\.

## Using the AWS SDK<a name="WhatIs.Managing.Means.SDK"></a>

If you want to access ElastiCache from an application, you can use one of the AWS software development kits \(SDKs\)\. The SDKs wrap the ElastiCache API calls, and insulate your application from the low\-level details of the ElastiCache API\. You provide your credentials, and the SDK libraries take care of authentication and request signing\. For more information about using the AWS SDKs, see [Tools for Amazon Web Services](https://aws.amazon.com/tools/)\.

## Using the ElastiCache API<a name="WhatIs.Managing.Means.API"></a>

You can also write application code directly against the ElastiCache web service API\. When using the API, you must write the necessary code to construct and authenticate your HTTP requests, parse the results from ElastiCache, and handle any errors\. For more information about the API, see [Using the ElastiCache API](ProgrammingGuide.md)\.

## See also<a name="what-is-managing-see-also"></a>

For more detailed information on managing your Amazon ElastiCache for Memcached deployment, see the following:
+ [Managing Your ElastiCache for Memcached Implementation](managing-elasticache.md)
+ [Securing Network Access](Security.md)
+ [Monitoring Usage, Events, and Costs](MonitoringECMetrics.md)