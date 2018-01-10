# Viewing a Cluster's Details \(AWS CLI\)<a name="Clusters.ViewDetails.CLI"></a>

You can view the details for a cluster using the AWS CLI `describe-cache-clusters` command\. If the `--cache-cluster-id` parameter is omitted, details for multiple clusters, up to `--max-items`, are returned\. If the `--cache-cluster-id` parameter is included, details for the specified cluster are returned\. You can limit the number of records returned with the `--max-items` parameter\.

The following code lists the details for `myCluster`\.

```
aws elasticache describe-cache-clusters --cache-cluster-id myCluster
```

The following code list the details for up to 25 clusters\.

```
aws elasticache describe-cache-clusters --max-items 25
```

Use the command `describe-cache-cluster` to display a list of nodes for a cluster, as in the following example, and note the identifiers of the nodes you want to remove\. 

For Linux, macOS, or Unix:

```
aws elasticache describe-cache-clusters \
    --cache-cluster-id my-memcached-cluster \
    --show-cache-node-info
```

For Windows:

```
aws elasticache describe-cache-clusters ^
    --cache-cluster-id my-memcached-cluster ^
    --show-cache-node-info
```

This operation produces output similar to the following \(JSON format\):

```
{
    "CacheClusters": [
        {
            "Engine": "memcached", 
            "CacheNodes": [
                {
                    "CacheNodeId": "0001", 
                    "Endpoint": {
                        "Port": 11211, 
                        "Address": "my-memcached-cluster.7ef-example.0001.usw2.cache.amazonaws.com"
                    }, 
                    "CacheNodeStatus": "available", 
                    "ParameterGroupStatus": "in-sync", 
                    "CacheNodeCreateTime": "2016-09-21T16:28:28.973Z", 
                    "CustomerAvailabilityZone": "us-west-2b"
                }, 
                {
                    "CacheNodeId": "0002", 
                    "Endpoint": {
                        "Port": 11211, 
                        "Address": "my-memcached-cluster.7ef-example.0002.usw2.cache.amazonaws.com"
                    }, 
                    "CacheNodeStatus": "available", 
                    "ParameterGroupStatus": "in-sync", 
                    "CacheNodeCreateTime": "2016-09-21T16:28:28.973Z", 
                    "CustomerAvailabilityZone": "us-west-2b"
                }, 
                {
                    "CacheNodeId": "0003", 
                    "Endpoint": {
                        "Port": 11211, 
                        "Address": "my-memcached-cluster.7ef-example.0003.usw2.cache.amazonaws.com"
                    }, 
                    "CacheNodeStatus": "available", 
                    "ParameterGroupStatus": "in-sync", 
                    "CacheNodeCreateTime": "2016-09-21T16:28:28.973Z", 
                    "CustomerAvailabilityZone": "us-west-2b"
                }
            ], 
            "CacheParameterGroup": {
                "CacheNodeIdsToReboot": [], 
                "CacheParameterGroupName": "default.memcached1.4", 
                "ParameterApplyStatus": "in-sync"
            }, 
            "CacheClusterId": "my-memcached-cluster", 
            "PreferredAvailabilityZone": "us-west-2b", 
            "ConfigurationEndpoint": {
                "Port": 11211, 
                "Address": "my-memcached-cluster.7ef-example.cfg.usw2.cache.amazonaws.com"
            }, 
            "CacheSecurityGroups": [], 
            "CacheClusterCreateTime": "2016-09-21T16:28:28.973Z", 
            "AutoMinorVersionUpgrade": true, 
            "CacheClusterStatus": "available", 
            "NumCacheNodes": 3, 
            "ClientDownloadLandingPage": "https://console.aws.amazon.com/elasticache/home#client-download:", 
            "SecurityGroups": [
                {
                    "Status": "active", 
                    "SecurityGroupId": "sg-dbe93fa2"
                }
            ], 
            "CacheSubnetGroupName": "default", 
            "EngineVersion": "1.4.24", 
            "PendingModifiedValues": {}, 
            "PreferredMaintenanceWindow": "sat:09:00-sat:10:00", 
            "CacheNodeType": "cache.m3.medium"
        }
    ]
}
```

For more information, go to the AWS CLI for ElastiCache topic [http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-clusters.html](http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-clusters.html)\.