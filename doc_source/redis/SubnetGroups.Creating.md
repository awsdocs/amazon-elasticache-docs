# Creating a subnet group<a name="SubnetGroups.Creating"></a>

A *cache subnet group* is a collection of subnets that you may want to designate for your cache clusters in a VPC\. When launching a cache cluster in a VPC, you need to select a cache subnet group\. Then ElastiCache uses that cache subnet group to assign IP addresses within that subnet to each cache node in the cluster\.

When you create a new subnet group, note the number of available IP addresses\. If the subnet has very few free IP addresses, you might be constrained as to how many more nodes you can add to the cluster\. To resolve this issue, you can assign one or more subnets to a subnet group so that you have a sufficient number of IP addresses in your cluster's Availability Zone\. After that, you can add more nodes to your cluster\.

The following procedures show you how to create a subnet group called `mysubnetgroup` \(console\), the AWS CLI, and the ElastiCache API\.

## Creating a subnet group \(Console\)<a name="SubnetGroups.Creating.CON"></a>

The following procedure shows how to create a subnet group \(console\)\.

**To create a subnet group \(Console\)**

1. Sign in to the AWS Management Console, and open the ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation list, choose **Subnet Groups**\.

1. Choose **Create Subnet Group**\.

1. In the **Create Subnet Group** wizard, do the following\. When all the settings are as you want them, choose **Yes, Create**\.

   1. In the **Name** box, type a name for your subnet group\.

   1. In the **Description** box, type a description for your subnet group\.

   1. In the **VPC ID** box, choose the Amazon VPC that you created\.

   1. In the **Availability Zone** and **Subnet ID** lists, choose the Availability Zone or [Local Zone](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Local_zones.html) and ID of your private subnet, and then choose **Add**\.  
![\[Image: Create Subnet VPC screen\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/vpc-03.png)

1. In the confirmation message that appears, choose **Close**\.

Your new subnet group appears in the **Subnet Groups** list of the ElastiCache console\. At the bottom of the window you can choose the subnet group to see details, such as all of the subnets associated with this group\.

## Creating a subnet group \(AWS CLI\)<a name="SubnetGroups.Creating.CLI"></a>

At a command prompt, use the command `create-cache-subnet-group` to create a subnet group\.

For Linux, macOS, or Unix:

```
aws elasticache create-cache-subnet-group \
    --cache-subnet-group-name mysubnetgroup \
    --cache-subnet-group-description "Testing" \
    --subnet-ids subnet-53df9c3a
```

For Windows:

```
aws elasticache create-cache-subnet-group ^
    --cache-subnet-group-name mysubnetgroup ^
    --cache-subnet-group-description "Testing" ^
    --subnet-ids subnet-53df9c3a
```

This command should produce output similar to the following:

```
{
    "CacheSubnetGroup": {
        "VpcId": "vpc-37c3cd17", 
        "CacheSubnetGroupDescription": "Testing", 
        "Subnets": [
            {
                "SubnetIdentifier": "subnet-53df9c3a", 
                "SubnetAvailabilityZone": {
                    "Name": "us-west-2a"
                }
            }
        ], 
        "CacheSubnetGroupName": "mysubnetgroup"
    }
}
```

For more information, see the AWS CLI topic [create\-cache\-subnet\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-cache-subnet-group.html)\.

## Creating a subnet group \(ElastiCache API\)<a name="SubnetGroups.Creating.API"></a>

Using the ElastiCache API, call `CreateCacheSubnetGroup` with the following parameters: 
+ `CacheSubnetGroupName=``mysubnetgroup`
+ `CacheSubnetGroupDescription=``=Testing`
+ `SubnetIds.member.1=``subnet-53df9c3a`

**Example**  

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=CreateCacheSubnetGroup
    &CacheSubnetGroupDescription=Testing
    &CacheSubnetGroupName=mysubnetgroup
    &SignatureMethod=HmacSHA256
    &SignatureVersion=4
    &SubnetIds.member.1=subnet-53df9c3a 
    &Timestamp=20141201T220302Z
    &Version=2014-12-01
    &X-Amz-Algorithm=AWS4-HMAC-SHA256
    &X-Amz-Credential=<credential>
    &X-Amz-Date=20141201T220302Z
    &X-Amz-Expires=20141201T220302Z
    &X-Amz-Signature=<signature>
    &X-Amz-SignedHeaders=Host
```