# Finding Node Endpoints and Port Numbers<a name="ClientConfig.FindingEndpointsAndPorts"></a>

To connect to a cache node, your application needs to know the endpoint and port number for that node\.

## Finding Node Endpoints and Port Numbers \(Console\)<a name="ClientConfig.FindingEndpointsAndPorts.CON"></a>

 **To determine node endpoints and port numbers** 

1. Sign in to the [Amazon ElastiCache Management Console](https://aws.amazon.com/elasticache) and choose either **Memcached** or **Redis**\.

   A list of all clusters running the chosen engine appears\.

1. Continue below for the engine and configuration you're running\.

**Memcached**

1. Choose the name of the cluster of interest\.

1. Locate the **Port** and **Endpoint** columns for the node you're interested in\.

**Redis: Non\-cluster mode**

1. Choose the name of the cluster of interest\.

1. Locate the **Port** and **Endpoint** columns for the node you're interested in\.

**Redis: Cluster mode**

1. Choose the name of the cluster of interest\.

   A list of all the shards in that cluster appears\.

1. Choose the name of the shard of interest\.

   A list of all the nodes in that shard appears

1. Locate the **Port** and **Endpoint** columns for the node you're interested in\.

## Finding Cache Node Endpoints and Port Numbers \(AWS CLI\)<a name="ClientConfig.FindingEndpointsAndPorts.CLI"></a>

To determine cache node endpoints and port numbers, use the command `describe-cache-clusters` with the `--show-cache-node-info` parameter\.

```
aws elasticache describe-cache-clusters --show-cache-node-info 
```

This command should produce output similar to the following:

```
{
    "CacheClusters": [
        {
            "Engine": "redis", 
            "CacheNodes": [
                {
                    "CacheNodeId": "0001", 
                    "Endpoint": {
                        "Port": 6379, 
                        "Address": "redis0x1.7adw3s.0001.usw2.cache.amazonaws.com"
                    }, 
                    "CacheNodeStatus": "available", 
                    "ParameterGroupStatus": "in-sync", 
                    "CacheNodeCreateTime": "2017-04-05T20:45:28.907Z", 
                    "CustomerAvailabilityZone": "us-west-2b"
                }
            ], 
            "CacheParameterGroup": {
                "CacheNodeIdsToReboot": [], 
                "CacheParameterGroupName": "default.redis3.2", 
                "ParameterApplyStatus": "in-sync"
            }, 
            "SnapshotRetentionLimit": 1, 
            "CacheClusterId": "redis0x1", 
            "CacheSecurityGroups": [], 
            "NumCacheNodes": 1, 
            "SnapshotWindow": "00:00-01:00", 
            "CacheClusterCreateTime": "2017-04-05T20:45:28.907Z", 
            "AutoMinorVersionUpgrade": true, 
            "CacheClusterStatus": "available", 
            "PreferredAvailabilityZone": "us-west-2b", 
            "ClientDownloadLandingPage": "https://console.aws.amazon.com/elasticache/home#client-download:", 
            "CacheSubnetGroupName": "default", 
            "EngineVersion": "3.2.4", 
            "PendingModifiedValues": {}, 
            "PreferredMaintenanceWindow": "sun:06:00-sun:07:00", 
            "CacheNodeType": "cache.m3.medium"
        },
        
        ******* some output omitted for brevity *******
        
        {
            "Engine": "memcached", 
            "CacheNodes": [
                {
                    "CacheNodeId": "0001", 
                    "Endpoint": {
                        "Port": 11211, 
                        "Address": "mem03.5edv7s.0001.usw2.cache.amazonaws.com"
                    }, 
                    "CacheNodeStatus": "available", 
                    "ParameterGroupStatus": "in-sync", 
                    "CacheNodeCreateTime": "2017-04-25T19:24:38.977Z", 
                    "CustomerAvailabilityZone": "us-west-2a"
                }
            ], 
            "CacheParameterGroup": {
                "CacheNodeIdsToReboot": [], 
                "CacheParameterGroupName": "default.memcached1.4", 
                "ParameterApplyStatus": "in-sync"
            }, 
            "CacheClusterId": "mem03", 
            "PreferredAvailabilityZone": "us-west-2a", 
            "ConfigurationEndpoint": {
                "Port": 11211, 
                "Address": "mem03.9dcv5r.cfg.usw2.cache.amazonaws.com"
            }, 
            "CacheSecurityGroups": [], 
            "CacheClusterCreateTime": "2017-04-25T19:24:38.977Z", 
            "AutoMinorVersionUpgrade": true, 
            "CacheClusterStatus": "available", 
            "NumCacheNodes": 1, 
            "ClientDownloadLandingPage": "https://console.aws.amazon.com/elasticache/home#client-download:", 
            "SecurityGroups": [
                {
                    "Status": "active", 
                    "SecurityGroupId": "sg-dbe93fa2"
                }
            ], 
            "CacheSubnetGroupName": "default", 
            "EngineVersion": "1.4.34", 
            "PendingModifiedValues": {}, 
            "PreferredMaintenanceWindow": "thu:10:30-thu:11:30", 
            "CacheNodeType": "cache.t2.micro"
        }, 
    ]
}
```

The fully qualified DNS names and port numbers are in the Endpoint section of the output\.

## Finding Cache Node Endpoints and Port Numbers \(ElastiCache API\)<a name="ClientConfig.FindingEndpointsAndPorts.API"></a>

To determine cache node endpoints and port numbers, use the action `DescribeCacheClusters` with the `ShowCacheNodeInfo=true` parameter\.

**Example**  

```
 1. https://elasticache.us-west-2.amazonaws.com /
 2.     ?Action=DescribeCacheClusters
 3.     &ShowCacheNodeInfo=true
 4.     &SignatureVersion=4
 5.     &SignatureMethod=HmacSHA256
 6.     &Timestamp=20140421T220302Z
 7.     &Version=2014-09-30   
 8.     &X-Amz-Algorithm=AWS4-HMAC-SHA256
 9.     &X-Amz-Credential=<credential>
10.     &X-Amz-Date=20140421T220302Z
11.     &X-Amz-Expires=20140421T220302Z
12.     &X-Amz-Signature=<signature>
13.     &X-Amz-SignedHeaders=Host
```