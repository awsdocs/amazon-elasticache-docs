# Finding connection endpoints<a name="Endpoints"></a>

Your application connects to your cluster using endpoints\. An endpoint is a node or cluster's unique address\.

**Which endpoints to use**
+ **Redis standalone node**, use the node's endpoint for both read and write operations\.

   
+ **Redis \(cluster mode disabled\) clusters**, use the *Primary Endpoint* for all write operations\. Use the *Reader Endpoint* to evenly split incoming connections to the endpoint between all read replicas\. Use the individual *Node Endpoints* for read operations \(In the API/CLI these are referred to as Read Endpoints\)\.

   
+ **Redis \(cluster mode enabled\) clusters**, use the cluster's *Configuration Endpoint* for all operations\. You must use a client that supports Redis Cluster \(Redis 3\.2\)\. You can still read from individual node endpoints \(In the API/CLI these are referred to as Read Endpoints\)\.

   

The following sections guide you through discovering the endpoints you'll need for the engine you're running\.

**Topics**
+ [Finding Redis \(Cluster Mode Disabled\) Cluster Endpoints \(Console\)](#Endpoints.Find.Redis)
+ [Finding Endpoints for a Redis \(Cluster Mode Enabled\) Endpoints \(Console\)](#Endpoints.Find.RedisCluster)
+ [Finding Endpoints \(AWS CLI\)](#Endpoints.Find.CLI)
+ [Finding Endpoints \(ElastiCache API\)](#Endpoints.Find.API)

## Finding a Redis \(Cluster Mode Disabled\) Cluster's Endpoints \(Console\)<a name="Endpoints.Find.Redis"></a>

If a Redis \(cluster mode disabled\) cluster has only one node, the node's endpoint is used for both reads and writes\. If a Redis \(cluster mode disabled\) cluster has multiple nodes, there are three types of endpoints; the *primary endpoint*, the *reader endpoint* and the *node endpoints*\.

The primary endpoint is a DNS name that always resolves to the primary node in the cluster\. The primary endpoint is immune to changes to your cluster, such as promoting a read replica to the primary role\. For write activity, we recommend that your applications connect to the primary endpoint instead of connecting directly to the primary\.

A reader endpoint will evenly split incoming connections to the endpoint between all read replicas in a ElastiCache for Redis cluster\. Additional factors such as when the application creates the connections or how the application \(re\)\-uses the connections will determine the traffic distribution\. Reader endpoints keep up with cluster changes in real\-time as replicas are added or removed\. You can place your ElastiCache for Redis cluster’s multiple read replicas in different AWS Availability Zones \(AZ\) to ensure high availability of reader endpoints\. 

**Note**  
A reader endpoint is not a load balancer\. It is a DNS record that will resolve to an IP address of one of the replica nodes in a round robin fashion\.

For read activity, applications can also connect to any node in the cluster\. Unlike the primary endpoint, node endpoints resolve to specific endpoints\. If you make a change in your cluster, such as adding or deleting a replica, you must update the node endpoints in your application\.

**To find a Redis \(cluster mode disabled\) cluster's endpoints**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Redis**\.

   The clusters screen will appear with a list of Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\) clusters\.

1. To find the cluster's Primary and/or Reader endpoints, choose the box to the left of cluster's name\.  
![\[Image: Primary endpoint for a Redis (cluster mode disabled) cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/Reader-Endpoint.png)

   *Primary and Reader endpoints for a Redis \(cluster mode disabled\) cluster*

   If there is only one node in the cluster, there is no primary endpoint and you can continue at the next step\.

1. If the Redis \(cluster mode disabled\) cluster has replica nodes, you can find the cluster's replica node endpoints by choosing the cluster's name\.

   The nodes screen appears with each node in the cluster, primary and replicas, listed with its endpoint\.  
![\[Image: Node endpoints for a Redis (cluster mode disabled) cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-Endpoints-Redis-Node.png)

   *Node endpoints for a Redis \(cluster mode disabled\) cluster*

1. To copy an endpoint to your clipboard:

   1. One endpoint at a time, find then highlight the endpoint you want to copy\.

   1. Right\-click the highlighted endpoint, then choose **Copy** from the context menu\.

   The highlighted endpoint is now copied to your clipboard\. For information on using the endpoint to connect to a node, see [Connecting to nodes](nodes-connecting.md)\.

A Redis \(cluster mode disabled\) primary endpoint looks something like the following\. There is a difference depending upon whether or not In\-Transit encryption is enabled\.

**In\-transit encryption not enabled**

```
clusterName.xxxxxx.nodeId.regionAndAz.cache.amazonaws.com:port
			
redis-01.7abc2d.0001.usw2.cache.amazonaws.com:6379
```

**In\-transit encryption enabled**

```
master.clusterName.xxxxxx.regionAndAz.cache.amazonaws.com:port

master.ncit.ameaqx.use1.cache.amazonaws.com:6379
```

## Finding Endpoints for a Redis \(Cluster Mode Enabled\) Cluster \(Console\)<a name="Endpoints.Find.RedisCluster"></a>

Use the *Configuration Endpoint* for both read and write operations\. Redis determines which of the cluster's node to access\.

The following procedure demonstrates how to find and copy Redis \(cluster mode enabled\) cluster endpoints\.

**To find the configuration endpoint for a Redis \(cluster mode enabled\) cluster**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Redis**\.

   A list of clusters running any version of Redis appears\.

1. From the list of clusters, choose the box to the left of a cluster running "Clustered Redis"\.

   The screen expands showing details about the selected cluster\.

1. Locate the *Configuration endpoint*\.  
![\[Image: Configuration endpoint for a Redis (cluster mode enabled) cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-Endpoints-RedisCluster-Configuration.png)

   *Configuration endpoint for a Redis \(cluster mode enabled\) cluster*

**To find the node endpoints for a Redis \(cluster mode enabled\) cluster**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Redis**\.

   A list of clusters running any version of Redis appears\.

1. From the list of clusters, choose the cluster name of a cluster running "Clustered Redis"\.

   The shards page opens\.

1. Choose the name of the shard you want node endpoint for\.

   A list of the shard's nodes appears with each node's endpoint\.

1. Locate the *Endpoint* column and read the endpoint for each node\.  
![\[Image: Node endpoints for a Redis (cluster mode enabled) cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/ElastiCache-Endpoints-RedisCluster-Node.png)

   *Node endpoints for a Redis \(cluster mode enabled\) cluster*

**To copy an endpoint to your clipboard**

1. Find the endpoint you want to copy using one of the preceeding procedures\.

1. Highlight the endpoint that you want to copy\.

1. Right\-click the highlighted endpoint and choose **Copy** from the context menu\.

   The highlighted endpoint is now copied to your clipboard\. For information on using the endpoint to connect to a node, see [Connecting to nodes](nodes-connecting.md)\.

A Redis \(cluster mode enabled\) configuration endpoint looks something like the following\.

**In\-transit encryption not enabled**

```
clusterName.xxxxxx.regionAndAz.cache.amazonaws.com:port
			
rce.ameaqx.use1.cache.amazonaws.com:6379
```

**In\-transit encryption enabled**

```
clustercfg.clusterName.xxxxxx.regionAndAz.cache.amazonaws.com:port
			
clustercfg.rce.ameaqx.use1.cache.amazonaws.com:6379
```

## Finding Endpoints \(AWS CLI\)<a name="Endpoints.Find.CLI"></a>

You can use the AWS CLI for Amazon ElastiCache to discover the endpoints for nodes, clusters, and replication groups\.

**Topics**
+ [Finding Endpoints for Nodes and Clusters \(AWS CLI\)](#Endpoints.Find.CLI.Nodes)
+ [Finding the Endpoints for Replication Groups \(AWS CLI\)](#Endpoints.Find.CLI.ReplGroups)

### Finding Endpoints for Nodes and Clusters \(AWS CLI\)<a name="Endpoints.Find.CLI.Nodes"></a>

You can use the AWS CLI to discover the endpoints for a cluster and its nodes with the `describe-cache-clusters` command\. For Redis clusters, the command returns the cluster endpoint\.  If you include the optional parameter `--show-cache-node-info`, the command will also return the endpoints of the individual nodes in the cluster\.

**Example**  
The following command retrieves the cluster information for the single\-node Redis \(cluster mode disabled\) cluster *mycluster*\.  
The parameter `--cache-cluster-id` can be used with single\-node Redis \(cluster mode disabled\) cluster id or specific node ids in Redis replication groups\. The `--cache-cluster-id` of a Redis replication group is a 4\-digit value such as `0001`\. If `--cache-cluster-id` is the id of a cluster \(node\) in a Redis replication group, the `replication-group-id` is included in the output\.
For Linux, macOS, or Unix:  

```
aws elasticache describe-cache-clusters \
    --cache-cluster-id redis-cluster \
    --show-cache-node-info
```
For Windows:  

```
aws elasticache describe-cache-clusters ^
    --cache-cluster-id redis-cluster ^
    --show-cache-node-info
```
Output from the above operation should look something like this \(JSON format\)\.  

```
{
    "CacheClusters": [
        {
            "CacheClusterStatus": "available",
            "SecurityGroups": [
                {
                    "SecurityGroupId": "sg-77186e0d",
                    "Status": "active"
                }
            ],
            "CacheNodes": [
                {
                    "CustomerAvailabilityZone": "us-east-1b",
                    "CacheNodeCreateTime": "2018-04-25T18:19:28.241Z",
                    "CacheNodeStatus": "available",
                    "CacheNodeId": "0001",
                    "Endpoint": {
                        "Address": "redis-cluster.ameaqx.0001.use1.cache.amazonaws.com",
                        "Port": 6379
                    },
                    "ParameterGroupStatus": "in-sync"
                }
            ],
            "AtRestEncryptionEnabled": false,
            "CacheClusterId": "redis-cluster",
            "TransitEncryptionEnabled": false,
            "CacheParameterGroup": {
                "ParameterApplyStatus": "in-sync",
                "CacheNodeIdsToReboot": [],
                "CacheParameterGroupName": "default.redis3.2"
            },
            "NumCacheNodes": 1,
            "PreferredAvailabilityZone": "us-east-1b",
            "AutoMinorVersionUpgrade": true,
            "Engine": "redis",
            "AuthTokenEnabled": false,
            "PendingModifiedValues": {},
            "PreferredMaintenanceWindow": "tue:08:30-tue:09:30",
            "CacheSecurityGroups": [],
            "CacheSubnetGroupName": "default",
            "CacheNodeType": "cache.t2.small",
            "EngineVersion": "3.2.10",
            "ClientDownloadLandingPage": "https://console.aws.amazon.com/elasticache/home#client-download:",
            "CacheClusterCreateTime": "2018-04-25T18:19:28.241Z"
        }
    ]
}
```

For more information, see the topic [describe\-cache\-clusters](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-clusters.html)\.

### Finding the Endpoints for Replication Groups \(AWS CLI\)<a name="Endpoints.Find.CLI.ReplGroups"></a>

You can use the AWS CLI to discover the endpoints for a replication group and its clusters with the `describe-replication-groups` command\. The command returns the replication group's primary endpoint and a list of all the clusters \(nodes\) in the replication group with their endpoints, along with the reader endpoint\. 

The following operation retrieves the primary endpoint and reader endpoint for the replication group `myreplgroup`\. Use the primary endpoint for all write operations\. 

```
aws elasticache describe-replication-groups \
    --replication-group-id myreplgroup
```

For Windows:

```
aws elasticache describe-replication-groups ^
    --replication-group-id myreplgroup
```

Output from this operation should look something like this \(JSON format\)\.

```
{
   "ReplicationGroups": [
     {
       "Status": "available", 
       "Description": "test", 
       "NodeGroups": [
         {
            "Status": "available", 
               "NodeGroupMembers": [
                  {
                     "CurrentRole": "primary", 
                     "PreferredAvailabilityZone": "us-west-2a", 
                     "CacheNodeId": "0001", 
                     "ReadEndpoint": {
                        "Port": 6379, 
                        "Address": "myreplgroup-001.1abc4d.0001.usw2.cache.amazonaws.com"
                     }, 
                     "CacheClusterId": "myreplgroup-001"
                  }, 
                  {
                     "CurrentRole": "replica", 
                     "PreferredAvailabilityZone": "us-west-2b", 
                     "CacheNodeId": "0001", 
                     "ReadEndpoint": {
                        "Port": 6379, 
                        "Address": "myreplgroup-002.1abc4d.0001.usw2.cache.amazonaws.com"
                     }, 
                     "CacheClusterId": "myreplgroup-002"
                  }, 
                  {
                     "CurrentRole": "replica", 
                     "PreferredAvailabilityZone": "us-west-2c", 
                     "CacheNodeId": "0001", 
                     "ReadEndpoint": {
                        "Port": 6379, 
                        "Address": "myreplgroup-003.1abc4d.0001.usw2.cache.amazonaws.com"
                     }, 
                     "CacheClusterId": "myreplgroup-003"
                  }
               ], 
               "NodeGroupId": "0001", 
               "PrimaryEndpoint": {
                  "Port": 6379, 
                  "Address": "myreplgroup.1abc4d.ng.0001.usw2.cache.amazonaws.com"
               },
               "ReaderEndpoint": {
                  "Port": 6379, 
                  "Address": "myreplgroup-ro.1abc4d.ng.0001.usw2.cache.amazonaws.com"
               }
            }
         ], 
         "ReplicationGroupId": "myreplgroup", 
         "AutomaticFailover": "enabled", 
         "SnapshottingClusterId": "myreplgroup-002", 
         "MemberClusters": [
            "myreplgroup-001", 
            "myreplgroup-002", 
            "myreplgroup-003"
         ], 
         "PendingModifiedValues": {}
      }
   ]
}
```

For more information, see [describe\-replication\-groups](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-replication-groups.html) in the *AWS CLI Command Reference*\.

## Finding Endpoints \(ElastiCache API\)<a name="Endpoints.Find.API"></a>

You can use the Amazon ElastiCache API to discover the endpoints for nodes, clusters, and replication groups\.

**Topics**
+ [Finding Endpoints for Nodes and Clusters \(ElastiCache API\)](#Endpoints.Find.API.Nodes)
+ [Finding Endpoints for Replication Groups \(ElastiCache API\)](#Endpoints.Find.API.ReplGroups)

### Finding Endpoints for Nodes and Clusters \(ElastiCache API\)<a name="Endpoints.Find.API.Nodes"></a>

You can use the ElastiCache API to discover the endpoints for a cluster and its nodes with the `DescribeCacheClusters` action\. For Redis clusters, the command returns the cluster endpoint\.  If you include the optional parameter `ShowCacheNodeInfo`, the action also returns the endpoints of the individual nodes in the cluster\.

**Example**  

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=DescribeCacheClusters
    &CacheClusterId=mycluster
    &ShowCacheNodeInfo=true
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20150202T192317Z
    &Version=2015-02-02
    &X-Amz-Credential=<credential>
```

### Finding Endpoints for Replication Groups \(ElastiCache API\)<a name="Endpoints.Find.API.ReplGroups"></a>

You can use the ElastiCache API to discover the endpoints for a replication group and its clusters with the `DescribeReplicationGroups` action\. The action returns the replication group's primary endpoint and a list of all the clusters in the replication group with their endpoints, along with the reader endpoint\. 

The following operation retrieves the primary endpoint \(PrimaryEndpoint\), reader endpoint \(ReaderEndpoint\) and individual node endpoints \(ReadEndpoint\) for the replication group `myreplgroup`\. Use the primary endpoint for all write operations\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=DescribeReplicationGroups
    &ReplicationGroupId=myreplgroup
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20150202T192317Z
    &Version=2015-02-02
    &X-Amz-Credential=<credential>
```

For more information, see [DescribeReplicationGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReplicationGroups.html)\.  