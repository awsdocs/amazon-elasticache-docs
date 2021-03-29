# Modifying a subnet group<a name="SubnetGroups.Modifying"></a>

You can modify a subnet group's description, or modify the list of subnet IDs associated with the subnet group\. You cannot delete a subnet ID from a subnet group if a cluster is currently using that subnet\.

The following procedures show you how to modify a subnet group\.

## Modifying subnet groups \(Console\)<a name="SubnetGroups.Modifying.CON"></a>

**To modify a subnet group**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Subnet Groups**\.

1. In the list of subnet groups, choose the one you want to modify\.

1. In the lower portion of the ElastiCache console, make any changes to the description or the list of subnet IDs for the subnet group\. To save your changes, choose **Save**\.

## Modifying subnet groups \(AWS CLI\)<a name="SubnetGroups.Modifying.CLI"></a>

At a command prompt, use the command `modify-cache-subnet-group` to modify a subnet group\.

For Linux, macOS, or Unix:

```
aws elasticache modify-cache-subnet-group \
    --cache-subnet-group-name mysubnetgroup \
    --cache-subnet-group-description "New description" \
    --subnet-ids "subnet-42df9c3a" "subnet-48fc21a9"
```

For Windows:

```
aws elasticache modify-cache-subnet-group ^
    --cache-subnet-group-name mysubnetgroup ^
    --cache-subnet-group-description "New description" ^
    --subnet-ids "subnet-42df9c3a" "subnet-48fc21a9"
```

This command should produce output similar to the following:

```
{
    "CacheSubnetGroup": {
        "VpcId": "vpc-73cd3c17", 
        "CacheSubnetGroupDescription": "New description", 
        "Subnets": [
            {
                "SubnetIdentifier": "subnet-42dcf93a", 
                "SubnetAvailabilityZone": {
                    "Name": "us-west-2a"
                }
            },
            {
                "SubnetIdentifier": "subnet-48fc12a9", 
                "SubnetAvailabilityZone": {
                    "Name": "us-west-2a"
                }
            }
        ], 
        "CacheSubnetGroupName": "mysubnetgroup"
    }
}
```

For more information, see the AWS CLI topic [modify\-cache\-subnet\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-subnet-group.html)\.

## Modifying subnet groups \(ElastiCache API\)<a name="SubnetGroups.Modifying.API"></a>

Using the ElastiCache API, call `ModifyCacheSubnetGroup` with the following parameters:
+ `CacheSubnetGroupName=``mysubnetgroup`
+ Any other parameters whose values you want to change\. This example uses `CacheSubnetGroupDescription=``New%20description` to change the description of the subnet group\.

**Example**  

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=ModifyCacheSubnetGroup
    &CacheSubnetGroupDescription=New%20description
    &CacheSubnetGroupName=mysubnetgroup
    &SubnetIds.member.1=subnet-42df9c3a
    &SubnetIds.member.2=subnet-48fc21a9
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

**Note**  
When you create a new subnet group, take note the number of available IP addresses\. If the subnet has very few free IP addresses, you might be constrained as to how many more nodes you can add to the cluster\. To resolve this issue, you can assign one or more subnets to a subnet group so that you have a sufficient number of IP addresses in your cluster's Availability Zone\. After that, you can add more nodes to your cluster\.