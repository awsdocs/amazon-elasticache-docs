# Managing your cost allocation tags using the AWS CLI<a name="Tagging.Managing.CLI"></a>

You can use the AWS CLI to add, modify, or remove cost allocation tags\.

Sample arn: `arn:aws:elasticache:us-west-2:1234567890:cluster:my-cluster`

Cost allocation tags are applied to ElastiCache for Redis nodes\. The node to be tagged is specified using an ARN \(Amazon Resource Name\)\.

Sample arn: `arn:aws:elasticache:us-west-2:1234567890:cluster:my-cluster`

**Topics**
+ [Listing tags using the AWS CLI](#Tagging.Managing.CLI.List)
+ [Adding tags using the AWS CLI](#Tagging.Managing.CLI.Add)
+ [Modifying tags using the AWS CLI](#Tagging.Managing.CLI.Modify)
+ [Removing tags using the AWS CLI](#Tagging.Managing.CLI.Remove)

## Listing tags using the AWS CLI<a name="Tagging.Managing.CLI.List"></a>

You can use the AWS CLI to list tags on an existing ElastiCache resource by using the [list\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticache/list-tags-for-resource.html) operation\.

The following code uses the AWS CLI to list the tags on the Redis node `my-cluster-001` in the `my-cluster` cluster in region us\-west\-2\.

For Linux, macOS, or Unix:

```
aws elasticache list-tags-for-resource \
  --resource-name arn:aws:elasticache:us-west-2:0123456789:cluster:my-cluster-001
```

For Windows:

```
aws elasticache list-tags-for-resource ^
  --resource-name arn:aws:elasticache:us-west-2:0123456789:cluster:my-cluster-001
```

Output from this operation will look something like the following, a list of all the tags on the resource\.

```
{
   "TagList": [
      {
         "Value": "10110",
         "Key": "CostCenter"
      },
      {
         "Value": "EC2",
         "Key": "Service"
      }
   ]
}
```

If there are no tags on the resource, the output will be an empty TagList\.

```
{
   "TagList": []
}
```

For more information, see the AWS CLI for ElastiCache [list\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticache/list-tags-for-resource.html)\.

## Adding tags using the AWS CLI<a name="Tagging.Managing.CLI.Add"></a>

You can use the AWS CLI to add tags to an existing ElastiCache resource by using the [add\-tags\-to\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticache/add-tags-to-resource.html) CLI operation\. If the tag key does not exist on the resource, the key and value are added to the resource\. If the key already exists on the resource, the value associated with that key is updated to the new value\.

The following code uses the AWS CLI to add the keys `Service` and `Region` with the values `elasticache` and `us-west-2` respectively to the node `my-cluster-001` in the cluster `my-cluster` in region us\-west\-2\.

For Linux, macOS, or Unix:

```
aws elasticache add-tags-to-resource \
 --resource-name arn:aws:elasticache:us-west-2:0123456789:cluster:my-cluster-001 \
 --tags Key=Service,Value=elasticache \
        Key=Region,Value=us-west-2
```

For Windows:

```
aws elasticache add-tags-to-resource ^
 --resource-name arn:aws:elasticache:us-west-2:0123456789:cluster:my-cluster-001 ^
 --tags Key=Service,Value=elasticache ^
        Key=Region,Value=us-west-2
```

Output from this operation will look something like the following, a list of all the tags on the resource following the operation\.

```
{
   "TagList": [
      {
         "Value": "elasticache",
         "Key": "Service"
      },
      {
         "Value": "us-west-2",
         "Key": "Region"
      }
   ]
}
```

For more information, see the AWS CLI for ElastiCache [add\-tags\-to\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticache/add-tags-to-resource.html)\.

You can also use the AWS CLI to add tags to a cluster when you create a new cluster by using the operation [create\-cache\-cluster](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-cache-cluster.html)\. You cannot add tags when creating a cluster using the ElastiCache management console\. After the cluster is created, you can then use the console to add tags to the cluster\.

## Modifying tags using the AWS CLI<a name="Tagging.Managing.CLI.Modify"></a>

You can use the AWS CLI to modify the tags on a node in an ElastiCache for Redis cluster\.

To modify tags:
+ Use [add\-tags\-to\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticache/add-tags-to-resource.html) to either add a new tag and value or to change the value associated with an existing tag\.
+ Use [remove\-tags\-from\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticache/remove-tags-from-resource.html) to remove specified tags from the resource\.

Output from either operation will be a list of tags and their values on the specified cluster\.

## Removing tags using the AWS CLI<a name="Tagging.Managing.CLI.Remove"></a>

You can use the AWS CLI to remove tags from an existing node in an ElastiCache for Redis cluster by using the [remove\-tags\-from\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticache/remove-tags-from-resource.html) operation\.

The following code uses the AWS CLI to remove the tags with the keys `Service` and `Region` from the node `my-cluster-001` in  the cluster `my-cluster` in the us\-west\-2 region\.

For Linux, macOS, or Unix:

```
aws elasticache remove-tags-from-resource \
 --resource-name arn:aws:elasticache:us-west-2:0123456789:cluster:my-cluster-001 \
 --tag-keys PM Service
```

For Windows:

```
aws elasticache remove-tags-from-resource ^
 --resource-name arn:aws:elasticache:us-west-2:0123456789:cluster:my-cluster-001 ^
 --tag-keys PM Service
```

Output from this operation will look something like the following, a list of all the tags on the resource following the operation\.

```
{
   "TagList": []
}
```

For more information, see the AWS CLI for ElastiCache [remove\-tags\-from\-resource](https://docs.aws.amazon.com/cli/latest/reference/elasticache/remove-tags-from-resource.html)\.