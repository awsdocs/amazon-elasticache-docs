# Subnets and subnet groups<a name="SubnetGroups"></a>

A *subnet group* is a collection of subnets \(typically private\) that you can designate for your clusters running in an Amazon Virtual Private Cloud \(VPC\) environment\.

If you create a cluster in an Amazon VPC, you must specify a subnet group\. ElastiCache uses that subnet group to choose a subnet and IP addresses within that subnet to associate with your nodes\.

This section covers how to create and leverage subnets and subnet groups to manage access to your ElastiCache resources\. 

For more information about subnet group usage in an Amazon VPC environment, see [Authorize access](GettingStarted.AuthorizeAccess.md)\.

**Topics**
+ [Creating a subnet group](SubnetGroups.Creating.md)
+ [Assigning a subnet group to a cluster](SubnetGroups.Assigning.md)
+ [Modifying a subnet group](SubnetGroups.Modifying.md)
+ [Deleting a subnet group](SubnetGroups.Deleting.md)