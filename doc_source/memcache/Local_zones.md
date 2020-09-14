# Local Zones<a name="Local_zones"></a>

A *Local Zone* is an extension of an AWS Region that is geographically close to your users\. You can extend any VPC from the parent AWS Region into Local Zones by creating a new subnet and assigning it to the AWS Local Zone\. When you create a subnet in a Local Zone, your VPC is extended to that Local Zone\. The subnet in the Local Zone operates the same as other subnets in your VPC\.

 When you create an ElastiCache cluster, you can choose a subnet in a Local Zone\. Local Zones have their own connections to the internet and support AWS Direct Connect\. Thus, resources created in a Local Zone can serve local users with very low\-latency communications\. For more information, see [AWS Local Zones](https://aws.amazon.com/about-aws/global-infrastructure/localzones/)\. 

A Local Zone is represented by an AWS Region code followed by an identifier that indicates the location, for example `us-west-2-lax-1a`\.

For a list of ElastiCache available regions and endpoints, see [Supported Regions & Endpoints](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/RegionsAndAZs.html)\.

**Note**  
 A Local Zone can't be included in a Multi\-AZ deployment\.

## To use a Local Zone<a name="Local_zones-using"></a>

1. Enable the Local Zone in the Amazon EC2 console\.

   For more information, see [Enabling Local Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#enable-zone-group) in the *Amazon EC2 User Guide for Linux Instances*\.

1. Create a subnet in the Local Zone\.

   For more information, see [Creating a subnet in your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#AddaSubnet) in the *Amazon VPC User Guide*\.

1. Create an ElastiCache subnet group in the Local Zone\.

   When you create an ElastiCache subnet group, choose the Availability Zone group for the Local Zone\.

   For more information, see [Creating a subnet group](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/SubnetGroups.Creating.html) in the *ElastiCache User Guide*\.

1. Create an ElastiCache for Memcached cluster that uses the ElastiCache subnet in the Local Zone\.

    For more information see: [Creating a Memcached Cluster \(Console\)](Clusters.Create.CON.Memcached.md)