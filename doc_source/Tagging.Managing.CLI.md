# Managing Your Cost Allocation Tags Using the AWS CLI<a name="Tagging.Managing.CLI"></a>

You can use the AWS CLI to add, modify, or remove cost allocation tags\.

Cost allocation tags are applied to ElastiCache resources\. What that resource is and how it is specified in an ARN depends on the engine and structure of the cluster\.

+ **Memcached**: Tags are applied to clusters\.

  Sample arn: `arn:aws:elasticache:us-west-2:1234567890:cluster:mymemcached`

+ **Redis**: Tags are applied to individual nodes\. Because of this, nodes in Redis clusters with replication can have different tags\.

  Sample arns

  + Redis \(cluster mode disabled\) no replication:

    Sample arn: `arn:aws:elasticache:us-west-2:1234567890:cluster:myredis`

  + Redis \(cluster mode disabled\) with replication:

    Sample arn: `arn:aws:elasticache:us-west-2:1234567890:cluster:myredis-001`

  + Redis \(cluster mode enabled\):

    Sample arn: `arn:aws:elasticache:us-west-2:1234567890:cluster:myredis-0001-001`

+ **Backups \(Redis\)**: Tags are applied to the backup\.

  Sample arn: `arn:aws:elasticache:us-west-2:1234567890:snapshot:myredisbackup`


+ [Listing Tags Using the AWS CLI](#Tagging.Managing.CLI.List)
+ [Adding Tags Using the AWS CLI](#Tagging.Managing.CLI.Add)
+ [Modifying Tags Using the AWS CLI](#Tagging.Managing.CLI.Modify)
+ [Removing Tags Using the AWS CLI](#Tagging.Managing.CLI.Remove)

## Listing Tags Using the AWS CLI<a name="Tagging.Managing.CLI.List"></a>

You can use the AWS CLI to list tags on an existing ElastiCache resource by using the [list\-tags\-for\-resource](http://docs.aws.amazon.com/cli/latest/reference/elasticache/list-tags-for-resource.html) operation\.

The following code uses the AWS CLI to list the tags on the Memcached cluster `myCluster` in the us\-west\-2 region\.

For Linux, macOS, or Unix:

```
aws elasticache list-tags-for-resource \
  --resource-name arn:aws:elasticache:us-west-2:0123456789:cluster:myCluster
```

For Windows:

```
aws elasticache list-tags-for-resource ^
  --resource-name arn:aws:elasticache:us-west-2:0123456789:cluster:myCluster
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

For more information, see the AWS CLI for ElastiCache [list\-tags\-for\-resource](http://docs.aws.amazon.com/cli/latest/reference/elasticache/list-tags-for-resource.html)\.

## Adding Tags Using the AWS CLI<a name="Tagging.Managing.CLI.Add"></a>

You can use the AWS CLI to add tags to an existing ElastiCache resource by using the [add\-tags\-to\-resource](http://docs.aws.amazon.com/cli/latest/reference/elasticache/add-tags-to-resource.html) CLI operation\. If the tag key does not exist on the resource, the key and value are added to the resource\. If the key already exists on the resource, the value associated with that key is updated to the new value\.

The following code uses the AWS CLI to add the keys `Service` and `Region` with the values `elasticache` and `us-west-2` respectively to the resource `myCluster` in the us\-west\-2 region\.

For Linux, macOS, or Unix:

```
aws elasticache add-tags-to-resource \
 --resource-name arn:aws:elasticache:us-west-2:0123456789:cluster:memcluster \
 --tags Key=Service,Value=elasticache \
        Key=Region,Value=us-west-2
```

For Windows:

```
aws elasticache add-tags-to-resource ^
 --resource-name arn:aws:elasticache:us-west-2:0123456789:cluster:memcluster ^
 --tags Key=PM ^
        Key=Region,Value=us-west-2
```

Output from this operation will look something like the following, a list of all the tags on the resource following the operation\.

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
      },
      {
         "Value": "",
         "Key": "PM"
      },
      {
         "Value": "us-west-2",
         "Key": "Region"
      }
   ]
}
```

For more information, see the AWS CLI for ElastiCache [add\-tags\-to\-resource](http://docs.aws.amazon.com/cli/latest/reference/elasticache/add-tags-to-resource.html)\.

You can also use the AWS CLI to add tags to a cluster when you create a new cluster by using the operation [create\-cache\-cluster](http://docs.aws.amazon.com/cli/latest/reference/elasticache/create-cache-cluster.html), or when you create a new replication group by using the operation [create\-replication\-group](http://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)\. Note that you cannot add tags during resource creation with the ElastiCache management console\. After the cluster or replication group is created, you can then use the console to add tags to the resource\.

## Modifying Tags Using the AWS CLI<a name="Tagging.Managing.CLI.Modify"></a>

You can use the AWS CLI to modify the tags on an ElastiCache resource\.

To modify the value of a tag:

+ Use [add\-tags\-to\-resource](http://docs.aws.amazon.com/cli/latest/reference/elasticache/add-tags-to-resource.html) to either add a new tag and value or to change the value associated with an existing tag\.

+ Use [remove\-tags\-from\-resource](http://docs.aws.amazon.com/cli/latest/reference/elasticache/remove-tags-from-resource.html) to remove specified tags from the resource\.

Output from either operation will be a list of tags and their values on the specified resource\.

## Removing Tags Using the AWS CLI<a name="Tagging.Managing.CLI.Remove"></a>

You can use the AWS CLI to remove tags from an existing ElastiCache resource by using the [remove\-tags\-from\-resource](http://docs.aws.amazon.com/cli/latest/reference/elasticache/remove-tags-from-resource.html) operation\.

The following code uses the AWS CLI to remove the tags with the keys `Service` and `Region` from the resource `myCluster` in the us\-west\-2 region\.

For Linux, macOS, or Unix:

```
aws elasticache remove-tags-from-resource \
 --resource-name arn:aws:elasticache:us-west-2:0123456789:cluster:myCluster \
 --tag-keys PM Service
```

For Windows:

```
aws elasticache remove-tags-from-resource ^
 --resource-name arn:aws:elasticache:us-west-2:0123456789:cluster:myCluster ^
 --tag-keys PM Service
```

Output from this operation will look something like the following, a list of all the tags on the resource following the operation\.

```
{
   "TagList": [
      {
         "Value": "10110",
         "Key": "CostCenter"
      },
      {
         "Value": "us-west-2",
         "Key": "Region"
      }
   ]
}
```

For more information, see the AWS CLI for ElastiCache [remove\-tags\-from\-resource](http://docs.aws.amazon.com/cli/latest/reference/elasticache/remove-tags-from-resource.html)\.