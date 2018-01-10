# Listing a Parameter Group's Values<a name="ParameterGroups.ListingValues"></a>

You can list the parameters and their values for a parameter group using the ElastiCache console, the AWS CLI, or the ElastiCache API\.

## Listing a Parameter Group's Values \(Console\)<a name="ParameterGroups.ListingValues.CON"></a>

The following procedure shows how to list the parameters and their values for a parameter group using the ElastiCache console\.

**To list a parameter group's parameters and their values using the ElastiCache console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. To see a list of all available parameter groups, in the left hand navigation pane choose **Parameter Groups**\.

1. Choose the parameter group for which you want to list the parameters and values by choosing the box to the left of the parameter group's name\.

   The parameters and their values will be listed at the bottom of the screen\. Due to the number of parameters, you may have to scroll up and down to find the parameter you're interested in\.

## Listing a Parameter Group's Values \(AWS CLI\)<a name="ParameterGroups.ListingValues.CLI"></a>

To list a parameter group's parameters and their values using the AWS CLI, use the command `describe-cache-parameters`\.

The following sample code list all the parameters and their values for the parameter group *myRedis28*\.

For Linux, macOS, or Unix:

```
aws elasticache describe-cache-parameters \
    --cache-parameter-group-name myRedis28
```

For Windows:

```
aws elasticache describe-cache-parameters ^
    --cache-parameter-group-name myRedis28
```

The output of this command will look something like this\.

```
{
    "Parameters": [
        {
            "Description": "Apply rehashing or not.", 
            "DataType": "string", 
            "ChangeType": "requires-reboot", 
            "IsModifiable": true, 
            "AllowedValues": "yes,no", 
            "Source": "system", 
            "ParameterValue": "yes", 
            "ParameterName": "activerehashing", 
            "MinimumEngineVersion": "2.8.6"
        }, 
(...sample truncated...)
        {
            "Description": "Enable Redis persistence.", 
            "DataType": "string", 
            "ChangeType": "immediate", 
            "IsModifiable": true, 
            "AllowedValues": "yes,no", 
            "Source": "system", 
            "ParameterValue": "no", 
            "ParameterName": "appendonly", 
            "MinimumEngineVersion": "2.8.6"
        }
    ]
}
```

For more information, see [http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-parameters.html](http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-parameters.html)\.

## Listing a Parameter Group's Values \(ElastiCache API\)<a name="ParameterGroups.ListingValues.API"></a>

To list a parameter group's parameters and their values using the ElastiCache API, use the `DescribeCacheParameters` action\.

The following sample code list all the parameters for the parameter group *myRedis28*\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DescribeCacheParameters
   &CacheParameterGroupName=myRedis28
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

        </CacheNodeTypeSpecificValues>
        <AllowedValues>1-100000</AllowedValues>
        <ParameterName>max_cache_memory</ParameterName>
        <MinimumEngineVersion>1.4.5</MinimumEngineVersion>
      </CacheNodeTypeSpecificParameter>
      <CacheNodeTypeSpecificParameter>
        <DataType>integer</DataType>
        <Source>system</Source>
        <IsModifiable>false</IsModifiable>
        <Description>The number of memcached threads to use.</Description>
        <CacheNodeTypeSpecificValues>
          <CacheNodeTypeSpecificValue>
            <Value>2</Value>
            <CacheClusterClass>cache.c1.medium</CacheClusterClass>
          </CacheNodeTypeSpecificValue>
          <CacheNodeTypeSpecificValue>
            <Value>8</Value>
            <CacheClusterClass>cache.c1.xlarge</CacheClusterClass>
          </CacheNodeTypeSpecificValue>

...output omitted...

        </CacheNodeTypeSpecificValues>
        <AllowedValues>1-8</AllowedValues>
        <ParameterName>num_threads</ParameterName>
        <MinimumEngineVersion>1.4.5</MinimumEngineVersion>
      </CacheNodeTypeSpecificParameter>
    </CacheClusterClassSpecificParameters>
    <Parameters>
      <Parameter>
        <ParameterValue>1024</ParameterValue>
        <DataType>integer</DataType>
        <Source>system</Source>
        <IsModifiable>false</IsModifiable>
        <Description>The backlog queue limit.</Description>
        <AllowedValues>1-10000</AllowedValues>
        <ParameterName>backlog_queue_limit</ParameterName>
        <MinimumEngineVersion>1.4.5</MinimumEngineVersion>
      </Parameter>
      <Parameter>
        <ParameterValue>auto</ParameterValue>
        <DataType>string</DataType>
        <Source>system</Source>
        <IsModifiable>false</IsModifiable>
        <Description>Binding protocol.</Description>
        <AllowedValues>auto,binary,ascii</AllowedValues>
        <ParameterName>binding_protocol</ParameterName>
        <MinimumEngineVersion>1.4.5</MinimumEngineVersion>
      </Parameter>

...output omitted...

    </Parameters>
  </DescribeCacheParametersResult>
  <ResponseMetadata>
    <RequestId>6d355589-af49-11e0-97f9-279771c4477e</RequestId>
  </ResponseMetadata>
</DescribeCacheParametersResponse>
```

For more information, see [http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheParameters.html](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheParameters.html)\.