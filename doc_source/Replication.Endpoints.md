# Finding Replication Group Endpoints<a name="Replication.Endpoints"></a>

An application can connect to any node in a replication group, provided that it has the DNS endpoint and port number for that node\. Depending upon whether you are running a Redis \(cluster mode disabled\) or a Redis \(cluster mode enabled\) replication group, you will be interested in different endpoints\.

**Redis \(cluster mode disabled\)**  
Redis \(cluster mode disabled\) clusters with replicas have two types of endpoints; the *primary endpoint* and the *node endpoints*\. The primary endpoint is a DNS name that always resolves to the primary node in the cluster\. The primary endpoint is immune to changes to your cluster, such as promoting a read replica to the primary role\. For write activity, we recommend that your applications connect to the primary endpoint instead of connecting directly to the primary\.

For read activity, applications can connect to any node in the cluster\. Unlike the primary endpoint, node endpoints resolve to specific endpoints\. If you make a change in your cluster, such as adding or deleting a replica, you must update the node endpoints in your application\.

For read activity, applications can connect to any node in the cluster\. Unlike the primary endpoint, node endpoints resolve to specific endpoints\. If you make a change in your cluster, such as adding or deleting a replica, you must update the node endpoints in your application\.

**Redis \(cluster mode enabled\)**  
Redis \(cluster mode enabled\) clusters with replicas, because they have multiple shards \(API/CLI: node groups\), which mean they also have multiple primary nodes, have a different endpoint structure than Redis \(cluster mode disabled\) clusters\. Redis \(cluster mode enabled\) has a *configuration endpoint* which "knows" all the primary and node endpoints in the cluster\. Your application connects to the configuration endpoint\. Whenever your application writes to or reads from the cluster's configuration endpoint, Redis, behind the scenes, determines which shard the key belongs to and which endpoint in that shard to use\. It is all quite transparent to your application\.

You can find the endpoints for a cluster using the ElastiCache console, the AWS CLI, or the ElastiCache API\.

**Finding Replication Group Endpoints**

To find the endpoints for your replication group, see one of the following topics:

+ [Finding a Redis \(cluster mode disabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Redis)

+ [Finding a Redis \(cluster mode enabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.RedisCluster)

+ [Finding the Endpoints for Replication Groups \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI.ReplGroups)

+ [Finding Endpoints for Replication Groups \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API.ReplGroups)

## Redis \(cluster mode disabled\)<a name="replication-endpoints-redis-classic"></a>

Redis \(cluster mode disabled\) clusters with replicas have two types of endpoints; the *primary endpoint* and the *node endpoints*\. The primary endpoint is a DNS name that always resolves to the primary node in the cluster\. The primary endpoint is immune to changes to your cluster, such as promoting a read replica to the primary role\. For write activity, we recommend that your applications connect to the primary endpoint instead of connecting directly to the primary\.

For read activity, applications can connect to any node in the cluster\. Unlike the primary endpoint, node endpoints resolve to specific endpoints\. If you make a change in your cluster, such as adding or deleting a replica, you must update the node endpoints in your application\.

## Redis \(cluster mode enabled\)<a name="replication-endpoints-redis-cluster"></a>

You can view the details, including endpoints, for a replication group using the AWS CLI `describe-replication-groups` command\. Use the `-replication-group-id` parameter to specify which replication group's endpoints you want to find\.

For an AWS CLI example for finding endpoints, see [Finding the Endpoints for Replication Groups \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI.ReplGroups)\.

**Example Finding endpoints for a replication group \(AWS CLI\)**  
The following code lists the details, including the replication group's endpoints, for `my-repl-group`\.  

```
aws elasticache describe-replication-groups -replication-group-id myreplgroup 
```
Output from this operation should look something like this \(JSON format\)\. The `ReadEndpoint` is each node's unique endpoint\. You should use this endpoint for all read operations\. For write operations, use the `PrimaryEndpoint` endpoint \.  

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

For more information, go to the AWS CLI for ElastiCache topic [describe\-replication\-groups](http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-replication-groups.html)\.

## Finding Replication Group Endpoints \(ElastiCache API\)<a name="Replication.Endpoints.Finding.API"></a>

You can view the details for a replication group using the ElastiCache API `DescribeReplicationGroups` action with the `ReplicationGroupId` parameter to specify a specific replication group\.

For an ElastiCache API example for finding endpoints, see [Finding Endpoints for Replication Groups \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API.ReplGroups)\.

**Example Finding endpoints for a replication group \(ElastiCache API\)**  
The following code lists the details, including the replication group's endpoints, for myReplGroup\.  

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DescribeReplicationGroups
   &MaxRecords=100
   &ReplicationGroupId=myReplGrp
   &Version=2015-02-02
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```
Output from this operation should look something like this\. The `ReadEndpoint` is each node's unique endpoint\. You should use this endpoint for all read operations\. For write operations, use the `PrimaryEndpoint` endpoint \.  

```
<DescribeReplicationGroupsResponse xmlns="http://elasticache.amazonaws.com/doc/2015-02-02/"> 
   <DescribeReplicationGroupsResult> 
     <ReplicationGroups> 
       <ReplicationGroup> 
         <SnapshottingClusterId>myreplgrp</SnapshottingClusterId> 
         <MemberClusters> 
            <ClusterId>myreplgrp-001</ClusterId> 
         </MemberClusters> 
         <NodeGroups> 
            <NodeGroup> 
              <NodeGroupId>0001</NodeGroupId> 
              <PrimaryEndpoint> 
                <Port>6379</Port> 
                <Address>myreplgrp.q68zge.ng.0001.use1devo.elmo-dev.amazonaws.com</Address> 
              </PrimaryEndpoint> 
              <Status>available</Status> 
              <NodeGroupMembers> 
                <NodeGroupMember> 
                  <CacheClusterId>myreplgrp-001</CacheClusterId> 
                  <ReadEndpoint> 
                     <Port>6379</Port> 
                     <Address>myreplgrp-001.q68zge.0001.use1devo.elmo-dev.amazonaws.com</Address> 
                  </ReadEndpoint> 
                  <PreferredAvailabilityZone>us-west-2c</PreferredAvailabilityZone> 
                  <CacheNodeId>0001</CacheNodeId> 
                  <CurrentRole>primary</CurrentRole> 
                </NodeGroupMember> 
                <NodeGroupMember> 
                  <CacheClusterId>myreplgrp-002</CacheClusterId> 
                  <ReadEndpoint> 
                     <Port>6379</Port> 
                     <Address>myreplgrp-002.q68zge.0002.use1devo.elmo-dev.amazonaws.com</Address> 
                  </ReadEndpoint> 
                  <PreferredAvailabilityZone>us-west-2b</PreferredAvailabilityZone> 
                  <CacheNodeId>0002</CacheNodeId> 
                  <CurrentRole>replica</CurrentRole> 
                </NodeGroupMember> 
              </NodeGroupMembers> 
            </NodeGroup> 
         </NodeGroups> 
         <ReplicationGroupId>myreplgrp</ReplicationGroupId> 
         <Status>available</Status> 
         <PendingModifiedValues /> 
         <Description>My replication group</Description> 
       </ReplicationGroup> 
     </ReplicationGroups> 
   </DescribeReplicationGroupsResult> 
   <ResponseMetadata> 
     <RequestId>144745b0-b9d3-11e3-8a16-7978bb24ffdf</RequestId> 
   </ResponseMetadata> 
</DescribeReplicationGroupsResponse>
```

**Redis \(cluster mode disabled\)**  
The primary endpoint, including the port, is found between the `<PrimaryEndpoint>` and `</PrimaryEndpoint>` tags\.

```
<CacheClusterId>myreplgrp-001</CacheClusterId> 
<PrimaryEndpoint> 
    <Port>6379</Port> 
    <Address>myreplgrp.q68zge.ng.0001.use1devo.elmo-dev.amazonaws.com</Address> 
</PrimaryEndpoint>
```

The read endpoints are found between the `<ReadEndpoint>` and `</ReadEndpoint>` tags for each node group in the replication group\.

```
<ReadEndpoint> 
   <Port>6379</Port> 
   <Address>myreplgrp-001.q68zge.0001.use1devo.elmo-dev.amazonaws.com</Address> 
</ReadEndpoint>
```

and

```
<CacheClusterId>myreplgrp-002</CacheClusterId> 
<ReadEndpoint> 
   <Port>6379</Port> 
   <Address>myreplgrp-002.q68zge.0001.use1devo.elmo-dev.amazonaws.com</Address> 
</ReadEndpoint>
```

**Redis \(cluster mode enabled\)**  
The configuration endpoint, including the port, is found between the `<ConfigurationEndpoint>` and `</ConfigurationEndpoint>` tags\.

```
<CacheClusterId>myreplgrp-001</CacheClusterId>
```

For more information, go to the ElastiCache API reference topic [DescribeReplicationGroups](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReplicationGroups.html)\.