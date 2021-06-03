# Scaling down Redis clusters with replicas<a name="Scaling.RedisReplGrps.ScaleDown"></a>

The following sections walk you through how to scale a Redis \(cluster mode disabled\) cache cluster with replica nodes down to a smaller node type\. Ensuring that the new, smaller node type is large enough to accommodate all the data and overhead is very important to success\. For more information, see [Ensuring that you have enough memory to create a Redis snapshot](BestPractices.BGSAVE.md)\.

**Important**  
If your parameter group uses `reserved-memory` to set aside memory for Redis overhead, before you begin scaling be sure that you have a custom parameter group that reserves the correct amount of memory for your new node type\. Alternatively, you can modify a custom parameter group so that it uses `reserved-memory-percent` and use that parameter group for your new cluster\.  
If you're using `reserved-memory-percent`, doing this is not necessary\.   
For more information, see [Managing Reserved Memory](redis-memory-management.md)\.

**Topics**

## Scaling down a Redis cluster with replicas \(Console\)<a name="Scaling.RedisReplGrps.ScaleDown.CON"></a>

The following process scales your Redis cluster with replica nodes to a smaller node type using the ElastiCache console\.

**To scale down a Redis cluster with replica nodes \(console\)**

1. Ensure that the smaller node type is adequate for your data and overhead needs\. 

1. If your parameter group uses `reserved-memory` to set aside memory for Redis overhead, ensure that you have a custom parameter group to set aside the correct amount of memory for your new node type\.

   Alternatively, you can modify your custom parameter group to use `reserved-memory-percent`\. For more information, see [Managing Reserved Memory](redis-memory-management.md)\.

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the list of clusters, choose the cluster you want to scale down\. This cluster must be running the Redis engine and not the Clustered Redis engine\.

1. Choose **Modify**\.

1. In the **Modify Cluster** wizard:

   1. Choose the node type you want to scale down to from the **Node type** list\.

   1. If you're using `reserved-memory` to manage your memory, from the **Parameter Group** list, choose the custom parameter group that reserves the correct amount of memory for your new node type\.

1. If you want to perform the scale\-down process right away, choose the **Apply immediately** check box\. If the **Apply immediately** check box is left not chosen, the scale\-down process is performed during this cluster's next maintenance window\.

1. Choose **Modify**\.

1. When the cluster’s status changes from *modifying* to *available*, your cluster has scaled to the new node type\. There is no need to update the endpoints in your application\.

## Scaling down a Redis replication group \(AWS CLI\)<a name="Scaling.RedisReplGrps.ScaleUp.CLI"></a>

The following process scales your replication group from its current node type to a new, smaller node type using the AWS CLI\. During this process, ElastiCache for Redis updates the DNS entries so they point to the new nodes\. Because of this you don't have to update the endpoints in your application\. For Redis 5\.0\.5 and above, you can scale auto failover enabled clusters while the cluster continues to stay online and serve incoming requests\. On version 5\.0\.4 and below, you may notice a brief interruption of reads and writes on previous versions from the primary node while the DNS entry is updated\.\.

However, reads from the read replica cache clusters continue uninterrupted\.

The amount of time it takes to scale down to a smaller node type varies, depending upon your node type and the amount of data in your current cache cluster\.

**To scale down a Redis Replication Group \(AWS CLI\)**

1. Determine which node types you can scale down to by running the AWS CLI `list-allowed-node-type-modifications` command with the following parameter\.
   + `--replication-group-id` – the name of the replication group\. Use this parameter to describe a particular replication group rather than all replication groups\.

   For Linux, OS X, or Unix:

   ```
   aws elasticache list-allowed-node-type-modifications \
   	    --replication-group-id my-repl-group
   ```

   For Windows:

   ```
   aws elasticache list-allowed-node-type-modifications ^
   	    --replication-group-id my-repl-group
   ```

   Output from this operation looks something like this \(JSON format\)\.

   ```
   {
   	    "ScaleDownModifications": [
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

1. Scale your current replication group up to the new node type using the AWS CLI `modify-replication-group` command with the following parameters\.
   + `--replication-group-id` – the name of the replication group\.
   + `--cache-node-type` – the new, smaller node type of the cache clusters in this replication group\. This value must be one of the instance types returned by the `list-allowed-node-type-modifications` command in step 1\.
   + `--cache-parameter-group-name` – \[Optional\] Use this parameter if you are using `reserved-memory` to manage your cluster's reserved memory\. Specify a custom cache parameter group that reserves the correct amount of memory for your new node type\. If you are using `reserved-memory-percent` you can omit this parameter\.
   + `--apply-immediately` – Causes the scale\-up process to be applied immediately\. To postpone the scale\-up operation to the next maintenance window, use `--no-apply-immediately`\.

   For Linux, OS X, or Unix:

   ```
   aws elasticache modify-replication-group \
   	    --replication-group-id my-repl-group \
   	    --cache-node-type cache.t2.small  \
   	    --cache-parameter-group-name redis32-m3-2xl \
   	    --apply-immediately
   ```

   For Windows:

   ```
   aws elasticache modify-replication-group ^
   	    --replication-group-id my-repl-group ^
   	    --cache-node-type cache.t2.small  ^
   	    --cache-parameter-group-name redis32-m3-2xl \
   	    --apply-immediately
   ```

   Output from this command looks something like this \(JSON format\)\.

   ```
   {"ReplicationGroup": {
   	        "Status": "available", 
   	        "Description": "Some description", 
   	        "NodeGroups": [
   	            {
   	                "Status": "available", 
   	                "NodeGroupMembers": [
   	                    {
   	                        "CurrentRole": "primary", 
   	                        "PreferredAvailabilityZone": "us-west-2b", 
   	                        "CacheNodeId": "0001", 
   	                        "ReadEndpoint": {
   	                            "Port": 6379, 
   	                            "Address": "my-repl-group-001.8fdx4s.0001.usw2.cache.amazonaws.com"
   	                        }, 
   	                        "CacheClusterId": "my-repl-group-001"
   	                    }, 
   	                    {
   	                        "CurrentRole": "replica", 
   	                        "PreferredAvailabilityZone": "us-west-2c", 
   	                        "CacheNodeId": "0001", 
   	                        "ReadEndpoint": {
   	                            "Port": 6379, 
   	                            "Address": "my-repl-group-002.8fdx4s.0001.usw2.cache.amazonaws.com"
   	                        }, 
   	                        "CacheClusterId": "my-repl-group-002"
   	                    }
   	                ], 
   	                "NodeGroupId": "0001", 
   	                "PrimaryEndpoint": {
   	                    "Port": 6379, 
   	                    "Address": "my-repl-group.8fdx4s.ng.0001.usw2.cache.amazonaws.com"
   	                }
   	            }
   	        ], 
   	        "ReplicationGroupId": "my-repl-group", 
   	        "SnapshotRetentionLimit": 1, 
   	        "AutomaticFailover": "disabled", 
   	        "SnapshotWindow": "12:00-13:00", 
   	        "SnapshottingClusterId": "my-repl-group-002", 
   	        "MemberClusters": [
   	            "my-repl-group-001", 
   	            "my-repl-group-002", 
   	        ], 
   	        "PendingModifiedValues": {}
   	    }
   	}
   ```

   For more information, see [modify\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-replication-group.html) in the *AWS CLI Reference*\.

1. If you used the `--apply-immediately` parameter, monitor the status of the replication group using the AWS CLI `describe-replication-group` command with the following parameter\. When the status changes from *modifying* to *available*, you can begin writing to your new, scaled down replication group\.
   + `--replication-group-id` – the name of the replication group\. Use this parameter to describe a particular replication group rather than all replication groups\.

   For Linux, OS X, or Unix:

   ```
   aws elasticache describe-replication-group \
   	    --replication-group-id my-replication-group
   ```

   For Windows:

   ```
   aws elasticache describe-replication-groups ^
   	    --replication-group-id my-replication-group
   ```

   For more information, see [describe\-replication\-groups](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-replication-groups.html) in the *AWS CLI Reference*\.

## Scaling down a Redis replication group \(ElastiCache API\)<a name="Scaling.RedisReplGrps.ScaleDown.API"></a>

The following process scales your replication group from its current node type to a new, smaller node type using the ElastiCache API\. During this process, ElastiCache for Redis updates the DNS entries so they point to the new nodes\. Because of this you don't have to update the endpoints in your application\. For Redis 5\.0\.5 and above, you can scale auto failover enabled clusters while the cluster continues to stay online and serve incoming requests\. On version 5\.0\.4 and below, you may notice a brief interruption of reads and writes on previous versions from the primary node while the DNS entry is updated\.\. However, reads from the read replica cache clusters continue uninterrupted\.

The amount of time it takes to scale down to a smaller node type varies, depending upon your node type and the amount of data in your current cache cluster\.

**To scale down a Redis Replication Group \(ElastiCache API\)**

1. Determine which node types you can scale down to using the ElastiCache API `ListAllowedNodeTypeModifications` action with the following parameter\.
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

1. Scale your current replication group up to the new node type using the `ModifyRedplicationGroup` ElastiCache API action and with the following parameters\.
   + `ReplicationGroupId` – the name of the replication group\.
   + `CacheNodeType` – the new, smaller node type of the cache clusters in this replication group\. This value must be one of the instance types returned by the `ListAllowedNodeTypeModifications` action in step 1\.
   + `CacheParameterGroupName` – \[Optional\] Use this parameter if you are using `reserved-memory` to manage your cluster's reserved memory\. Specify a custom cache parameter group that reserves the correct amount of memory for your new node type\. If you are using `reserved-memory-percent` you can omit this parameter\.
   + `ApplyImmediately` – Set to `true` to causes the scale\-up process to be applied immediately\. To postpone the scale\-down process to the next maintenance window, use `ApplyImmediately``=false`\.

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

1. If you used `ApplyImmediately``=true`, monitor the status of the replication group using the ElastiCache API `DescribeReplicationGroups` action with the following parameters\. When the status changes from *modifying* to *available*, you can begin writing to your new, scaled down replication group\.
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