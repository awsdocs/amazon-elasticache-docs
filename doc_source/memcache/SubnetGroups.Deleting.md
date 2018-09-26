# Deleting a Subnet Group<a name="SubnetGroups.Deleting"></a>

If you decide that you no longer need your subnet group, you can delete it\. You cannot delete a subnet group if it is currently in use by a cluster\.

The following procedures show you how to delete a subnet group\.

## Deleting a Subnet Group \(Console\)<a name="SubnetGroups.Deleting.CON"></a>

**To delete a subnet group**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Subnet Groups**\.

1. In the list of subnet groups, choose the one you want to delete and then choose **Delete**\.

1. When you are asked to confirm this operation, choose **Yes, Delete**\.

## Deleting a Subnet Group \(AWS CLI\)<a name="SubnetGroups.Deleting.CLI"></a>

Using the AWS CLI, call the command delete\-cache\-subnet\-group with the following parameter:
+ `--cache-subnet-group-name` *mysubnetgroup*

For Linux, macOS, or Unix:

```
aws elasticache delete-cache-subnet-group \
    --cache-subnet-group-name mysubnetgroup
```

For Windows:

```
aws elasticache delete-cache-subnet-group ^
    --cache-subnet-group-name mysubnetgroup
```

This command produces no output\.

For more information, see the AWS CLI topic [delete\-cache\-subnet\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/delete-cache-subnet-group.html)\.

## Deleting a Subnet Group \(ElastiCache API\)<a name="SubnetGroups.Deleting.API"></a>

Using the ElastiCache API, call `DeleteCacheSubnetGroup` with the following parameter:
+ `CacheSubnetGroupName=mysubnetgroup`

**Example**  
Line breaks are added for ease of reading\.  

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=DeleteCacheSubnetGroup
    &CacheSubnetGroupName=mysubnetgroup
    &SignatureMethod=HmacSHA256
    &SignatureVersion=4
    &Timestamp=20141201T220302Z
    &Version=2014-12-01
    &X-Amz-Algorithm=AWS4-HMAC-SHA256
    &X-Amz-Credential=<credential>
    &X-Amz-Date=20141201T220302Z
    &X-Amz-Expires=20141201T220302Z
    &X-Amz-Signature=<signature>
    &X-Amz-SignedHeaders=Host
```

This command produces no output\.

For more information, see the ElastiCache API topic [DeleteCacheSubnetGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheSubnetGroup.html)\.