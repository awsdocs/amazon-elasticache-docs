# Working with shards<a name="Shards"></a>

A shard \(API/CLI: node group\) is a collection of one to six Redis nodes\. A Redis \(cluster mode disabled\) cluster will never have more than one shard\. You can create a cluster with higher number of shards and lower number of replicas totaling up to 90 nodes per cluster\. This cluster configuration can range from 90 shards and 0 replicas to 15 shards and 5 replicas, which is the maximum number of replicas allowed\. The cluster's data is partitioned across the cluster's shards\. If there is more than one node in a shard, the shard implements replication with one node being the read/write primary node and the other nodes read\-only replica nodes\.

The node or shard limit can be increased to a maximum of 500 per cluster if the Redis engine version is 5\.0\.6 or higher\. For example, you can choose to configure a 500 node cluster that ranges between 83 shards \(one primary and 5 replicas per shard\) and 500 shards \(single primary and no replicas\)\. Make sure there are enough available IP addresses to accommodate the increase\. Common pitfalls include the subnets in the subnet group have too small a CIDR range or the subnets are shared and heavily used by other clusters\. For more information, see [Creating a subnet group](SubnetGroups.Creating.md)\.

 For versions below 5\.0\.6, the limit is 250 per cluster\.

To request a limit increase, see [AWS service limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) and choose the limit type **Nodes per cluster per instance type**\. 

When you create a Redis \(cluster mode enabled\) cluster using the ElastiCache console, you specify the number of shards in the cluster and the number of nodes in the shards\. For more information, see [Creating a Redis \(cluster mode enabled\) cluster \(Console\)](Clusters.Create.md#Clusters.Create.CON.RedisCluster)\. If you use the ElastiCache API or AWS CLI to create a cluster \(called *replication group* in the API/CLI\), you can configure the number of nodes in a shard \(API/CLI: node group\) independently\. For more information, see the following: 
+ API: [CreateReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html)
+ CLI: [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html)

Each node in a shard has the same compute, storage and memory specifications\. The ElastiCache API lets you control shard\-wide attributes, such as the number of nodes, security settings, and system maintenance windows\.

![\[Image: Redis shard configurations.\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCacheClusters-CSN-RedisShards.png)

*Redis shard configurations*

For more information, see [Offline resharding and shard rebalancing for Redis \(cluster mode enabled\)](redis-cluster-resharding-offline.md) and [Online resharding and shard rebalancing for Redis \(cluster mode enabled\)](redis-cluster-resharding-online.md)\.

## Finding a shard's ID<a name="shard-find-id"></a>

You can find a shard's ID using the AWS Management Console, the AWS CLI or the ElastiCache API\.

### Using the AWS Management Console<a name="shard-find-id-con"></a>



**Topics**
+ [For Redis \(Cluster Mode Disabled\)](#shard-find-id-con-classic)
+ [For Redis \(Cluster Mode Enabled\)](#shard-find-id-con-cluster)

#### For Redis \(Cluster Mode Disabled\)<a name="shard-find-id-con-classic"></a>

Redis \(cluster mode disabled\) replication group shard IDs are always `0001`\.

#### For Redis \(Cluster Mode Enabled\)<a name="shard-find-id-con-cluster"></a>

The following procedure uses the AWS Management Console to find a Redis \(cluster mode enabled\)'s replication group's shard ID\.

**To find the shard ID in a Redis \(cluster mode enabled\) replication group**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. On the navigation pane, choose **Redis**, then choose the name of the Redis \(cluster mode enabled\) replication group you want to find the shard IDs for\.

1. In the **Shard Name** column, the shard ID is the last four digits of the shard name\.

### Using the AWS CLI<a name="shard-find-id-cli"></a>

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
            "DataTiering": "disabled",
            "PendingModifiedValues": {}
        }
    ]
}
```

### Using the ElastiCache API<a name="shard-find-id-api"></a>

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