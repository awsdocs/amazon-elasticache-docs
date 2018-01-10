# Accessing Amazon ElastiCache<a name="WhatIs.Accessing"></a>

Your Amazon ElastiCache instances can only be accessed through an Amazon EC2 instance\.

If you launched your ElastiCache instance in an Amazon Virtual Private Cloud \(Amazon VPC\), you can access your ElastiCache instance from an Amazon EC2 instance in the same Amazon VPC or, by using VPC peering, from an Amazon EC2 in a different Amazon VPC\.

If you launched your ElastiCache instance in EC2 Classic, you allow the EC2 instance to access your cluster by granting the Amazon EC2 security group associated with the instance access to your cache security group\. By default, access to a cluster is restricted to the account that launched the cluster\.

For more information on granting Amazon EC2 access to your cluster, see [Step 4: Authorize Access](GettingStarted.AuthorizeAccess.md) and [Accessing ElastiCache Resources from Outside AWS](Access.Outside.md)\.