# Finding Your ElastiCache Endpoints<a name="Endpoints"></a>

Your application connects to your cluster using endpoints\. An endpoint is a node or cluster's unique address\.

**Which endpoints to use**

+ **Memcached cluster**, If you use Automatic Discovery, you can use the cluster's *configuration endpoint* to configure your Memcached client\. This means you must use a client that supports Automatic Discovery\.

  If you don't use Automatic Discovery, you must configure your client to use the individual node endpoints for reads and writes\. You must also keep track of them as you add and remove nodes\.

   

+ **Redis standalone node**, use the node's endpoint for both read and write operations\.

   

+ **Redis \(cluster mode disabled\) clusters**, use the *Primary Endpoint* for all write operations\. Use the individual *Node Endpoints* for read operations \(In the API/CLI these are referred to as Read Endpoints\)\.

   

+ **Redis \(cluster mode enabled\) clusters**, use the cluster's *Configuration Endpoint* for all operations\. You must use a client that supports Redis Cluster \(Redis 3\.2\)\. You can still read from individual node endpoints \(In the API/CLI these are referred to as Read Endpoints\)\.

   

The following sections guide you through discovering the endpoints you'll need for the engine you're running\.



## Finding a Memcached Cluster's Endpoints \(Console\)<a name="Endpoints.Find.Memcached"></a>

All Memcached endpoints are read/write endpoints\. To connect to nodes in a Memcached cluster your application can use either the endpoints for each node, or the cluster's configuration endpoint along with Automatic Discovery\. To use Automatic Discovery you must use a client that supports Automatic Discovery\.

When using Automatic Discovery, your client application connects to your Memcached cluster using the configuration endpoint\. As you scale your cluster by adding or removing nodes, your application will automatically "know" all the nodes in the cluster and be able to connect to any of them\. Without Automatic Discovery your application would have to do this, or you'd have to manually update endpoints in your application each time you added or removed a node\. For additional information on Automatic Discovery, see [Node Auto Discovery \(Memcached\)](AutoDiscovery.md)\.

The following procedure demonstrates how to find and copy a cluster's configuration endpoint or any of the node endpoints using the ElastiCache console\.

**To find and copy the endpoints for a Memcached cluster \(console\)**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Memcached**\.

   The cache clusters screen will appear with a list of Memcached clusters\.

1. Find the Memcached cluster you want the endpoints for\.

   If all you want is the configuration endpoint, you're done\. The configuration endpoint is in the **Configuration Endpoint** column and looks something like this, `clusterName.xxxxxx.cfg.usw2.cache.amazonaws.com:port`\.

   If you want to also see the individual node endpoints or copy any of the endpoints to your clipboard, choose **Copy Node Endpoint**\.  
![\[Image: Screen showing endpoints for a Memcached cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-Endpoints-Memcached.png)

   *Endpoints for a Memcached cluster*

1. To copy an endpoint to your clipboard:

   1. On the **Copy Node Endpoint** screen, highlight the endpoint you want to copy\.

   1. Right–click the highlighted endpoint, and then choose **Copy** from the context menu\.

   The highlighted endpoint is now copied to your clipboard\.

Configuration and node endpoints look very similar\. The differences are highlighted with **bold** following\.

```
myclustername.xxxxxx.cfg.usw2.cache.amazonaws.com:port   # configuration endpoint contains "cfg"
myclustername.xxxxxx.0001.usw2.cache.amazonaws.com:port  # node endpoint for node 0001
```

**Important**  
If you choose to create a CNAME for your Memcached configuration endpoint, in order for your automatic discovery client to recognize the CNAME as a configuration endpoint, you must include `.cfg.` in the CNAME\. 

## Finding a Redis \(cluster mode disabled\) Cluster's Endpoints \(Console\)<a name="Endpoints.Find.Redis"></a>

If a Redis \(cluster mode disabled\) cluster has only one node, the node's endpoint is used for both reads and writes\. If a Redis \(cluster mode disabled\) cluster has multiple nodes, there are two types of endpoints, the Primary endpoint which always points to whichever node is serving as Primary, and the node endpoints\. The Primary endpoint is used for writes\. The node endpoints are used for reads\.

**To find a Redis \(cluster mode disabled\) cluster's endpoints**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Redis**\.

   The clusters screen will appear with a list of Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\) clusters\.

1. To find the cluster's Primary endpoint, choose the box to the left of cluster's name\.

   If there is only one node in the cluster, there is no primary endpoint and you can continue at the next step\.  
![\[Image: Primary endpoint for a Redis (cluster mode disabled) cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-Endpoints-Redis-Primary.png)

   *Primary endpoint for a Redis \(cluster mode disabled\) cluster*

1. If the Redis \(cluster mode disabled\) cluster has replica nodes, you can find the cluster's replica node endpoints by choosing the cluster's name\.

   The nodes screen appears with each node in the cluster, primary and replicas, listed with its endpoint\.  
![\[Image: Node endpoints for a Redis (cluster mode disabled) cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-Endpoints-Redis-Node.png)

   *Node endpoints for a Redis \(cluster mode disabled\) cluster*

1. To copy an endpoint to your clipboard:

   1. One endpoint at a time, find then highlight the endpoint you want to copy\.

   1. Right\-click the highlighted endpoint, then choose **Copy** from the context menu\.

   The highlighted endpoint is now copied to your clipboard\.

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

## Finding a Redis \(cluster mode enabled\) Cluster's Endpoints \(Console\)<a name="Endpoints.Find.RedisCluster"></a>

Use the *Configuration Endpoint* for both read and write operations\. Redis determines which of the cluster's node to access\.

The following procedure demonstrates how to find and copy Redis \(cluster mode enabled\) cluster endpoints\.

**To find the configuration endpoint for a Redis \(cluster mode enabled\) cluster**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Redis**\.

   A list of clusters running any version of Redis appears\.

1. From the list of clusters, choose the box to the left of a cluster running "Clustered Redis"\.

   The screen expands showing details about the selected cluster\.

1. Locate the *Configuration endpoint*\.  
![\[Image: Configuration endpoint for a Redis (cluster mode enabled) cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-Endpoints-RedisCluster-Configuration.png)

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
![\[Image: Node endpoints for a Redis (cluster mode enabled) cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-Endpoints-RedisCluster-Node.png)

   *Node endpoints for a Redis \(cluster mode enabled\) cluster*

**To copy an endpoint to your clipboard**

1. Find the endpoint you want to copy using one of the preceeding procedures\.

1. Highlight the endpoint that you want to copy\.

1. Right\-click the highlighted endpoint and choose **Copy** from the context menu\.

   The highlighted endpoint is now copied to your clipboard\.

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

You can use the AWS CLI for Amazon ElastiCache to discover the endpoints for nodes, clusters, and replication groups


+ [Finding Endpoints for Nodes and Clusters \(AWS CLI\)](#Endpoints.Find.CLI.Nodes)
+ [Finding the Endpoints for Replication Groups \(AWS CLI\)](#Endpoints.Find.CLI.ReplGroups)

### Finding Endpoints for Nodes and Clusters \(AWS CLI\)<a name="Endpoints.Find.CLI.Nodes"></a>

You can use the AWS CLI to discover the endpoints for a cluster and its nodes with the `describe-cache-clusters` command\. For Redis clusters, the command returns the cluster endpoint\. For Memcached clusters, the command returns the configuration endpoint\. If you include the optional parameter `--show-cache-node-info`, the command will also return the endpoints of the individual nodes in the cluster\.

The following command retrieves the configuration endpoint \(`ConfigurationEndpoint`\) and individual node endpoints \(`Endpoint`\) for the Memcached cluster *mycluster*\.

For Linux, macOS, or Unix:

```
aws elasticache describe-cache-clusters \
    --cache-cluster-id mycluster \
    --show-cache-node-info
```

For Windows:

```
aws elasticache describe-cache-clusters ^
    --cache-cluster-id mycluster ^
    --show-cache-node-info
```

Output from the above operation should look something like this \(JSON format\)\.

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
                "Address": "mycluster.1abc4d.0001.usw2.cache.amazonaws.com"
             }, 
                "CacheNodeStatus": "available", 
                "ParameterGroupStatus": "in-sync", 
                "CacheNodeCreateTime": "2016-09-22T21:30:29.967Z", 
                "CustomerAvailabilityZone": "us-west-2b"
          }, 
          {
             "CacheNodeId": "0002", 
             "Endpoint": {
                "Port": 11211, 
                "Address": "mycluster.1abc4d.0002.usw2.cache.amazonaws.com"
             }, 
                "CacheNodeStatus": "available", 
                "ParameterGroupStatus": "in-sync", 
                "CacheNodeCreateTime": "2016-09-22T21:30:29.967Z", 
                "CustomerAvailabilityZone": "us-west-2b"
          }, 
          {
                "CacheNodeId": "0003", 
                "Endpoint": {
                   "Port": 11211, 
                   "Address": "mycluster.1abc4d.0003.usw2.cache.amazonaws.com"
                }, 
                   "CacheNodeStatus": "available", 
                   "ParameterGroupStatus": "in-sync", 
                   "CacheNodeCreateTime": "2016-09-22T21:30:29.967Z", 
                   "CustomerAvailabilityZone": "us-west-2b"
          }
       ], 
       "CacheParameterGroup": {
       "CacheNodeIdsToReboot": [], 
       "CacheParameterGroupName": "default.memcached1.4", 
       "ParameterApplyStatus": "in-sync"
            }, 
            "CacheClusterId": "mycluster", 
            "PreferredAvailabilityZone": "us-west-2b", 
            "ConfigurationEndpoint": {
                "Port": 11211, 
                "Address": "mycluster.1abc4d.cfg.usw2.cache.amazonaws.com"
            }, 
            "CacheSecurityGroups": [], 
            "CacheClusterCreateTime": "2016-09-22T21:30:29.967Z", 
            "AutoMinorVersionUpgrade": true, 
            "CacheClusterStatus": "available", 
            "NumCacheNodes": 3, 
            "ClientDownloadLandingPage": "https://console.aws.amazon.com/elasticache/home#client-download:", 
            "CacheSubnetGroupName": "default", 
            "EngineVersion": "1.4.24", 
            "PendingModifiedValues": {}, 
            "PreferredMaintenanceWindow": "mon:09:00-mon:10:00", 
            "CacheNodeType": "cache.m4.large"
        }
    ]   
}
```

**Important**  
If you choose to create a CNAME for your Memcached configuration endpoint, in order for your PHP client to recognize the CNAME as a configuration endpoint, you must include `.cfg.` in the CNAME\. For example, `mycluster.cfg.local` in your php\.ini file for the `session.save_path` parameter\.

For more information, go to the topic [describe\-cache\-clusters](http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-clusters.html)\.

### Finding the Endpoints for Replication Groups \(AWS CLI\)<a name="Endpoints.Find.CLI.ReplGroups"></a>

You can use the AWS CLI to discover the endpoints for a replication group and its clusters with the `describe-replication-groups` command\. The command returns the replication group's primary endpoint and a list of all the clusters in the replication group with their endpoints\. 

The following operation retrieves the primary endpoint \(PrimaryEndpoint\) and individual node endpoints \(ReadEndpoint\) for the replication group `myreplgroup`\. Use the primary endpoint for all write operations and the individual node endpoints for all read operations\.

For Linux, macOS, or Unix:

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

For more information, see [describe\-replication\-groups](http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-replication-groups.html) in the *AWS Command Line Interface Reference*\. 

## Finding Endpoints \(ElastiCache API\)<a name="Endpoints.Find.API"></a>

You can use the Amazon ElastiCache API to discover the endpoints for nodes, clusters, and replication groups


+ [Finding Endpoints for Nodes and Clusters \(ElastiCache API\)](#Endpoints.Find.API.Nodes)
+ [Finding Endpoints for Replication Groups \(ElastiCache API\)](#Endpoints.Find.API.ReplGroups)

### Finding Endpoints for Nodes and Clusters \(ElastiCache API\)<a name="Endpoints.Find.API.Nodes"></a>

You can use the ElastiCache API to discover the endpoints for a cluster and its nodes with the `DescribeCacheClusters` action\. For Redis clusters, the action returns the cluster endpoint\. For Memcached clusters, the action returns the configuration endpoint\. If you include the optional parameter `ShowCacheNodeInfo`, the action also returns the endpoints of the individual nodes in the cluster\.

The following command retrieves the configuration endpoint \(`ConfigurationEndpoint`\) and individual node endpoints \(`Endpoint`\) for the Memcached cluster *mycluster*\.

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

**Important**  
If you choose to create a CNAME for your Memcached configuration endpoint, in order for your PHP client to recognize the CNAME as a configuration endpoint, you must include `.cfg.` in the CNAME\. For example, `mycluster.cfg.local` in your php\.ini file for the `session.save_path` parameter\.

### Finding Endpoints for Replication Groups \(ElastiCache API\)<a name="Endpoints.Find.API.ReplGroups"></a>

You can use the ElastiCache API to discover the endpoints for a replication group and its clusters with the `DescribeReplicationGroups` action\. The action returns the replication group's primary endpoint and a list of all the clusters in the replication group with their endpoints\. 

The following operation retrieves the primary endpoint \(PrimaryEndpoint\) and individual node endpoints \(ReadEndpoint\) for the replication group `myreplgroup`\. Use the primary endpoint for all write operations and the individual node endpoints for all read operations\.

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

For more information, see [DescribeReplicationGroups](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReplicationGroups.html)\.  