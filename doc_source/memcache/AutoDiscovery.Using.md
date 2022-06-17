# Using Auto Discovery<a name="AutoDiscovery.Using"></a>

To begin using Auto Discovery, follow these steps:
+ [Step 1: Obtain the Configuration Endpoint](#AutoDiscovery.Using.ConfigEndpoint)
+ [Step 2: Download the ElastiCache Cluster Client](#AutoDiscovery.Using.ClusterClient)
+ [Step 3: Modify Your Application Program](#AutoDiscovery.Using.ModifyApp)

## Step 1: Obtain the Configuration Endpoint<a name="AutoDiscovery.Using.ConfigEndpoint"></a>

To connect to a cluster, client programs must know the cluster configuration endpoint\. See the topic [Finding a Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Memcached)

You can also use the `aws elasticache describe-cache-clusters` command with the `--show-cache-node-info` parameter:

Whatever method you use to find the cluster's endpoints, the configuration endpoint will always have **\.cfg** in its address\.

**Example Finding endpoints using the AWS CLI for ElastiCache**  
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
                        "Address": "mycluster.fnjyzo.cfg.0001.use1.cache.amazonaws.com"
                    }, 
                    "CacheNodeStatus": "available", 
                    "ParameterGroupStatus": "in-sync", 
                    "CacheNodeCreateTime": "2016-10-12T21:39:28.001Z", 
                    "CustomerAvailabilityZone": "us-east-1e"
                }, 
                {
                    "CacheNodeId": "0002", 
                    "Endpoint": {
                        "Port": 11211, 
                        "Address": "mycluster.fnjyzo.cfg.0002.use1.cache.amazonaws.com"
                    }, 
                    "CacheNodeStatus": "available", 
                    "ParameterGroupStatus": "in-sync", 
                    "CacheNodeCreateTime": "2016-10-12T21:39:28.001Z", 
                    "CustomerAvailabilityZone": "us-east-1a"
                }
            ], 
            "CacheParameterGroup": {
                "CacheNodeIdsToReboot": [], 
                "CacheParameterGroupName": "default.memcached1.4", 
                "ParameterApplyStatus": "in-sync"
            }, 
            "CacheClusterId": "mycluster", 
            "PreferredAvailabilityZone": "Multiple", 
            "ConfigurationEndpoint": {
                "Port": 11211, 
                "Address": "mycluster.fnjyzo.cfg.use1.cache.amazonaws.com"
            }, 
            "CacheSecurityGroups": [], 
            "CacheClusterCreateTime": "2016-10-12T21:39:28.001Z", 
            "AutoMinorVersionUpgrade": true, 
            "CacheClusterStatus": "available", 
            "NumCacheNodes": 2, 
            "ClientDownloadLandingPage": "https://console.aws.amazon.com/elasticache/home#client-download:", 
            "CacheSubnetGroupName": "default", 
            "EngineVersion": "1.4.24", 
            "PendingModifiedValues": {}, 
            "PreferredMaintenanceWindow": "sat:06:00-sat:07:00", 
            "CacheNodeType": "cache.r3.large"
        }
    ]
}
```

## Step 2: Download the ElastiCache Cluster Client<a name="AutoDiscovery.Using.ClusterClient"></a>

To take advantage of Auto Discovery, client programs must use the *ElastiCache Cluster Client*\. The ElastiCache Cluster Client is available for Java, PHP, and \.NET and contains all of the necessary logic for discovering and connecting to all of your cache nodes\.

**To download the ElastiCache Cluster Client**

1. Sign in to the AWS Management Console and open the ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the ElastiCache console, choose **ElastiCache Cluster Client** then choose **Download**\.

The source code for the ElastiCache Cluster Client for Java is available at [https://github\.com/amazonwebservices/aws\-elasticache\-cluster\-client\-memcached\-for\-java](https://github.com/amazonwebservices/aws-elasticache-cluster-client-memcached-for-java)\. This library is based on the popular Spymemcached client\. The ElastiCache Cluster Client is released under the Amazon Software License [https://aws\.amazon\.com/asl](https://aws.amazon.com/asl)\. You are free to modify the source code as you see fit\. You can even incorporate the code into other open source Memcached libraries, or into your own client code\.

**Note**  
To use the ElastiCache Cluster Client for PHP, you will first need to install it on your Amazon EC2 instance\. For more information, see [Installing the ElastiCache cluster client for PHP](Appendix.PHPAutoDiscoverySetup.md)\.  
For a TLS supported client download the binary with PHP version 7\.4 or higher\.  
To use the ElastiCache Cluster Client for \.NET, you will first need to install it on your Amazon EC2 instance\. For more information, see [Installing the ElastiCache cluster client for \.NET](Appendix.DotNETAutoDiscoverySetup.md)\.

## Step 3: Modify Your Application Program<a name="AutoDiscovery.Using.ModifyApp"></a>

Modify your application program so that it uses Auto Discovery\. The following sections show how to use the ElastiCache Cluster Client for Java, PHP, and \.NET\. 

**Important**  
When specifying the cluster's configuration endpoint, be sure that the endpoint has "\.cfg" in its address as shown here\. Do not use a CNAME or an endpoint without "\.cfg" in it\.   

```
"mycluster.fnjyzo.cfg.use1.cache.amazonaws.com";
```
 Failure to explicitly specify the cluster's configuration endpoint results in configuring to a specific node\.