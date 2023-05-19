# Online scaling up<a name="redis-cluster-vertical-scaling-scaling-up"></a>

**Topics**
+ [Scaling up Redis cache clusters \(Console\)](#redis-cluster-vertical-scaling-console)
+ [Scaling up Redis cache clusters \(AWS CLI\)](#Scaling.RedisStandalone.ScaleUp.CLI)
+ [Scaling up Redis cache clusters \(ElastiCache API\)](#VeticalScaling.RedisReplGrps.ScaleUp.API)

## Scaling up Redis cache clusters \(Console\)<a name="redis-cluster-vertical-scaling-console"></a>

The following procedure describes how to scale up a Redis cluster using the ElastiCache Management Console\. During this process, your Redis cluster will continue to serve requests with minimal downtime\.

**To scale up a Redis cluster \(console\)**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Redis clusters**\.

1. From the list of clusters, choose the cluster\. 

1. Choose **Modify**\.

1. In the **Modify Cluster** wizard:

   1. Choose the node type you want to scale to from the **Node type** list\. To scale up, select a node type larger than your existing node\. 

1. If you want to perform the scale\-up process right away, choose the **Apply immediately** box\. If the **Apply immediately** box is not chosen, the scale\-up process is performed during this cluster's next maintenance window\.

1. Choose **Modify**\.

   If you chose **Apply immediately** in the previous step, the cluster's status changes to *modifying*\. When the status changes to *available*, the modification is complete and you can begin using the new cluster\.

## Scaling up Redis cache clusters \(AWS CLI\)<a name="Scaling.RedisStandalone.ScaleUp.CLI"></a>

The following procedure describes how to scale up a Redis cache cluster using the AWS CLI\. During this process, your Redis cluster will continue to serve requests with minimal downtime\.

**To scale up a Redis cache cluster \(AWS CLI\)**

1. Determine the node types you can scale up to by running the AWS CLI `list-allowed-node-type-modifications` command with the following parameter\.

   For Linux, macOS, or Unix:

   ```
   aws elasticache list-allowed-node-type-modifications \
   	    --replication-group-id my-replication-group-id
   ```

   For Windows:

   ```
   aws elasticache list-allowed-node-type-modifications ^
   	    --replication-group-id my-replication-group-id
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
   	       "ScaleDownModifications": [
   	        "cache.t2.micro", 
   	        "cache.t2.small ", 
   	        "cache.t2.medium",
   	       	"cache.t1.small "
   	    ], 
   }
   ```

   For more information, see [list\-allowed\-node\-type\-modifications](https://docs.aws.amazon.com/cli/latest/reference/elasticache/list-allowed-node-type-modifications.html) in the *AWS CLI Reference*\.

1. Modify your replication group to scale up to the new, larger node type, using the AWS CLI `modify-replication-group` command and the following parameters\.
   + `--replication-group-id` – The name of the replication group you are scaling up to\. 
   + `--cache-node-type` – The new node type you want to scale the cache cluster\. This value must be one of the node types returned by the `list-allowed-node-type-modifications` command in step 1\.
   + `--cache-parameter-group-name` – \[Optional\] Use this parameter if you are using `reserved-memory` to manage your cluster's reserved memory\. Specify a custom cache parameter group that reserves the correct amount of memory for your new node type\. If you are using `reserved-memory-percent` you can omit this parameter\.
   + `--apply-immediately` – Causes the scale\-up process to be applied immediately\. To postpone the scale\-up process to the cluster's next maintenance window, use the `--no-apply-immediately` parameter\.

   For Linux, macOS, or Unix:

   ```
   aws elasticache modify-replication-group  \
   	    --replication-group-id my-redis-cluster \
   	    --cache-node-type cache.m3.xlarge \	    
   	    --apply-immediately
   ```

   For Windows:

   ```
   aws elasticache modify-replication-group ^
   	    --replication-group-id my-redis-cluster ^
   	    --cache-node-type cache.m3.xlarge ^	   
   	    --apply-immediately
   ```

   Output from the above command looks something like this \(JSON format\)\.

   ```
   {
   		"ReplicationGroup": {
           "Status": "modifying",
           "Description": "my-redis-cluster",
           "NodeGroups": [
               {
                   "Status": "modifying",
                   "Slots": "0-16383",
                   "NodeGroupId": "0001",
                   "NodeGroupMembers": [
                       {
                           "PreferredAvailabilityZone": "us-east-1f",
                           "CacheNodeId": "0001",
                           "CacheClusterId": "my-redis-cluster-0001-001"
                       },
                       {
                           "PreferredAvailabilityZone": "us-east-1d",
                           "CacheNodeId": "0001",
                           "CacheClusterId": "my-redis-cluster-0001-002"
                       }
                   ]
               }
           ],
           "ConfigurationEndpoint": {
               "Port": 6379,
               "Address": "my-redis-cluster.r7gdfi.clustercfg.use1.cache.amazonaws.com"
           },
           "ClusterEnabled": true,
           "ReplicationGroupId": "my-redis-cluster",
           "SnapshotRetentionLimit": 1,
           "AutomaticFailover": "enabled",
           "SnapshotWindow": "07:30-08:30",
           "MemberClusters": [
               "my-redis-cluster-0001-001",
               "my-redis-cluster-0001-002"
           ],
           "CacheNodeType": "cache.m3.xlarge",
            "DataTiering": "disabled"
           "PendingModifiedValues": {}
       }
   }
   ```

   For more information, see [modify\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group.html) in the *AWS CLI Reference*\.

1. If you used the `--apply-immediately`, check the status of the cache cluster using the AWS CLI `describe-cache-clusters` command with the following parameter\. When the status changes to *available*, you can begin using the new, larger cache cluster node\.

## Scaling up Redis cache clusters \(ElastiCache API\)<a name="VeticalScaling.RedisReplGrps.ScaleUp.API"></a>

The following process scales your cache cluster from its current node type to a new, larger node type using the ElastiCache API\. During this process, ElastiCache for Redis updates the DNS entries so they point to the new nodes\. Because of this you don't have to update the endpoints in your application\. For Redis 5\.0\.5 and above, you can scale auto failover enabled clusters while the cluster continues to stay online and serve incoming requests\. On version 4\.0\.10 and below, you may notice a brief interruption of reads and writes on previous versions from the primary node while the DNS entry is updated\.\.

The amount of time it takes to scale up to a larger node type varies, depending upon your node type and the amount of data in your current cache cluster\.

**To scale up a Redis Cache Cluster \(ElastiCache API\)**

1. Determine which node types you can scale up to using the ElastiCache API `ListAllowedNodeTypeModifications` action with the following parameter\.
   + `ReplicationGroupId` – the name of the replication group\. Use this parameter to describe a specific replication group rather than all replication groups\.

   ```
   https://elasticache.us-west-2.amazonaws.com/
   	   ?Action=ListAllowedNodeTypeModifications
   	   &ReplicationGroupId=MyReplGroup
   	   &Version=2015-02-02
   	   &SignatureVersion=4
   	   &SignatureMethod=HmacSHA256
   	   &Timestamp=20150202T192317Z
   	   &X-Amz-Credential=<credential>
   ```

   For more information, see [ListAllowedNodeTypeModifications](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ListAllowedNodeTypeModifications.html) in the *Amazon ElastiCache API Reference*\.

1. Scale your current replication group up to the new node type using the `ModifyReplicationGroup` ElastiCache API action and with the following parameters\.
   + `ReplicationGroupId` – the name of the replication group\.
   + `CacheNodeType` – the new, larger node type of the cache clusters in this replication group\. This value must be one of the instance types returned by the `ListAllowedNodeTypeModifications` action in step 1\.
   + `CacheParameterGroupName` – \[Optional\] Use this parameter if you are using `reserved-memory` to manage your cluster's reserved memory\. Specify a custom cache parameter group that reserves the correct amount of memory for your new node type\. If you are using `reserved-memory-percent` you can omit this parameter\.
   + `ApplyImmediately` – Set to `true` to causes the scale\-up process to be applied immediately\. To postpone the scale\-up process to the next maintenance window, use `ApplyImmediately``=false`\.

   ```
   https://elasticache.us-west-2.amazonaws.com/
   	   ?Action=ModifyReplicationGroup
   	   &ApplyImmediately=true
   	   &CacheNodeType=cache.m3.2xlarge
   	   &CacheParameterGroupName=redis32-m3-2xl
   	   &ReplicationGroupId=myReplGroup
   	   &SignatureVersion=4
   	   &SignatureMethod=HmacSHA256
   	   &Timestamp=20141201T220302Z
   	   &Version=2014-12-01
   	   &X-Amz-Algorithm=&AWS;4-HMAC-SHA256
   	   &X-Amz-Date=20141201T220302Z
   	   &X-Amz-SignedHeaders=Host
   	   &X-Amz-Expires=20141201T220302Z
   	   &X-Amz-Credential=<credential>
   	   &X-Amz-Signature=<signature>
   ```

   For more information, see [ModifyReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyReplicationGroup.html) in the *Amazon ElastiCache API Reference*\.

1. If you used `ApplyImmediately``=true`, monitor the status of the replication group using the ElastiCache API `DescribeReplicationGroups` action with the following parameters\. When the status changes from *modifying* to *available*, you can begin writing to your new, scaled up replication group\.
   + `ReplicationGroupId` – the name of the replication group\. Use this parameter to describe a particular replication group rather than all replication groups\.

   ```
   https://elasticache.us-west-2.amazonaws.com/
   	   ?Action=DescribeReplicationGroups
   	   &ReplicationGroupId=MyReplGroup
   	   &Version=2015-02-02
   	   &SignatureVersion=4
   	   &SignatureMethod=HmacSHA256
   	   &Timestamp=20150202T192317Z
   	   &X-Amz-Credential=<credential>
   ```

   For more information, see [DescribeReplicationGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReplicationGroups.html) in the *Amazon ElastiCache API Reference*\.