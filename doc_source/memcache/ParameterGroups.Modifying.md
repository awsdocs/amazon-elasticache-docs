# Modifying a Parameter Group<a name="ParameterGroups.Modifying"></a>

**Important**  
You cannot modify any default parameter group\.

You can modify some parameter values in a parameter group\. These parameter values are applied to clusters associated with the parameter group\. For more information on when a parameter value change is applied to a parameter group, see [Memcached Specific Parameters](ParameterGroups.Memcached.md)\.

## Modifying a Parameter Group \(Console\)<a name="ParameterGroups.Modifying.CON"></a>

The following procedure shows how to change the `binding_protocol` parameter's value using the ElastiCache console\. You would use the same procedure to change the value of any parameter\.

**To change a parameter's value using the ElastiCache console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. To see a list of all available parameter groups, in the left hand navigation pane choose **Parameter Groups**\.

1. Choose the parameter group you want to modify by choosing the box to the left of the parameter group's name\.

   The parameter group's parameters will be listed at the bottom of the screen\. You may need to page through the list to see all the parameters\.

1. To modify one or more parameters, choose **Edit Parameters**\.

1. In the **Edit Parameter Group:** screen, scroll using the left and right arrows until you find the `binding_protocol` parameter, then type `ascii` in the **Value** column\.

1. Choose **Save Changes**\.

1. To find the name of the parameter you changed, see [Memcached Specific Parameters](ParameterGroups.Memcached.md)\. If changes to the parameter take place *After restart*, reboot every cluster that uses this parameter group\. For more information, see [Rebooting a Cluster](Clusters.Rebooting.md)\.

## Modifying a Parameter Group \(AWS CLI\)<a name="ParameterGroups.Modifying.CLI"></a>

To change a parameter's value using the AWS CLI, use the command `modify-cache-parameter-group`\.

**Example**  
To find the name and permitted values of the parameter you want to change, see [Memcached Specific Parameters](ParameterGroups.Memcached.md)  
The following sample code sets the value of two parameters, *chunk\_size* and *chunk\_size\_growth\_fact* on the parameter group `myMem14`\.  
For Linux, macOS, or Unix:  

```
aws elasticache modify-cache-parameter-group \
    --cache-parameter-group-name myMem14 \
    --parameter-name-values \
        ParameterName=chunk_size,ParameterValue=96 \
        ParameterName=chunk_size_growth_fact,ParameterValue=1.5
```
For Windows:  

```
aws elasticache modify-cache-parameter-group ^
    --cache-parameter-group-name myMem14 ^
    --parameter-name-values ^
        ParameterName=chunk_size,ParameterValue=96 ^
        ParameterName=chunk_size_growth_fact,ParameterValue=1.5
```
Output from this command will look something like this\.  

```
{
    "CacheParameterGroupName": "myMem14"
}
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-parameter-group.html](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-parameter-group.html)\.

If changes to the parameter take place *After restart*, reboot every cluster that uses this parameter group\. For more information, see [Rebooting a Cluster](Clusters.Rebooting.md)\.

## Modifying a Parameter Group \(ElastiCache API\)<a name="ParameterGroups.Modifying.API"></a>

To change a parameter group's parameter values using the ElastiCache API, use the `ModifyCacheParameterGroup` action\.

**Example**  
To find the name and permitted values of the parameter you want to change, see [Memcached Specific Parameters](ParameterGroups.Memcached.md)  
The following sample code sets the value of two parameters, *chunk\_size* and *chunk\_size\_growth\_fact* on the parameter group `myMem14`\.  

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=ModifyCacheParameterGroup
   &CacheParameterGroupName=myMem14
   &ParameterNameValues.member.1.ParameterName=chunk_size
   &ParameterNameValues.member.1.ParameterValue=96
   &ParameterNameValues.member.2.ParameterName=chunk_size_growth_fact
   &ParameterNameValues.member.2.ParameterValue=1.5
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &Version=2015-02-02
   &X-Amz-Credential=<credential>
```

For more information, see [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheParameterGroup.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheParameterGroup.html)\.

After updating and saving the parameter, if the change to the parameter take place *After restart*, reboot every cluster that uses this parameter group\. For more information, see [Rebooting a Cluster](Clusters.Rebooting.md)\.