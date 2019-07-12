# Scaling Up Single\-Node Redis \(cluster mode disabled\) Clusters<a name="Scaling.RedisStandalone.ScaleUp"></a>

When you scale up a single\-node Redis cluster, ElastiCache performs the following process, whether you use the ElastiCache console, the AWS CLI, or the ElastiCache API\.

1. A new cache cluster with the new node type is spun up in the same Availability Zone as the existing cache cluster\.

1. The cache data in the existing cache cluster is copied to the new cache cluster\. How long this process takes depends upon your node type and how much data is in the cache cluster\.

1. Reads and writes are now served using the new cache cluster\. Because the new cache cluster's endpoints are the same as they were for the old cache cluster, you do not need to update the endpoints in your application\. You will notice a brief interruption of reads and writes from the primary node while the DNS entry is updated\.

1. ElastiCache deletes the old cache cluster\.

As shown in the following table, your Redis scale\-up operation is blocked if you have an engine upgrade scheduled for the next maintenance window\. For more information on Maintenance Windows, see [Managing Maintenance](maintenance-window.md)\.


**Blocked Redis operations**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Scaling.RedisStandalone.ScaleUp.html)

If you have a pending operation that is blocking you, you can do one of the following\.
+ Schedule your Redis scale\-up operation for the next maintenance window by clearing the **Apply immediately** check box \(CLI use: `--no-apply-immediately`, API use: `ApplyImmediately=false`\)\.
+ Wait until your next maintenance window \(or after\) to perform your Redis scale up operation\.
+ Add the Redis engine upgrade to this cache cluster modification with the **Apply Immediately** check box chosen \(CLI use: `--apply-immediately`, API use: `ApplyImmediately=true`\)\. This unblocks your scale up operation by causing the engine upgrade to be performed immediately\.

You can scale up a single\-node Redis \(cluster mode disabled\) cluster using the ElastiCache console, the AWS CLI, or ElastiCache API\.

**Important**  
If your parameter group uses `reserved-memory` to set aside memory for Redis overhead, before you begin scaling be sure that you have a custom parameter group that reserves the correct amount of memory for your new node type\. Alternatively, you can modify a custom parameter group so that it uses `reserved-memory-percent` and use that parameter group for your new cluster\.  
If you're using `reserved-memory-percent`, doing this is not necessary\.   
For more information, see [Managing Reserved Memory](redis-memory-management.md)\.

## Scaling Up Single\-Node Redis \(cluster mode disabled\) Clusters \(Console\)<a name="Scaling.RedisStandalone.ScaleUp.CON"></a>

The following procedure describes how to scale up a single\-node Redis cluster using the ElastiCache Management Console\.

**To scale up a single\-node Redis cluster \(console\)**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Redis**\.

1. From the list of clusters, choose the cluster you want to scale up \(it must be running the Redis engine, not the Clustered Redis engine\)\. 

1. Choose **Modify**\.

1. In the **Modify Cluster** wizard:

   1. Choose the node type you want to scale to from the **Node type** list\.

      The list identifies all the node types you can scale up to\.

   1. If you're using `reserved-memory` to manage your memory, from the **Parameter Group** list, choose the custom parameter group that reserves the correct amount of memory for your new node type\.

1. If you want to perform the scale up process right away, choose the **Apply immediately** box\. If the **Apply immediately** box is not chosen, the scale\-up process is performed during this cluster's next maintenance window\.

1. Choose **Modify**\.

   If you chose **Apply immediately** in the previous step, the cluster's status changes to *modifying*\. When the status changes to *available*, the modification is complete and you can begin using the new cluster\.

## Scaling Up Single\-Node Redis Cache Clusters \(AWS CLI\)<a name="Scaling.RedisStandalone.ScaleUp.CLI"></a>

The following procedure describes how to scale up a single\-node Redis cache cluster using the AWS CLI\.

**To scale up a single\-node Redis cache cluster \(AWS CLI\)**

1. Determine the node types you can scale up to by running the AWS CLI `list-allowed-node-type-modifications` command with the following parameter\.
   + `--cache-cluster-id` – Name of the single\-node Redis cache cluster you want to scale up\.

   For Linux, macOS, or Unix:

   ```
   aws elasticache list-allowed-node-type-modifications \
   	    --cache-cluster-id my-cache-cluster-id
   ```

   For Windows:

   ```
   aws elasticache list-allowed-node-type-modifications ^
   	    --cache-cluster-id my-cache-cluster-id
   ```

   Output from the above command looks something like this \(JSON format\)\.

   ```
   {
   	    "ScaleUpModifications": [
   	        "cache.m3.2xlarge", 
   	        "cache.m3.large", 
   	        "cache.m3.xlarge", 
   	        "cache.m4.10xlarge", 
   	        "cache.m4.2xlarge", 
   	        "cache.m4.4xlarge", 
   	        "cache.m4.large", 
   	        "cache.m4.xlarge", 
   	        "cache.r3.2xlarge", 
   	        "cache.r3.4xlarge", 
   	        "cache.r3.8xlarge", 
   	        "cache.r3.large", 
   	        "cache.r3.xlarge"
   	    ]
   	}
   ```

   For more information, see [list\-allowed\-node\-type\-modifications](https://docs.aws.amazon.com/cli/latest/reference/elasticache/list-allowed-node-type-modifications.html) in the *AWS CLI Reference*\.

1. Modify your existing cache cluster specifying the cache cluster to scale up and the new, larger node type, using the AWS CLI `modify-cache-cluster` command and the following parameters\.
   + `--cache-cluster-id` – The name of the cache cluster you are scaling up\. 
   + `--cache-node-type` – The new node type you want to scale the cache cluster up to\. This value must be one of the node types returned by the `list-allowed-node-type-modifications` command in step 1\.
   + `--cache-parameter-group-name` – \[Optional\] Use this parameter if you are using `reserved-memory` to manage your cluster's reserved memory\. Specify a custom cache parameter group that reserves the correct amount of memory for your new node type\. If you are using `reserved-memory-percent` you can omit this parameter\.
   + `--apply-immediately` – Causes the scale\-up process to be applied immediately\. To postpone the scale\-up process to the cluster's next maintenance window, use the `--no-apply-immediately` parameter\.

   For Linux, macOS, or Unix:

   ```
   aws elasticache modify-cache-cluster \
   	    --cache-cluster-id my-redis-cache-cluster \
   	    --cache-node-type cache.m2.xlarge \
   	    --cache-parameter-group-name redis32-m2-xl \
   	    --apply-immediately
   ```

   For Windows:

   ```
   aws elasticache modify-cache-cluster ^
   	    --cache-cluster-id my-redis-cache-cluster ^
   	    --cache-node-type cache.m2.xlarge ^
   	    --cache-parameter-group-name redis32-m2-xl ^
   	    --apply-immediately
   ```

   Output from the above command looks something like this \(JSON format\)\.

   ```
   {
   	    "CacheCluster": {
   	        "Engine": "redis", 
   	        "CacheParameterGroup": {
   	            "CacheNodeIdsToReboot": [], 
   	            "CacheParameterGroupName": "default.redis3.2", 
   	            "ParameterApplyStatus": "in-sync"
   	        }, 
   	        "SnapshotRetentionLimit": 1, 
   	        "CacheClusterId": "my-redis-cache-cluster", 
   	        "CacheSecurityGroups": [], 
   	        "NumCacheNodes": 1, 
   	        "SnapshotWindow": "00:00-01:00", 
   	        "CacheClusterCreateTime": "2017-02-21T22:34:09.645Z", 
   	        "AutoMinorVersionUpgrade": true, 
   	        "CacheClusterStatus": "modifying", 
   	        "PreferredAvailabilityZone": "us-west-2a", 
   	        "ClientDownloadLandingPage": "https://console.aws.amazon.com/elasticache/home#client-download:", 
   	        "CacheSubnetGroupName": "default", 
   	        "EngineVersion": "3.2.4", 
   	        "PendingModifiedValues": {
   	            "CacheNodeType": "cache.m3.2xlarge"
   	        }, 
   	        "PreferredMaintenanceWindow": "tue:11:30-tue:12:30", 
   	        "CacheNodeType": "cache.m3.medium"
   	    }
   	}
   ```

   For more information, see [modify\-cache\-cluster](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-cluster.html) in the *AWS CLI Reference*\.

1. If you used the `--apply-immediately`, check the status of the new cache cluster using the AWS CLI `describe-cache-clusters` command with the following parameter\. When the status changes to *available*, you can begin using the new, larger cache cluster\.
   + `--cache-cache cluster-id` – The name of your single\-node Redis cache cluster\. Use this parameter to describe a particular cache cluster rather than all cache clusters\.

   ```
   aws elasticache describe-cache-clusters --cache-cluster-id my-redis-cache-cluster
   ```

   For more information, see [describe\-cache\-clusters](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-clusters.html) in the *AWS CLI Reference*\.

## Scaling Up Single\-Node Redis Cache Clusters \(ElastiCache API\)<a name="Scaling.RedisStandalone.ScaleUp.API"></a>

The following procedure describes how to scale up a single\-node Redis cache cluster using the ElastiCache API\.

**To scale up a single\-node Redis cache cluster \(ElastiCache API\)**

1. Determine the node types you can scale up to by running the ElastiCache API `ListAllowedNodeTypeModifications` action with the following parameter\.
   + `CacheClusterId` – The name of the single\-node Redis cache cluster you want to scale up\.

   ```
   https://elasticache.us-west-2.amazonaws.com/
   	   ?Action=ListAllowedNodeTypeModifications
   	   &CacheClusterId=MyRedisCacheCluster
   	   &Version=2015-02-02
   	   &SignatureVersion=4
   	   &SignatureMethod=HmacSHA256
   	   &Timestamp=20150202T192317Z
   	   &X-Amz-Credential=<credential>
   ```

   For more information, see [ListAllowedNodeTypeModifications](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ListAllowedNodeTypeModifications.html) in the *Amazon ElastiCache API Reference*\.

1. Modify your existing cache cluster specifying the cache cluster to scale up and the new, larger node type, using the `ModifyCacheCluster` ElastiCache API action and the following parameters\.
   + `CacheClusterId` – The name of the cache cluster you are scaling up\.
   + `CacheNodeType` – The new, larger node type you want to scale the cache cluster up to\. This value must be one of the node types returned by the `ListAllowedNodeTypeModifications` action in step 1\.
   + `CacheParameterGroupName` – \[Optional\] Use this parameter if you are using `reserved-memory` to manage your cluster's reserved memory\. Specify a custom cache parameter group that reserves the correct amount of memory for your new node type\. If you are using `reserved-memory-percent` you can omit this parameter\.
   + `ApplyImmediately` – Set to `true` to cause the scale\-up process to be performed immediately\. To postpone the scale\-up process to the cluster's next maintenance window, use `ApplyImmediately``=false`\.

   ```
   https://elasticache.us-west-2.amazonaws.com/
   	   ?Action=ModifyCacheCluster
   	   &ApplyImmediately=true
   	   &CacheClusterId=MyRedisCacheCluster
   	   &CacheNodeType=cache.m2.xlarge
   	   &CacheParameterGroupName redis32-m2-xl
   	   &Version=2015-02-02
   	   &SignatureVersion=4
   	   &SignatureMethod=HmacSHA256
   	   &Timestamp=20150202T192317Z
   	   &X-Amz-Credential=<credential>
   ```

   For more information, see [ModifyCacheCluster](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html) in the *Amazon ElastiCache API Reference*\.

1. If you used `ApplyImmediately``=true`, check the status of the new cache cluster using the ElastiCache API `DescribeCacheClusters` action with the following parameter\. When the status changes to *available*, you can begin using the new, larger cache cluster\.
   + `CacheClusterId` – The name of your single\-node Redis cache cluster\. Use this parameter to describe a particular cache cluster rather than all cache clusters\.

   ```
   https://elasticache.us-west-2.amazonaws.com/
   	   ?Action=DescribeCacheClusters
   	   &CacheClusterId=MyRedisCacheCluster
   	   &Version=2015-02-02
   	   &SignatureVersion=4
   	   &SignatureMethod=HmacSHA256
   	   &Timestamp=20150202T192317Z
   	   &X-Amz-Credential=<credential>
   ```

   For more information, see [DescribeCacheClusters](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheClusters.html) in the *Amazon ElastiCache API Reference*\.