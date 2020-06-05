# Viewing a Cluster's Details<a name="Clusters.ViewDetails"></a>

You can view detail information about one or more clusters using the ElastiCache console, AWS CLI, or ElastiCache API\.

**Topics**
+ [Viewing a Cluster's Details \(Console\)](#Clusters.ViewDetails.CON.Memcached)
+ [Viewing a Cluster's Details \(AWS CLI\)](#Clusters.ViewDetails.CLI)
+ [Viewing a Cluster's Details \(ElastiCache API\)](#Clusters.ViewDetails.API)

## Viewing a Cluster's Details \(Console\)<a name="Clusters.ViewDetails.CON.Memcached"></a>

You can view the details of a Memcached cluster using the ElastiCache console, the AWS CLI for ElastiCache, or the ElastiCache API\.

The following procedure details how to view the details of a Memcached cluster using the ElastiCache console\.

**To view a Memcached cluster's details**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the list in the upper\-right corner, choose the AWS Region you are interested in\.

1. In the ElastiCache console dashboard, choose **Memcached**\. Doing this displays a list of all your clusters that are running any version of Memcached\.

1. To see details of a cluster, choose the box to the left of the cluster's name\.

1. To view node information:

   1. Choose the cluster's name\.

   1. Choose the **Nodes** tab\.

   1. To view metrics on one or more nodes, choose the box to the left of the Node ID, and then choose the time range for the metrics from the **Time range** list\. Choosing multiple nodes generates overlay graphs\.  
![\[Image: Metrics over the last hour for two Memcached nodes\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-Memcached-Metrics.png)

      *Metrics over the last hour for two Memcached nodes*

## Viewing a Cluster's Details \(AWS CLI\)<a name="Clusters.ViewDetails.CLI"></a>

You can view the details for a cluster using the AWS CLI `describe-cache-clusters` command\. If the `--cache-cluster-id` parameter is omitted, details for multiple clusters, up to `--max-items`, are returned\. If the `--cache-cluster-id` parameter is included, details for the specified cluster are returned\. You can limit the number of records returned with the `--max-items` parameter\.

The following code lists the details for `my-cluster`\.

```
aws elasticache describe-cache-clusters --cache-cluster-id my-cluster
```

The following code list the details for up to 25 clusters\.

```
aws elasticache describe-cache-clusters --max-items 25
```

**Example**  
For Linux, macOS, or Unix:  

```
aws elasticache describe-cache-clusters \
    --cache-cluster-id my-cluster \
    --show-cache-node-info
```
For Windows:  

```
aws elasticache describe-cache-clusters ^
    --cache-cluster-id my-cluster ^
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
                        "Address": "my-cluster.7ef-example.0001.usw2.cache.amazonaws.com"
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
                        "Address": "my-cluster.7ef-example.0002.usw2.cache.amazonaws.com"
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
                        "Address": "my-cluster.7ef-example.0003.usw2.cache.amazonaws.com"
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
            "CacheClusterId": "my-cluster", 
            "PreferredAvailabilityZone": "us-west-2b", 
            "ConfigurationEndpoint": {
                "Port": 11211, 
                "Address": "my-cluster.7ef-example.cfg.usw2.cache.amazonaws.com"
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

For more information, see the AWS CLI for ElastiCache topic [https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-clusters.html](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-clusters.html)\.

## Viewing a Cluster's Details \(ElastiCache API\)<a name="Clusters.ViewDetails.API"></a>

You can view the details for a cluster using the ElastiCache API `DescribeCacheClusters` action\. If the `CacheClusterId` parameter is included, details for the specified cluster are returned\. If the `CacheClusterId` parameter is omitted, details for up to `MaxRecords` \(default 100\) clusters are returned\. The value for `MaxRecords` cannot be less than 20 or greater than 100\.

The following code lists the details for `my-cluster`\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DescribeCacheClusters
   &CacheClusterId=my-cluster
   &Version=2015-02-02
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```

The following code list the details for up to 25 clusters\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DescribeCacheClusters
   &MaxRecords=25
   &Version=2015-02-02
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```

For more information, see the ElastiCache API reference topic [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheClusters.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheClusters.html)\.