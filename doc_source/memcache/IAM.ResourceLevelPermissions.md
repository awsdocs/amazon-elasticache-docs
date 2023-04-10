# Resource\-level permissions<a name="IAM.ResourceLevelPermissions"></a>

You can restrict the scope of permissions by specifying resources in an IAM policy\. Many Elasticache API actions support a resource type that varies depending on the behavior of the action\. Every IAM policy statement grants permission to an action that's performed on a resource\. When the action doesn't act on a named resource, or when you grant permission to perform the action on all resources, the value of the resource in the policy is a wildcard \(\*\)\. For many API actions, you can restrict the resources that a user can modify by specifying the Amazon Resource Name \(ARN\) of a resource, or an ARN pattern that matches multiple resources\. To restrict permissions by resource, specify the resource by ARN\.

**ElastiCache Resource ARN Format**

**Note**  
For resource\-level permissions to be effective, the resource name on the ARN string should be lower case\.
+ Cluster – arn:aws:elasticache:*us\-east\-2:123456789012*:cluster:my\-cluster
+ Parameter group – arn:aws:elasticache:*us\-east\-2:123456789012*:parametergroup:my\-parameter\-group
+ Security group – arn:aws:elasticache:*us\-east\-2:123456789012*:securitygroup:my\-security\-group
+ Subnet group – arn:aws:elasticache:*us\-east\-2:123456789012*:subnetgroup:my\-subnet\-group
+ Reserved instance – arn:aws:elasticache:*us\-east\-2:123456789012*:reservedinstance:my\-reserved\-instance

**Topics**
+ [Example 1: Allow a user full access to specific ElastiCache resource types](#example-allow-list-current-elasticache-resources-resource)

## Example 1: Allow a user full access to specific ElastiCache resource types<a name="example-allow-list-current-elasticache-resources-resource"></a>

The following policy explicitly allows all resources of type subnet group, security group and replication group\.

```
{
        "Sid": "Example1",
        "Effect": "Allow",
        "Action": "elasticache:*",
        "Resource": [
             "arn:aws:elasticache:us-east-1:account-id:subnetgroup:*",
             "arn:aws:elasticache:us-east-1:account-id:securitygroup:*""
        ]
}
```