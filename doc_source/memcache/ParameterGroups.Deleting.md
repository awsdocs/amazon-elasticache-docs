# Deleting a parameter group<a name="ParameterGroups.Deleting"></a>

You can delete a custom parameter group using the ElastiCache console, the AWS CLI, or the ElastiCache API\.

You cannot delete a parameter group if it is associated with any clusters\. Nor can you delete any of the default parameter groups\.

## Deleting a parameter group \(Console\)<a name="ParameterGroups.Deleting.CON"></a>

The following procedure shows how to delete a parameter group using the ElastiCache console\.

**To delete a parameter group using the ElastiCache console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. To see a list of all available parameter groups, in the left hand navigation pane choose **Parameter Groups**\.

1. Choose the parameter groups you want to delete by choosing the box to the left of the parameter group's name\.

   The **Delete** button will become active\.

1. Choose **Delete**\.

   The **Delete Parameter Groups** confirmation screen will appear\.

1. To delete the parameter groups, on the **Delete Parameter Groups** confirmation screen, choose **Delete**\.

   To keep the parameter groups, choose **Cancel**\.

## Deleting a parameter group \(AWS CLI\)<a name="ParameterGroups.Deleting.CLI"></a>

To delete a parameter group using the AWS CLI, use the command `delete-cache-parameter-group`\. For the parameter group to delete, the parameter group specified by `--cache-parameter-group-name` cannot have any clusters associated with it, nor can it be a default parameter group\.

The following sample code deletes the *myMem14* parameter group\.

**Example**  
For Linux, macOS, or Unix:  

```
aws elasticache delete-cache-parameter-group \
    --cache-parameter-group-name myMem14
```
For Windows:  

```
aws elasticache delete-cache-parameter-group ^
    --cache-parameter-group-name myMem14
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/elasticache/delete-cache-parameter-group.html](https://docs.aws.amazon.com/cli/latest/reference/elasticache/delete-cache-parameter-group.html)\.

## Deleting a parameter group \(ElastiCache API\)<a name="ParameterGroups.Deleting.API"></a>

To delete a parameter group using the ElastiCache API, use the `DeleteCacheParameterGroup` action\. For the parameter group to delete, the parameter group specified by `CacheParameterGroupName` cannot have any clusters associated with it, nor can it be a default parameter group\.

**Example**  
The following sample code deletes the *myMem14* parameter group\.  

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DeleteCacheParameterGroup
   &CacheParameterGroupName=myMem14
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &Version=2015-02-02
   &X-Amz-Credential=<credential>
```

For more information, see [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheParameterGroup.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteCacheParameterGroup.html)\.