# Creating a Security Group<a name="SecurityGroups.Creating"></a>

This topic is relevant to you only if you are not running in an Amazon VPC\. If you are running in an Amazon VPC, see [Amazon VPCs and ElastiCache Security](VPCs.md)\.

To create a security group, you need to provide a name and a description\.

The following procedures show you how to create a new security group\.

## Creating a Security Group \(Console\)<a name="SecurityGroups.Creating.CON"></a>

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Security Groups**\.

1. Choose **Create Security Group**\.

1. In **Create Security Group**, type the name of the new security group in **Security Group**\.

1. In **Description**, type a description for the new security group\. 

1. Choose **Create**\. 

## Creating a Security Group \(AWS CLI\)<a name="SecurityGroups.Creating.CLI"></a>

At a command prompt, use the `create-cache-security-group` command with the following parameters:
+ \-\-cache\-security\-group\-name – The name of the security group you are creating\.

  **Example:** mysecuritygroup
+ \-\-description – A description for this security group\.

  **Example:** "My new security group"

For Linux, macOS, or Unix:

```
aws elasticache create-cache-security-group \
    --cache-security-group-name mysecuritygroup \
    --description "My new security group"
```

For Windows:

```
aws elasticache create-cache-security-group ^
    --cache-security-group-name mysecuritygroup ^
    --description "My new security group"
```

For more information, see [create\-cache\-security\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-cache-security-group.html)\.

## Creating a Security Group \(ElastiCache API\)<a name="SecurityGroups.Creating.API"></a>

Using the ElastiCache API operation `CreateCacheSecurityGroup` with the following parameters:
+ `CacheSecurityGroupName` – The name of the security group you are creating\.

  **Example:** `mysecuritygroup`
+ `Description` – A URL encoded description for this security group\.

  **Example:** `My%20security%20group`

**Example**  
Line breaks are added for ease of reading\.  

```
https://elasticache.us-west-2.amazonaws.com /
    ?Action=CreateCacheSecurityGroup
    &CacheSecurityGroupName=mysecuritygroup
    &Description=My%20security%20group   
    &Version=2015-02-02   
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20150202T220302Z
    &X-Amz-Algorithm=AWS4-HMAC-SHA256
    &X-Amz-Date=20150202T220302Z
    &X-Amz-SignedHeaders=Host
    &X-Amz-Expires=20150202T220302Z
    &X-Amz-Credential=<credential>
    &X-Amz-Signature=<signature>
```