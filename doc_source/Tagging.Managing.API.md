# Managing Your Cost Allocation Tags Using the ElastiCache API<a name="Tagging.Managing.API"></a>

You can use the ElastiCache API to add, modify, or remove cost allocation tags\.

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


+ [Listing Tags Using the ElastiCache API](#Tagging.Managing.API.List)
+ [Adding Tags Using the ElastiCache API](#Tagging.Managing.API.Add)
+ [Modifying Tags Using the ElastiCache API](#Tagging.Managing.API.Modify)
+ [Removing Tags Using the ElastiCache API](#Tagging.Managing.API.Remove)

## Listing Tags Using the ElastiCache API<a name="Tagging.Managing.API.List"></a>

You can use the ElastiCache API to list tags on an existing resource by using the [ListTagsForResource](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ListTagsForResource.html) operation\.

The following code uses the ElastiCache API to list the tags on the resource `myCluster` in the us\-west\-2 region\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=ListTagsForResource
   &ResourceName=arn:aws:elasticache:us-west-2:0123456789:cluster:myCluster
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Version=2015-02-02
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```

## Adding Tags Using the ElastiCache API<a name="Tagging.Managing.API.Add"></a>

You can use the ElastiCache API to add tags to an existing ElastiCache resource by using the [AddTagsToResource](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_AddTagsToResource.html) operation\. If the tag key does not exist on the resource, the key and value are added to the resource\. If the key already exists on the resource, the value associated with that key is updated to the new value\.

The following code uses the ElastiCache API to add the keys `Service` and `Region` with the values `elasticache` and `us-west-2` respectively to the resource `myCluster` in the us\-west\-2 region\. 

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=AddTagsToResource
   &ResourceName=arn:aws:elasticache:us-west-2:0123456789:cluster:memclusterr
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Tags.member.1.Key=Service 
   &Tags.member.1.Value=elasticache
   &Tags.member.2.Key=Region
   &Tags.member.2.Value=us-west-2
   &Version=2015-02-02
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```

For more information, see [AddTagsToResource](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_AddTagsToResource.html) in the *Amazon ElastiCache API Reference*\.

## Modifying Tags Using the ElastiCache API<a name="Tagging.Managing.API.Modify"></a>

You can use the ElastiCache API to modify the tags on an ElastiCache resource\.

To modify the value of a tag:

+ Use [AddTagsToResource](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_AddTagsToResource.html) operation to either add a new tag and value or to change the value of an existing tag\.

+ Use [RemoveTagsFromResource](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RemoveTagsFromResource.html) to remove tags from the resource\.

Output from either operation will be a list of tags and their values on the specified resource\.

Use [RemoveTagsFromResource](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RemoveTagsFromResource.html) to remove tags from the resource\.

## Removing Tags Using the ElastiCache API<a name="Tagging.Managing.API.Remove"></a>

You can use the ElastiCache API to remove tags from an existing ElastiCache resource by using the [RemoveTagsFromResource](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RemoveTagsFromResource.html) operation\.

The following code uses the ElastiCache API to remove the tags with the keys `Service` and `Region` from the resource `myCluster` in the us\-west\-2 region\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=RemoveTagsFromResource
   &ResourceName=arn:aws:elasticache:us-west-2:0123456789:cluster:myCluster
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &TagKeys.member.1=Service
   &TagKeys.member.2=Region
   &Version=2015-02-02
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```