# Finding connection endpoints<a name="Endpoints"></a>

Your application connects to your cluster using endpoints\. An endpoint is a node or cluster's unique address\.

**Which endpoints to use**
+ **Memcached cluster**, If you use Automatic Discovery, you can use the cluster's *configuration endpoint* to configure your Memcached client\. This means you must use a client that supports Automatic Discovery\.

  If you don't use Automatic Discovery, you must configure your client to use the individual node endpoints for reads and writes\. You must also keep track of them as you add and remove nodes\.

   

The following sections guide you through discovering the endpoints you'll need for the engine you're running\.

## Finding a Cluster's Endpoints \(Console\)<a name="Endpoints.Find.Memcached"></a>

All Memcached endpoints are read/write endpoints\. To connect to nodes in a Memcached cluster your application can use either the endpoints for each node, or the cluster's configuration endpoint along with Automatic Discovery\. To use Automatic Discovery you must use a client that supports Automatic Discovery\.

When using Automatic Discovery, your client application connects to your Memcached cluster using the configuration endpoint\. As you scale your cluster by adding or removing nodes, your application will automatically "know" all the nodes in the cluster and be able to connect to any of them\. Without Automatic Discovery your application would have to do this, or you'd have to manually update endpoints in your application each time you added or removed a node\. For additional information on Automatic Discovery, see [Automatically identify nodes in your cluster](AutoDiscovery.md)\.

The following procedure demonstrates how to find and copy a cluster's configuration endpoint or any of the node endpoints using the ElastiCache console\.

**To find and copy the endpoints for a Memcached cluster \(console\)**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Memcached**\.

   The cache clusters screen will appear with a list of Memcached clusters\.

1. Find the Memcached cluster you want the endpoints for\.

   If all you want is the configuration endpoint, you're done\. The configuration endpoint is in the **Configuration Endpoint** column and looks something like this, `clusterName.xxxxxx.cfg.usw2.cache.amazonaws.com:port`\.

   If you want to also see the individual node endpoints or copy any of the endpoints to your clipboard, choose **Copy Node Endpoint**\.  
![\[Image: Screen showing endpoints for a Memcached cluster\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-Endpoints-Memcached.png)

   *Endpoints for a Memcached cluster*

1. To copy an endpoint to your clipboard:

   1. On the **Copy Node Endpoint** screen, highlight the endpoint you want to copy\.

   1. Right–click the highlighted endpoint, and then choose **Copy** from the context menu\.

   The highlighted endpoint is now copied to your clipboard\. For information on using the endpoint to connect to a node, see [Connecting to nodes](nodes-connecting.md)\.

Configuration and node endpoints look very similar\. The differences are highlighted with **bold** following\.

```
myclustername.xxxxxx.cfg.usw2.cache.amazonaws.com:port   # configuration endpoint contains "cfg"
myclustername.xxxxxx.0001.usw2.cache.amazonaws.com:port  # node endpoint for node 0001
```

**Important**  
If you choose to create a CNAME for your Memcached configuration endpoint, in order for your automatic discovery client to recognize the CNAME as a configuration endpoint, you must include `.cfg.` in the CNAME\. 

## Finding Endpoints \(AWS CLI\)<a name="Endpoints.Find.CLI"></a>

You can use the AWS CLI for Amazon ElastiCache to discover the endpoints for nodes and clusters\.

**Topics**
+ [Finding Endpoints for Nodes and Clusters \(AWS CLI\)](#Endpoints.Find.CLI.Nodes)

### Finding Endpoints for Nodes and Clusters \(AWS CLI\)<a name="Endpoints.Find.CLI.Nodes"></a>

You can use the AWS CLI to discover the endpoints for a cluster and its nodes with the `describe-cache-clusters` command\. For Memcached clusters, the command returns the configuration endpoint\. If you include the optional parameter `--show-cache-node-info`, the command will also return the endpoints of the individual nodes in the cluster\.

**Example**  
The following command retrieves the configuration endpoint \(`ConfigurationEndpoint`\) and individual node endpoints \(`Endpoint`\) for the Memcached cluster *mycluster*\.  
For Linux, OS X, or Unix:  

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
If you choose to create a CNAME for your Memcached configuration endpoint, in order for your auto discovery client to recognize the CNAME as a configuration endpoint, you must include `.cfg.` in the CNAME\. For example, `mycluster.cfg.local` in your php\.ini file for the `session.save_path` parameter\.

For more information, see the topic [describe\-cache\-clusters](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-clusters.html)\.

## Finding Endpoints \(ElastiCache API\)<a name="Endpoints.Find.API"></a>

You can use the Amazon ElastiCache API to discover the endpoints for nodes and clusters\.

**Topics**
+ [Finding Endpoints for Nodes and Clusters \(ElastiCache API\)](#Endpoints.Find.API.Nodes)

### Finding Endpoints for Nodes and Clusters \(ElastiCache API\)<a name="Endpoints.Find.API.Nodes"></a>

You can use the ElastiCache API to discover the endpoints for a cluster and its nodes with the `DescribeCacheClusters` action\. For Memcached clusters, the command returns the configuration endpoint\. If you include the optional parameter `ShowCacheNodeInfo`, the action also returns the endpoints of the individual nodes in the cluster\.

**Example**  
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
If you choose to create a CNAME for your Memcached configuration endpoint, in order for your auto discovery client to recognize the CNAME as a configuration endpoint, you must include `.cfg.` in the CNAME\. For example, `mycluster.cfg.local` in your php\.ini file for the `session.save_path` parameter\.