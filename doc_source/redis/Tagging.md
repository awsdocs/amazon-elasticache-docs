# Monitoring costs with cost allocation tags<a name="Tagging"></a>

When you add cost allocation tags to your resources in Amazon ElastiCache, you can track costs by grouping expenses on your invoices by resource tag values\.

An ElastiCache cost allocation tag is a key\-value pair that you define and associate with an ElastiCache resource\. The key and value are case\-sensitive\. You can use a tag key to define a category, and the tag value can be an item in that category\. For example, you might define a tag key of `CostCenter` and a tag value of `10010`, indicating that the resource is assigned to the 10010 cost center\. You can also use tags to designate resources as being used for test or production by using a key such as `Environment` and values such as `test` or `production`\. We recommend that you use a consistent set of tag keys to make it easier to track costs associated with your resources\.

Use cost allocation tags to organize your AWS bill to reflect your own cost structure\. To do this, sign up to get your AWS account bill with tag key values included\.  Then, to see the cost of combined resources, organize your billing information according to resources with the same tag key values\. For example, you can tag several resources with a specific application name, and then organize your billing information to see the total cost of that application across several services\. 

You can also combine tags to track costs at a greater level of detail\. For example, to track your service costs by region you might use the tag keys `Service` and `Region`\. On one resource you might have the values `ElastiCache` and `Asia Pacific (Singapore)`, and on another resource the values `ElastiCache` and `Europe (Frankfurt)`\. You can then see your total ElastiCache costs broken out by region\. For more information, see [Use Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) in the *AWS Billing and Cost Management User Guide*\.

You can add ElastiCache cost allocation tags to Redis nodes\. When you add, list, modify, copy, or remove a tag, the operation is applied only to the specified node\.

**Characteristics of ElastiCache cost allocation tags**
+ Cost allocation tags are applied to ElastiCache resources which are specified in CLI and API operations as an ARN\. The resource\-type will be a "cluster"\.

  Sample ARN: `arn:aws:elasticache:<region>:<customer-id>:<resource-type>:<resource-name>`

  Sample arn: `arn:aws:elasticache:us-west-2:1234567890:cluster:my-cluster`
+ The tag key is the required name of the tag\. The key's string value can be from 1 to 128 Unicode characters long and cannot be prefixed with `aws:`\. The string can contain only the set of Unicode letters, digits, blank spaces, underscores \( \_ \), periods \( \. \), colons \( : \), backslashes \( \\ \), equal signs \( = \), plus signs \( \+ \), hyphens \( \- \), or at signs \( @ \)\.

   
+ The tag value is the optional value of the tag\. The value's string value can be from 1 to 256 Unicode characters in length and cannot be prefixed with `aws:`\. The string can contain only the set of Unicode letters, digits, blank spaces, underscores \( \_ \), periods \( \. \), colons \( : \), backslashes \( \\ \), equal signs \( = \), plus signs \( \+ \), hyphens \( \- \), or at signs \( @ \)\.

   
+ An ElastiCache resource can have a maximum of 50 tags\.

   
+ Values do not have to be unique in a tag set\. For example, you can have a tag set where the keys `Service` and `Application` both have the value `ElastiCache`\.

AWS does not apply any semantic meaning to your tags\. Tags are interpreted strictly as character strings\. AWS does not automatically set any tags on any ElastiCache resource\.

**Topics**
+ [Managing your cost allocation tags using the AWS CLI](Tagging.Managing.CLI.md)
+ [Managing your cost allocation tags using the ElastiCache API](Tagging.Managing.API.md)