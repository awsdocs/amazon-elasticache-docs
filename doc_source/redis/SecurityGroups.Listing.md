# Listing available security groups<a name="SecurityGroups.Listing"></a>

This topic is relevant to you only if you are not running in an Amazon VPC\. If you are running in an Amazon VPC, see [Amazon VPCs and ElastiCache security](VPCs.md)\.

You can list which security groups have been created for your AWS account\.

The following procedures show you how to list the available security groups for your AWS account\.

## Listing available security groups \(Console\)<a name="SecurityGroups.Listing.CON"></a>

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Security Groups**\.

   The available security groups appear in the **Security Groups** list\.

## Listing available security groups \(AWS CLI\)<a name="SecurityGroups.Listing.CLI"></a>

At a command prompt, use the `describe-cache-security-groups` command to list all available security groups for your AWS account\.

```
aws elasticache describe-cache-security-groups
```

JSON output from this command will look something like this\.

```
{
    "Marker": "Marker",
    "CacheSecurityGroups": [
        {
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
    ]
}
```

For more information, see [describe\-cache\-security\-groups](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-security-groups.html)\.

## Listing available security groups \(ElastiCache API\)<a name="SecurityGroups.Listing.API"></a>

Using the ElastiCache API, call `DescribeCacheSecurityGroups`\.

**Example**  
Line breaks are added for ease of reading\.  

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=DescribeCacheSecurityGroups
    &MaxRecords=100
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