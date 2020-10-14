# Using Local Zones with ElastiCache<a name="Local_zones"></a>

A *Local Zone* is an extension of an AWS Region that is geographically close to your users\. You can extend any virtual private cloud \(VPC\) from a parent AWS Region into a Local Zones by creating a new subnet and assigning it to the Local Zone\. When you create a subnet in a Local Zone, your VPC is extended to that Local Zone\. The subnet in the Local Zone operates the same as other subnets in your VPC\.

By using Local Zones, you can place resources such as an ElastiCache cluster in multiple locations close to your users\. 

When you create an ElastiCache cluster, you can choose a subnet in a Local Zone\. Local Zones have their own connections to the internet and support AWS Direct Connect\. Thus, resources created in a Local Zone can serve local users with very low\-latency communications\. For more information, see [AWS Local Zones](https://aws.amazon.com/about-aws/global-infrastructure/localzones/)\. 

A Local Zone is represented by an AWS Region code followed by an identifier that indicates the location, for example `us-west-2-lax-1a`\.

At this time, the available Local Zones are `us-west-2-lax-1a` and `us-west-2-lax-1b`\.

The following limitations apply to ElastiCache for Local Zones:
+  A Local Zone can't be included in a Multi\-AZ deployment\.
+ Global datastores aren't supported\.
+ Online migration isn't supported\.
+ The following node types are supported by Local Zones at this time: 
  + Current generation: 

    **M5 node types:** `cache.m5.large`, `cache.m5.xlarge`, `cache.m5.2xlarge`, `cache.m5.4xlarge`, `cache.m5.12xlarge`, `cache.m5.24xlarge` 

    **R5 node types:** `cache.r5.large`, `cache.r5.xlarge`, `cache.r5.2xlarge`, `cache.r5.4xlarge`, `cache.r5.12xlarge`, `cache.r5.24xlarge`

    **T3 node types:** `cache.t3.micro`, `cache.t3.small`, `cache.t3.medium`

## Enabling a Local Zone<a name="Local_zones-using"></a>

1. Enable the Local Zone in the Amazon EC2 console\.

   For more information, see [Enabling Local Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#enable-zone-group) in the *Amazon EC2 User Guide*\.

1. Create a subnet in the Local Zone\.

   For more information, see [Creating a subnet in your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#AddaSubnet) in the *Amazon VPC User Guide*\.

1. Create an ElastiCache subnet group in the Local Zone\.

   When you create an ElastiCache subnet group, choose the Availability Zone group for the Local Zone\.

   For more information, see [Creating a subnet group](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/SubnetGroups.Creating.html) in the *ElastiCache User Guide*\.

1. Create an ElastiCache for Memcached cluster that uses the ElastiCache subnet in the Local Zone\.

    For more information, see [Creating a Memcached Cluster \(Console\)](Clusters.Create.CON.Memcached.md)\.