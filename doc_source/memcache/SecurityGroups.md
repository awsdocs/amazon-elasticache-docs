# Security Groups: EC2\-Classic<a name="SecurityGroups"></a>

**Important**  
Amazon ElastiCache security groups are only applicable to clusters that are *not* running in an Amazon Virtual Private Cloud environment \(VPC\)\. If you are running in an Amazon Virtual Private Cloud, ** Security Groups** is not available in the console navigation pane\.  
If you are running your ElastiCache nodes in an Amazon VPC, you control access to your clusters with Amazon VPC security groups, which are different from ElastiCache security groups\. For more information about using ElastiCache in an Amazon VPC, see [Amazon VPCs and ElastiCache Security](VPCs.md)

Amazon ElastiCache allows you to control access to your clusters using ElastiCache security groups\. An ElastiCache security group acts like a firewall, controlling network access to your cluster\. By default, network access is turned off to your clusters\. If you want your applications to access your cluster, you must explicitly enable access from hosts in specific Amazon EC2 security groups\. Once ingress rules are configured, the same rules apply to all clusters associated with that security group\.

To allow network access to your cluster, create a security group and use the `AuthorizeCacheSecurityGroupIngress` API operation \(CLI: `authorize-cache-security-group-ingress`\) to authorize the desired Amazon EC2 security group \(which in turn specifies the Amazon EC2 instances allowed\)\. The security group can be associated with your cluster at the time of creation, or using the `ModifyCacheCluster` API operation \(CLI: `modify-cache-cluster`\)\.

**Important**  
Access control based on IP range is currently not enabled at the individual cluster level\. All clients to a cluster must be within the EC2 network, and authorized via security groups as described previously\.

For more information about using ElastiCache with Amazon VPCs, see [Amazon VPCs and ElastiCache Security](VPCs.md)\.

Note that Amazon EC2 instances running in an Amazon VPC can't connect to ElastiCache clusters in EC2\-Classic\.

**Topics**
+ [Creating a Security Group](SecurityGroups.Creating.md)
+ [Listing Available Security Groups](SecurityGroups.Listing.md)
+ [Authorizing Network Access to an Amazon EC2 Security Group](SecurityGroups.AuthorizingEC2.md)