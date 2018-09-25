# Authorizing Network Access to an Amazon EC2 Security Group<a name="SecurityGroups.AuthorizingEC2"></a>

This topic is relevant to you only if you are not running in an Amazon VPC\. If you are running in an Amazon VPC, see [Amazon VPCs and ElastiCache Security](VPCs.md)\.

If you want to access your cluster from an Amazon EC2 instance, you must grant access to the Amazon EC2 security group that the EC2 instance belongs to\. The following procedures show you how to grant access to an Amazon EC2 Security Group\. 

**Important**  
Authorizing an Amazon EC2 security group only grants access to your clusters from all EC2 instances belonging to the Amazon EC2 security group\. 
It takes approximately one minute for changes to access permissions to take effect\. 

## Authorizing Network Access to an Amazon EC2 Security Group \(Console\)<a name="SecurityGroups.AuthorizingEC2.CON"></a>

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Security Groups**\.

1. In the **Security Groups** list, choose the box to the left of the security group that you want to grant access to\. 

1. At the bottom of the window, in the **EC2 Security Group Name** list, choose your Amazon EC2 security group\.

1. Choose **Add**\. 

## Authorizing Network Access to an Amazon EC2 Security Group \(AWS CLI\)<a name="SecurityGroups.AuthorizingEC2.CLI"></a>

At a command prompt, use the `authorize-cache-security-group-ingress` command to grant access to an Amazon EC2 security group with the following parameters\.
+ `--cache-security-group-name` – the name of the security group you are granting Amazon EC2 access to\.
+ `--ec2-security-group-name` – the name of the Amazon EC2 security group that the Amazon EC2 instance belongs to\.
+ `--ec2-security-group-owner-id` – the id of the owner of the Amazon EC2 security group\.

**Example**  
For Linux, macOS, or Unix:  

```
aws elasticache authorize-cache-security-group-ingress \
    --cache-security-group-name default  \
    --ec2-security-group-name myec2group \
    --ec2-security-group-owner-id 987654321021
```
For Windows:  

```
aws elasticache authorize-cache-security-group-ingress ^
    --cache-security-group-name default ^
    --ec2-security-group-name myec2group ^
    --ec2-security-group-owner-id 987654321021
```
The command should produce output similar to the following:  

```
{
    "CacheSecurityGroup": {
        "OwnerId": "OwnerId", 
        "CacheSecurityGroupName": "CacheSecurityGroupName", 
        "Description": "Description", 
        "EC2SecurityGroups": [
            {
                "Status": "available", 
                "EC2SecurityGroupName": "EC2SecurityGroupName", 
                "EC2SecurityGroupOwnerId": "EC2SecurityGroupOwnerId"
            }
        ]
    }
}
```

For more information, see [authorize\-cache\-security\-group\-ingress](https://docs.aws.amazon.com/cli/latest/reference/elasticache/authorize-cache-security-group-ingress.html)\.

## Authorizing Network Access to an Amazon EC2 Security Group \(ElastiCache API\)<a name="SecurityGroups.AuthorizingEC2.API"></a>

Using the ElastiCache API, call `AuthorizeCacheSecurityGroupIngress` with the following parameters:
+ `CacheSecurityGroupName` – the name of the security group you are granting Amazon EC2 access to\.
+ `EC2SecurityGroupName` – the name of the Amazon EC2 security group that the Amazon EC2 instance belongs to\.
+ `EC2SecurityGroupOwnerId` – the id of the owner of the Amazon EC2 security group\.

**Example**  

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=AuthorizeCacheSecurityGroupIngress
    &EC2SecurityGroupOwnerId=987654321021
    &EC2SecurityGroupName=myec2group
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

For more information, see [AuthorizeCacheSecurityGroupIngress](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_AuthorizeCacheSecurityGroupIngress.html)\.