# Subnets and Subnet Groups<a name="SubnetGroups"></a>

A *subnet group* is a collection of subnets \(typically private\) that you can designate for your clusters running in an Amazon Virtual Private Cloud \(VPC\) environment\.

If you create a cluster in an Amazon VPC, you must specify a subnet group\. ElastiCache uses that subnet group to choose a subnet and IP addresses within that subnet to associate with your nodes\.

This section covers how to create and leverage subnets and subnet groups to manage access to your ElastiCache resources\. 

For more information about subnet group usage in an Amazon VPC environment, see [Step 2: Authorize Access](GettingStarted.AuthorizeAccess.md)\.

**Topics**
+ [Creating a Subnet Group](SubnetGroups.Creating.md)
+ [Assigning a Subnet Group to a Cluster or Replication Group](SubnetGroups.Assigning.md)
+ [Modifying a Subnet Group](SubnetGroups.Modifying.md)
+ [Deleting a Subnet Group](SubnetGroups.Deleting.md)