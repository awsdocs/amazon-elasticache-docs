# Finding a Shard's ID<a name="shard-find-id"></a>

You can find a shard's ID using the AWS Management Console, the AWS CLI or the ElastiCache API\.

**Topics**
+ [Using the AWS Management Console](#shard-find-id-con)
+ [Using the AWS CLI](#shard-find-id-cli)
+ [Using the ElastiCache API](#shard-find-id-api)

## Using the AWS Management Console<a name="shard-find-id-con"></a>

**Topics**
+ [For Redis \(Cluster Mode Disabled\)](#shard-find-id-con-classic)
+ [For Redis \(Cluster Mode Enabled\)](#shard-find-id-con-cluster)

### For Redis \(Cluster Mode Disabled\)<a name="shard-find-id-con-classic"></a>

Redis \(cluster mode disabled\) replication group shard IDs are always `0001`\.

### For Redis \(Cluster Mode Enabled\)<a name="shard-find-id-con-cluster"></a>

The following procedure uses the AWS Management Console to find a Redis \(cluster mode enabled\)'s replication group's shard ID\.

**To find the shard ID in a Redis \(cluster mode enabled\) replication group**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. On the navigation pane, choose **Redis**, then choose the name of the Redis \(cluster mode enabled\) replication group you want to find the shard IDs for\.

1. In the **Shard Name** column, the shard ID is the last four digits of the shard name\.

## Using the AWS CLI<a name="shard-find-id-cli"></a>

To find shard \(node group\) ids for either Redis \(cluster mode disabled\) or Redis \(cluster mode enabled\) replication groups use the AWS CLI operation `describe-replication-groups` with the following optional parameter\.
+ **`--replication-group-id`**—An optional parameter which when used limits the output to the details of the specified replication group\. If this parameter is omitted, the details of up to 100 replication groups is returned\.

**Example**  
This command returns the details for `sample-repl-group`\.  
For Linux, macOS, or Unix:  

```
aws elasticache describe-replication-groups \
    --replication-group-id sample-repl-group
```
For Windows:  

```
aws elasticache describe-replication-groups ^
    --replication-group-id sample-repl-group
```
Output from this command looks something like this\. The shard \(node group\) ids are *highlighted* here to make finding them easier\.  

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
                            "CacheClusterId": "sample-repl-group-0001-001"
                        }, 
                        {
                            "PreferredAvailabilityZone": "us-west-2a", 
                            "CacheNodeId": "0001", 
                            "CacheClusterId": "sample-repl-group-0001-002"
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
                            "CacheClusterId": "sample-repl-group-0002-001"
                        }, 
                        {
                            "PreferredAvailabilityZone": "us-west-2a", 
                            "CacheNodeId": "0001", 
                            "CacheClusterId": "sample-repl-group-0002-002"
                        }
                    ]
                }
            ], 
            "ConfigurationEndpoint": {
                "Port": 6379, 
                "Address": "sample-repl-group.9dcv5r.clustercfg.usw2.cache.amazonaws.com"
            }, 
            "ClusterEnabled": true, 
            "ReplicationGroupId": "sample-repl-group", 
            "SnapshotRetentionLimit": 1, 
            "AutomaticFailover": "enabled", 
            "SnapshotWindow": "13:00-14:00", 
            "MemberClusters": [
                "sample-repl-group-0001-001", 
                "sample-repl-group-0001-002", 
                "sample-repl-group-0002-001", 
                "sample-repl-group-0002-002"
            ], 
            "CacheNodeType": "cache.m3.medium", 
            "PendingModifiedValues": {}
        }
    ]
}
```

## Using the ElastiCache API<a name="shard-find-id-api"></a>

To find shard \(node group\) ids for either Redis \(cluster mode disabled\) or Redis \(cluster mode enabled\) replication groups use the AWS CLI operation `describe-replication-groups` with the following optional parameter\.
+ **`ReplicationGroupId`**—An optional parameter which when used limits the output to the details of the specified replication group\. If this parameter is omitted, the details of up to *xxx* replication groups is returned\.

**Example**  
This command returns the details for `sample-repl-group`\.  
For Linux, macOS, or Unix:  

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DescribeReplicationGroup
   &ReplicationGroupId=sample-repl-group
   &Version=2015-02-02
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```