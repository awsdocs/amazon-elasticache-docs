# Creating a Parameter Group<a name="ParameterGroups.Creating"></a>

You need to create a new parameter group if there is one or more parameter values that you want changed from the default values\. You can create a parameter group using the ElastiCache console, the AWS CLI, or the ElastiCache API\.

## Creating a Parameter Group \(Console\)<a name="ParameterGroups.Creating.CON"></a>

The following procedure shows how to create a parameter group using the ElastiCache console\.

**To create a parameter group using the ElastiCache console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. To see a list of all available parameter groups, in the left hand navigation pane choose **Parameter Groups**\.

1. To create a parameter group, choose **Create Parameter Group**\.

   The **Create Parameter Group** screen appears\.

1. From the **Family** list, choose the parameter group family that will be the template for your parameter group\.

   The parameter group family, such as *memcached1\.4* , defines the actual parameters in your parameter group and their initial values\. The parameter group family must coincide with the cluster's engine and version\.

1. In the **Name** box, type in a unique name for this parameter group\.

   When creating a cluster or modifying a cluster's parameter group, you will choose the parameter group by its name\. Therefore, we recommend that the name be informative and somehow identify the parameter group's family\.

   Parameter group naming constraints are as follows:
   + Must begin with an ASCII letter\.
   + Can only contain ASCII letters, digits, and hyphens\.
   + Must be 1–255 characters long\.
   + Can't contain two consecutive hyphens\.
   + Can't end with a hyphen\.

1. In the **Description** box, type in a description for the parameter group\.

1. To create the parameter group, choose **Create**\.

   To terminate the process without creating the parameter group, choose **Cancel**\.

1. When the parameter group is created, it will have the family's default values\. To change the default values you must modify the parameter group\. For more information, see [Modifying a Parameter Group](ParameterGroups.Modifying.md)\.

## Creating a Parameter Group \(AWS CLI\)<a name="ParameterGroups.Creating.CLI"></a>

To create a parameter group using the AWS CLI, use the command `create-cache-parameter-group` with these parameters\.
+ `--cache-parameter-group-name` — The name of the parameter group\.

  Parameter group naming constraints are as follows:
  + Must begin with an ASCII letter\.
  + Can only contain ASCII letters, digits, and hyphens\.
  + Must be 1–255 characters long\.
  + Can't contain two consecutive hyphens\.
  + Can't end with a hyphen\.
+ `--cache-parameter-group-family` — The engine and version family for the parameter group\.
+ `--description` — A user supplied description for the parameter group\.

**Example**  
The following example creates a parameter group named *myMem14* using the memcached1\.4 family as the template\.   
For Linux, macOS, or Unix:  

```
aws elasticache create-cache-parameter-group \
    --cache-parameter-group-name myMem14  \
    --cache-parameter-group-family memcached1.4 \
    --description "My first parameter group"
```
For Windows:  

```
aws elasticache create-cache-parameter-group ^
    --cache-parameter-group-name myMem14  ^
    --cache-parameter-group-family memcached1.4 ^
    --description "My first parameter group"
```
The output from this command should look something like this\.  

```
{
    "CacheParameterGroup": {
        "CacheParameterGroupName": "myMem14", 
        "CacheParameterGroupFamily": "memcached1.4", 
        "Description": "My first  parameter group"
    }
}
```

When the parameter group is created, it will have the family's default values\. To change the default values you must modify the parameter group\. For more information, see [Modifying a Parameter Group](ParameterGroups.Modifying.md)\.

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-cache-parameter-group.html](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-cache-parameter-group.html)\.

## Creating a Parameter Group \(ElastiCache API\)<a name="ParameterGroups.Creating.API"></a>

To create a parameter group using the ElastiCache API, use the `CreateCacheParameterGroup` action with these parameters\.
+ `ParameterGroupName` — The name of the parameter group\.

  Parameter group naming constraints are as follows:
  + Must begin with an ASCII letter\.
  + Can only contain ASCII letters, digits, and hyphens\.
  + Must be 1–255 characters long\.
  + Can't contain two consecutive hyphens\.
  + Can't end with a hyphen\.
+ `CacheParameterGroupFamily` — The engine and version family for the parameter group\. For example, `memcached1.4`\.
+ `Description` — A user supplied description for the parameter group\.

**Example**  
The following example creates a parameter group named *myMem14* using the memcached1\.4 family as the template\.   

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=CreateCacheParameterGroup
   &CacheParameterGroupFamily=memcached1.4
   &CacheParameterGroupName=myMem14
   &Description=My%20first%20parameter%20group
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &Version=2015-02-02
   &X-Amz-Credential=<credential>
```
The response from this action should look something like this\.  

```
<CreateCacheParameterGroupResponse xmlns="http://elasticache.amazonaws.com/doc/2013-06-15/">
  <CreateCacheParameterGroupResult>
    <CacheParameterGroup>
      <CacheParameterGroupName>myMem14</CacheParameterGroupName>
      <CacheParameterGroupFamily>memcached1.4</CacheParameterGroupFamily>
      <Description>My first  parameter group</Description>
    </CacheParameterGroup>
  </CreateCacheParameterGroupResult>
  <ResponseMetadata>
    <RequestId>d8465952-af48-11e0-8d36-859edca6f4b8</RequestId>
  </ResponseMetadata>
</CreateCacheParameterGroupResponse>
```

When the parameter group is created, it will have the family's default values\. To change the default values you must modify the parameter group\. For more information, see [Modifying a Parameter Group](ParameterGroups.Modifying.md)\.

For more information, see [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheParameterGroup.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateCacheParameterGroup.html)\.