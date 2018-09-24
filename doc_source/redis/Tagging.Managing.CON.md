# Managing Your Tags Using the ElastiCache Console<a name="Tagging.Managing.CON"></a>

You can use the Amazon ElastiCache console to add, modify, or remove cost allocation tags\.

The following procedure walks you through viewing, adding, modifying, or deleting one or more cost allocation tags using the ElastiCache management console\.

Redis clusters may have zero, one, or multiple shards\. Because tags are added to Redis nodes rather than the entire cluster, procedure for managing tags on Redis clusters is slightly different for each of the three configurations when using the AWS Management Console\. Use one of the two following procedures to manage tags on your Redis cluster\.

**Managing tags on a Redis cluster with zero shards using the AWS Management Console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. Choose **Redis**\.

1. Choose the name of the cluster with zero shards you want to add tags to\.

1. Chose the box to the left of the cluster's node name\.

1. From the **Actions** list, choose **Manage tags**, and then use the dialog box to manage your tags\.  
![\[Image: Manage Tags\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-TaggingDialog-Add.png)

1. For each tag you want to add, modify, or remove:

   **To add, modify, or remove tags**
   + **To add a tag:** In the **Key** column, type a key name in the box that displays *Add key* and an optional value in the box to the right of the key name\.
   + **To modify a tag:** In the **Value** column, type a new value or remove the existing value for the tag\.
   + **To remove a tag:** Choose the **X** to the right of the tag\.

1. When you're finished, choose **Apply Changes**\.

**Managing tags on a Redis cluster with one or more shards using the AWS Management Console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. Choose **Redis**\.

1. Choose the name of the sharded cluster's you want to add tags to\.

1. If this cluster has only one shard, skip to step 6\. If this cluster has multiple shards, continue at step 5\.

1. From the list of shards, choose the name of the shard which has the node you want to add tags to\.

1. Choose the box to the left of the node you want to add tags to\.

1. From the **Actions** list, choose **Manage tags**, and then use the dialog box to manage your tags\.  
![\[Image: Manage Tags\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-TaggingDialog-Add.png)

1. For each tag you want to add, modify, or remove:

   **To add, modify, or remove tags**
   + **To add a tag:** In the **Key** column, type a key name in the box that displays *Add key* and an optional value in the box to the right of the key name\.
   + **To modify a tag:** In the **Value** column, type a new value or remove the existing value for the tag\.
   + **To remove a tag:** Choose the **X** to the right of the tag\.

1. When you're finished, choose **Apply Changes**\.