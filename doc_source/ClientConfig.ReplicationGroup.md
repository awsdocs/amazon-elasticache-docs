# Connecting to Nodes in a Redis Cluster<a name="ClientConfig.ReplicationGroup"></a>

**Note**  
At this time, clusters \(API/CLI: replication groups\) that support replication and read replicas are only supported for clusters running Redis\.

For clusters, ElastiCache provides console, CLI, and API interfaces to obtain connection information for individual nodes\.

For read\-only activity, applications can connect to any node in the cluster\. However, for write activity, we recommend that your applications connect to the primary endpoint \(Redis \(cluster mode disabled\)\) or configuration endpoint \(Redis \(cluster mode enabled\)\) for the cluster instead of connecting directly to a node\. This will ensure that your applications can always find the correct node, even if you decide to reconfigure your cluster by promoting a read replica to the primary role\.

## Connecting to Clusters in a Cluster \(Console\)<a name="ClientConfig.ReplicationGroup.CON"></a>

**To determine endpoints and port numbers**

+ See the topic, [Finding a Redis \(cluster mode disabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Redis)\.

## Connecting to Clusters in a Replication Group \(AWS CLI\)<a name="ClientConfig.ReplicationGroup.CLI"></a>

 **To determine cache node endpoints and port numbers**

Use the command `describe-replication-groups` with the name of your replication group:

```
aws elasticache describe-replication-groups redis2x2
```

This command should produce output similar to the following:

```
{
    "ReplicationGroups": [
        {
            "Status": "available", 
            "Description": "2 shards, 2 nodes (1 + 1 replica)", 
            "NodeGroups": [
                {
                    "Status": "available", 
                    "Slots": "0-8191", 
                    "NodeGroupId": "0001", 
                    "NodeGroupMembers": [
                        {
                            "PreferredAvailabilityZone": "us-west-2c", 
                            "CacheNodeId": "0001", 
                            "CacheClusterId": "redis2x2-0001-001"
                        }, 
                        {
                            "PreferredAvailabilityZone": "us-west-2a", 
                            "CacheNodeId": "0001", 
                            "CacheClusterId": "redis2x2-0001-002"
                        }
                    ]
                }, 
                {
                    "Status": "available", 
                    "Slots": "8192-16383", 
                    "NodeGroupId": "0002", 
                    "NodeGroupMembers": [
                        {
                            "PreferredAvailabilityZone": "us-west-2b", 
                            "CacheNodeId": "0001", 
                            "CacheClusterId": "redis2x2-0002-001"
                        }, 
                        {
                            "PreferredAvailabilityZone": "us-west-2a", 
                            "CacheNodeId": "0001", 
                            "CacheClusterId": "redis2x2-0002-002"
                        }
                    ]
                }
            ], 
            "ConfigurationEndpoint": {
                "Port": 6379, 
                "Address": "redis2x2.9dcv5r.clustercfg.usw2.cache.amazonaws.com"
            }, 
            "ClusterEnabled": true, 
            "ReplicationGroupId": "redis2x2", 
            "SnapshotRetentionLimit": 1, 
            "AutomaticFailover": "enabled", 
            "SnapshotWindow": "13:00-14:00", 
            "MemberClusters": [
                "redis2x2-0001-001", 
                "redis2x2-0001-002", 
                "redis2x2-0002-001", 
                "redis2x2-0002-002"
            ], 
            "CacheNodeType": "cache.m3.medium", 
            "PendingModifiedValues": {}
        }
    ]
}
```

## Connecting to Clusters in a Replication Group \(ElastiCache API\)<a name="ClientConfig.ReplicationGroup.API"></a>

 **To determine cache node endpoints and port numbers** 

Call `DescribeReplicationGroups` with the following parameter:

`ReplicationGroupId` = the name of your replication group\.

**Example**  

```
 1. https://elasticache.us-west-2.amazonaws.com /
 2.     ?Action=DescribeCacheClusters
 3.     &ReplicationGroupId=repgroup01
 4.     &Version=2014-09-30   
 5.     &SignatureVersion=4
 6.     &SignatureMethod=HmacSHA256
 7.     &Timestamp=20140421T220302Z
 8.     &X-Amz-Algorithm=AWS4-HMAC-SHA256
 9.     &X-Amz-Date=20140421T220302Z
10.     &X-Amz-SignedHeaders=Host
11.     &X-Amz-Expires=20140421T220302Z
12.     &X-Amz-Credential=<credential>
13.     &X-Amz-Signature=<signature>
```