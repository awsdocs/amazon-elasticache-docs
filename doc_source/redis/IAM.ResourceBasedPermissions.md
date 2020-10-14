# Resource\-level permissions<a name="IAM.ResourceBasedPermissions"></a>

You can restrict the scope of a user's permissions by specifying resources in an IAM policy\. Many Elasticache API actions support a resource type that varies depending on the behavior of the action\. Every IAM policy statement grants permission to an action that's performed on a resource\. When the action doesn't act on a named resource, or when you grant permission to perform the action on all resources, the value of the resource in the policy is a wildcard \(\*\)\. For many API actions, you can restrict the resources that a user can modify by specifying the Amazon Resource Name \(ARN\) of a resource, or an ARN pattern that matches multiple resources\. To restrict permissions by resource, specify the resource by ARN\.

**ElastiCache Resource ARN Format**
+ Cluster – arn:aws:elasticache:*us\-east\-2:123456789012*:cluster:my\-cluster
+ Snapshot – arn:aws:elasticache:*us\-east\-2:123456789012*:cluster:my\-snapshot
+ Parameter group – arn:aws:elasticache:*us\-east\-2:123456789012*:cluster:my\-parameter\-group
+ Replication group – arn:aws:elasticache:*us\-east\-2:123456789012*:cluster:my\-replication\-group
+ Security group – arn:aws:elasticache:*us\-east\-2:123456789012*:cluster:my\-security\-group
+ Subnet group – arn:aws:elasticache:*us\-east\-2:123456789012*:cluster:my\-subnet\-group
+ Reserved instance – arn:aws:elasticache:*us\-east\-2:123456789012*:cluster:my\-reserved\-instance
+ Global replication group – arn:aws:elasticache:*123456789012*:cluster:my\-global\-replication\-group 

**Topics**
+ [Example 1: Allow a User Full Access to Specific ElastiCache Resource Types](#example-allow-list-current-elasticache-resources-resource)
+ [Example 2: Deny a User Access to a Replication Group\.](#example-allow-specific-elasticache-actions-resource)

## Example 1: Allow a User Full Access to Specific ElastiCache Resource Types<a name="example-allow-list-current-elasticache-resources-resource"></a>

The following policy explicitly allow all resources of type subnet group, security group and replication group\.

```
{
        "Sid": "Example1",
        "Effect": "Allow",
        "Action": "elasticache:*",
        "Resource": [
             "arn:aws:elasticache:us-east-1:account-id:subnetgroup:*",
             "arn:aws:elasticache:us-east-1:account-id:securitygroup:*",
             "arn:aws:elasticache:us-east-1:account-id:replicationgroup:*"
        ]
}
```

## Example 2: Deny a User Access to a Replication Group\.<a name="example-allow-specific-elasticache-actions-resource"></a>

The following example explicitly denies access to a particular replication group\.

```
{
        "Sid": "Example2",
        "Effect": "Deny",
        "Action": "elasticache:*",
        "Resource": [
                "arn:aws:elasticache:us-east-1:account-id:replicationgroup:name"
        ]
}
```