# Managing Your Tags Using the ElastiCache Console<a name="Tagging.Managing.CON"></a>

You can use the Amazon ElastiCache console to add, modify, or remove cost allocation tags\.


+ [Managing Tags on a Memcached Cluster \(Console\)](#Tagging.Managing.CON.Mem)
+ [Managing Tags on a Redis \(cluster mode disabled\) Cluster \(Console\)](#Tagging.Managing.CON.Redis)
+ [Managing Tags on a Redis \(cluster mode enabled\) Cluster \(Console\)](#Tagging.Managing.CON.RedisCluster)
+ [Managing Tags on a Backup \(Console\)](#Tagging.Managing.CON.Backup)

## Managing Tags on a Memcached Cluster \(Console\)<a name="Tagging.Managing.CON.Mem"></a>

The following procedure walks you through viewing, adding, modifying, or deleting one or more cost allocation tags on a Memcached cluster using the ElastiCache management console\.

**To add, modify, or remove a tag on a Memcached cluster using the ElastiCache management console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. Choose **Memcached**\.

1. Choose the box to the left of the cluster's name you want to add tags to\.

   After you choose the cluster, you can see the tag names and values currently on this resource at the bottom of the details area\.

1. Choose **Manage Tags**, and then use the dialog box to manage your tags\.  
![\[Image: Manage Tags\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-TaggingDialog-Add.png)

1. For each tag you want to add, modify, or remove:

   **To add, modify, or remove tags**

   + **To add a tag:** In the **Key** column, type a key name in the box that displays *Add key* and an optional value in the box to the right of the key name\.

   + **To modify a tag:** In the **Value** column, type a new value or remove the existing value for the tag\.

   + **To remove a tag:** Choose the **X** to the right of the tag\.

1. When you're finished, choose **Apply Changes**\.

## Managing Tags on a Redis \(cluster mode disabled\) Cluster \(Console\)<a name="Tagging.Managing.CON.Redis"></a>

The following procedure walks you through viewing, adding, modifying, or deleting one or more cost allocation tags on the nodes in a Redis \(cluster mode disabled\) cluster using the ElastiCache management console\.

**To add, modify, or remove a tag on a Redis \(cluster mode disabled\) node using the ElastiCache management console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. Choose **Redis**\.

1. Choose the name of the cluster on which you want to add, modify, or remove tags\.

1. For each node in the cluster you want to view, add, modify, or remove tags on, do the following:

   1. Choose the box to the left of the node's name\.

   1. Choose **Actions**, then choose **Manage tags**\.  
![\[Image: Manage Tags dialog box\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-TaggingDialog-Add.png)

   1. For each tag you want to add, modify, or remove:

**To add, modify, or remove tags on a node**

      + **To add a tag:** In the **Key** column, type a key name in the box that displays *Add key*\. To add an optional value, tab to the *Empty value* box and type a value for the key\.

      + **To modify a tag:** In the **Value** column to the right of the key name, type a new value or remove the existing value for the tag\.

      + **To remove a tag:** Choose the **X** to the right of the tag\.

   1. When you're finished, choose **Apply Changes**\.

## Managing Tags on a Redis \(cluster mode enabled\) Cluster \(Console\)<a name="Tagging.Managing.CON.RedisCluster"></a>

The following procedure walks you through viewing, adding, modifying, or deleting one or more cost allocation tags on the nodes in a Redis \(cluster mode enabled\) cluster using the ElastiCache management console\.

**To add a tag to a Redis \(cluster mode enabled\) node using the ElastiCache management console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. Choose **Redis**\.

1. Choose the name of the cluster on which you want to add, modify, or remove tags\.

1. Choose the name of the shard on which you want to add, modify, or remove tags\.

1. For each node in the shard you want to view, add, modify, or remove tags on, do the following:

   1. Choose the box to the left of the node's name\.

   1. Choose **Actions**, then choose **Manage tags**\.  
![\[Image: Manage Tags dialog box\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-TaggingDialog-Add.png)

   1. For each tag you want to add, modify, or remove:

**To add, modify, or remove tags on a node**

      + **To add a tag:** In the **Key** column, type a key name in the box that displays *Add key*\. To add an optional value, tab to the *Empty value* box and type a value for the key\.

      + **To modify a tag:** In the **Value** column to the right of the key name, type a new value or remove the existing value for the tag\.

      + **To remove a tag:** Choose the **X** to the right of the tag\.

   1. When you're finished, choose **Apply Changes**\.

## Managing Tags on a Backup \(Console\)<a name="Tagging.Managing.CON.Backup"></a>

The following procedure walks you through viewing, adding, modifying, or deleting one or more cost allocation tags on a Redis backup using the ElastiCache management console\.

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. Choose **Backups**\.

1. Choose the box to the left of the backup's name you want to add tags to\.

   After you choose the cluster, you can see the tag names and values currently on this resource at the bottom of the details area\.

1. Choose **Manage Tags**, and then use the dialog box to manage your tags\.  
![\[Image: Manage Tags\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-TaggingDialog-Add.png)

1. For each tag you want to add, modify, or remove:

   **To add, modify, or remove tags**

   + **To add a tag:** In the **Key** column, type a key name in the box that displays *Add key* and an optional value in the box to the right of the key name\.

   + **To modify a tag:** In the **Value** column, type a new value or remove the existing value for the tag\.

   + **To remove a tag:** choose the **X** to the right of the tag\.

1. When you're finished, choose **Apply Changes**\.