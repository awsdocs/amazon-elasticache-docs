# Viewing a Security Group<a name="SecurityGroups.Viewing"></a>

This topic is relevant to you only if you are not running in an Amazon VPC\. If you are running in an Amazon VPC, see [Amazon Virtual Private Cloud \(Amazon VPC\) with ElastiCache](AmazonVPC.md)\.

You can view detailed information about your security group\.

The following procedures show you how to view the properties of a security group using the ElastiCache console, AWS CLI, and ElastiCache API\.

## Viewing a Security Group \(Console\)<a name="SecurityGroups.Viewing.CON"></a>

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Security Groups**\.

   The available cache security groups appear in the **Security Groups** list\.

1. Choose a cache security group from the ** Security Groups** list\.

   The list of authorizations defined for the security group appears in the detail section at the bottom of the window\.

## Viewing a Security Group \(AWS CLI\)<a name="SecurityGroups.Viewing.CLI"></a>

At the command prompt, use the AWS CLI `describe-cache-security-groups` command with the name of the security group you want to view\.

+ `--cache-security-group-name` – the name of the security group to return details for\.

```
aws elasticache describe-cache-security-groups --cache-security-group-name mysecuritygroup
```

JSON output from this command will look something like this\.

```
{
    "CacheSecurityGroup": {
        "OwnerId": "OwnerId",
        "CacheSecurityGroupName": "CacheSecurityGroupName",
        "Description": "Description",
        "EC2SecurityGroups": [
            {
                "Status": "Status",
                "EC2SecurityGroupName": "EC2SecurityGroupName",
                "EC2SecurityGroupOwnerId": "EC2SecurityGroupOwnerId"
            }
        ]
    }
}
```

For more information, see [describe\-cache\-security\-groups](http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-security-groups.html)\.

## Viewing a Security Group \(ElastiCache API\)<a name="SecurityGroups.Viewing.API"></a>

Using the ElastiCache API, call `DescribeCacheSecurityGroups` with the name of the security group you want to view\.

+ `CacheSecurityGroupName` – the name of the cache security group to return details for\.

**Example**  
Line breaks are added for ease of reading\.  

```
https://elasticache.amazonaws.com/
    ?Action=DescribeCacheSecurityGroups
    &CacheSecurityGroupName=mysecuritygroup
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