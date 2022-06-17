# Listing a parameter group's values<a name="ParameterGroups.ListingValues"></a>

You can list the parameters and their values for a parameter group using the ElastiCache console, the AWS CLI, or the ElastiCache API\.

## Listing a parameter group's values \(Console\)<a name="ParameterGroups.ListingValues.CON"></a>

The following procedure shows how to list the parameters and their values for a parameter group using the ElastiCache console\.

**To list a parameter group's parameters and their values using the ElastiCache console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. To see a list of all available parameter groups, in the left hand navigation pane choose **Parameter Groups**\.

1. Choose the parameter group for which you want to list the parameters and values by choosing the box to the left of the parameter group's name\.

   The parameters and their values will be listed at the bottom of the screen\. Due to the number of parameters, you may have to scroll up and down to find the parameter you're interested in\.

## Listing a parameter group's values \(AWS CLI\)<a name="ParameterGroups.ListingValues.CLI"></a>

To list a parameter group's parameters and their values using the AWS CLI, use the command `describe-cache-parameters`\.

**Example**  
The following sample code list all the parameters and their values for the parameter group *myMem14*\.  
For Linux, macOS, or Unix:  

```
aws elasticache describe-cache-parameters \
    --cache-parameter-group-name myMem14
```
For Windows:  

```
aws elasticache describe-cache-parameters ^
    --cache-parameter-group-name myMem14
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-parameters.html](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-parameters.html)\.

## Listing a parameter group's values \(ElastiCache API\)<a name="ParameterGroups.ListingValues.API"></a>

To list a parameter group's parameters and their values using the ElastiCache API, use the `DescribeCacheParameters` action\.

**Example**  
The following sample code list all the parameters for the parameter group *myMem14*\.  

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DescribeCacheParameters
   &CacheParameterGroupName=myMem14
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &Version=2015-02-02
   &X-Amz-Credential=<credential>
```
The response from this action will look something like this\. This response has been truncated\.  

```
<DescribeCacheParametersResponse xmlns="http://elasticache.amazonaws.com/doc/2013-06-15/">
  <DescribeCacheParametersResult>
    <CacheClusterClassSpecificParameters>
      <CacheNodeTypeSpecificParameter>
        <DataType>integer</DataType>
        <Source>system</Source>
        <IsModifiable>false</IsModifiable>
        <Description>The maximum configurable amount of memory to use to store items, in megabytes.</Description>
        <CacheNodeTypeSpecificValues>
          <CacheNodeTypeSpecificValue>
            <Value>1000</Value>
            <CacheClusterClass>cache.c1.medium</CacheClusterClass>
          </CacheNodeTypeSpecificValue>
          <CacheNodeTypeSpecificValue>
            <Value>6000</Value>
            <CacheClusterClass>cache.c1.xlarge</CacheClusterClass>
          </CacheNodeTypeSpecificValue>
          <CacheNodeTypeSpecificValue>
            <Value>7100</Value>
            <CacheClusterClass>cache.m1.large</CacheClusterClass>
          </CacheNodeTypeSpecificValue>
          <CacheNodeTypeSpecificValue>
            <Value>1300</Value>
            <CacheClusterClass>cache.m1.small</CacheClusterClass>
          </CacheNodeTypeSpecificValue>
          
...output omitted...

    </CacheClusterClassSpecificParameters>
  </DescribeCacheParametersResult>
  <ResponseMetadata>
    <RequestId>6d355589-af49-11e0-97f9-279771c4477e</RequestId>
  </ResponseMetadata>
</DescribeCacheParametersResponse>
```

For more information, see [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheParameters.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheParameters.html)\.