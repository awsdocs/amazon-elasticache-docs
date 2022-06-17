# Applying the self\-service updates<a name="applying-updates"></a>

You can start applying the service updates to your Memcached fleet from the time that the updates have an **available** status until they have an **expired** status\. Service updates of the type **security** are cumulative\. In other words, any nonexpired updates that you haven't applied yet are included with your latest update\. Service updates of the type **engine** are cumulative and may also contain security updates\.

**Note**  
You can apply only those service updates that have an **available** status, even if the recommended apply by date is past due\. 

For more information about reviewing your Memcached fleet and applying any service\-specific updates to applicable Memcached clusters, see [Applying the service updates using the console for Memcached](applying-updates-console.md#applying-updates-console-memcached-console)\.

When a new service update is available for one or more Memcached clusters, you can use the ElastiCache console, API, or AWS CLI to apply the update\. The following sections explain the options that you can use to apply updates\.

**Topics**
+ [Applying the service updates using the console](applying-updates-console.md)
+ [Applying the service updates using the AWS CLI](applying-updates-cli-memcached.md)