# Determine Available Engine Versions<a name="SelectEngine.RegionVersions"></a>

Not all versions of an engine are available in every region\. Therefore, before you create a cluster or replication group, you should determine which engine versions are supported in your region\.

You can determine which engine versions are supported in a region using the ElastiCache console, the AWS CLI, or the ElastiCache API\.

## Determine Available Engine Versions \(Console\)<a name="SelectEngine.RegionVersions.CON"></a>

When creating a cluster or replication group you are asked to choose an engine version from a list\. The engine versions in the list are those available in the current region\.

For more information, see [Creating a Cluster](Clusters.Create.md) or [Creating a Redis Cluster with Replicas from Scratch](Replication.CreatingReplGroup.NoExistingCluster.md)\.

## Determine Available Engine Versions \(AWS CLI\)<a name="SelectEngine.RegionVersions.CLI"></a>

To determine which engine versions are available in a region, use the `describe-cache-engine-versions` operation\. Use the optional parameter *\-\-region* to specify which region you want the available engine versions for\. If you omit the *\-\-region* parameter, engine versions are described for your current region\.

```
aws elasticache describe-cache-engine-versions --region us-east-2
```

The output of this operation should look something like this \(JSON format\)\.

```
{
    "CacheEngineVersions": [
        {
            "Engine": "memcached", 
            "CacheEngineDescription": "memcached", 
            "CacheEngineVersionDescription": "memcached version 1.4.14", 
            "CacheParameterGroupFamily": "memcached1.4", 
            "EngineVersion": "1.4.14"
        }, 
        ... some output omitted for brevity
        {
            "Engine": "redis", 
            "CacheEngineDescription": "Redis", 
            "CacheEngineVersionDescription": "redis version 2.8.6", 
            "CacheParameterGroupFamily": "redis2.8", 
            "EngineVersion": "2.8.6"
        }
    ]
}
```

For more information, see [describe\-cache\-engine\-versions](http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-engine-versions.html)\.

## Determine Available Engine Versions \(ElastiCache API\)<a name="SelectEngine.RegionVersions.API"></a>

To determine which engine versions are available in a region, use the `DescribeCacheEngineVersions` action\. Use the optional parameter *Region* to specify which region you want the available engine versions for\. If you omit the *Region* parameter, engine versions are described for your current region\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DescribeCacheEngineVersions
   &Region=us-east-2
   &Version=2015-02-02
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```

For more information, see [DescribeCacheEngineVersions](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheEngineVersions.html)\.