# Scaling clusters for Redis \(Cluster Mode Disabled\)<a name="scaling-redis-classic"></a>

Redis \(cluster mode disabled\) clusters can be a single\-node cluster with 0 shards or multi\-node clusters with 1 shard\. Single\-node clusters use the one node for both reads and writes\. Multi\-node clusters always have 1 node as the read/write primary node with 0 to 5 read\-only replica nodes\.

**Contents**
+ [Scaling single\-node clusters for Redis \(Cluster Mode Disabled\)](Scaling.RedisStandalone.md)
  + [Scaling up single\-node clusters for Redis \(Cluster Mode Disabled\)](Scaling.RedisStandalone.ScaleUp.md)
    + [Scaling up single\-node clusters for Redis \(Cluster Mode Disabled\) \(Console\)](Scaling.RedisStandalone.ScaleUp.md#Scaling.RedisStandalone.ScaleUp.CON)
    + [Scaling up single\-node Redis cache clusters \(AWS CLI\)](Scaling.RedisStandalone.ScaleUp.md#Scaling.RedisStandalone.ScaleUp.CLI)
    + [Scaling up single\-node Redis cache clusters \(ElastiCache API\)](Scaling.RedisStandalone.ScaleUp.md#Scaling.RedisStandalone.ScaleUp.API)
  + [Scaling down single\-node Redis clusters](Scaling.RedisStandalone.ScaleDown.md)
    + [Scaling down a single\-node Redis cluster \(Console\)](Scaling.RedisStandalone.ScaleDown.md#Scaling.RedisStandalone.ScaleDown.CON)
    + [Scaling down single\-node Redis cache clusters \(AWS CLI\)](Scaling.RedisStandalone.ScaleDown.md#Scaling.RedisStandalone.ScaleUpDown-Modify.CLI)
    + [Scaling down single\-node Redis cache clusters \(ElastiCache API\)](Scaling.RedisStandalone.ScaleDown.md#Scaling.RedisStandalone.ScaleDown.API)
+ [Scaling Redis \(Cluster Mode Disabled\) clusters with replica nodes](Scaling.RedisReplGrps.md)
  + [Scaling up Redis clusters with replicas](Scaling.RedisReplGrps.ScaleUp.md)
    + [Scaling up a Redis cluster with replicas \(Console\)](Scaling.RedisReplGrps.ScaleUp.md#Scaling.RedisReplGrps.ScaleUp.CON)
    + [Scaling up a Redis replication group \(AWS CLI\)](Scaling.RedisReplGrps.ScaleUp.md#Scaling.RedisReplGrps.ScaleUp.CLI)
    + [Scaling up a Redis replication group \(ElastiCache API\)](Scaling.RedisReplGrps.ScaleUp.md#Scaling.RedisReplGrps.ScaleUp.API)
  + [Scaling down Redis clusters with replicas](Scaling.RedisReplGrps.ScaleDown.md)
    + [Scaling down a Redis cluster with replicas \(Console\)](Scaling.RedisReplGrps.ScaleDown.md#Scaling.RedisReplGrps.ScaleDown.CON)
    + [Scaling down a Redis replication group \(AWS CLI\)](Scaling.RedisReplGrps.ScaleDown.md#Scaling.RedisReplGrps.ScaleUp.CLI)
    + [Scaling down a Redis replication group \(ElastiCache API\)](Scaling.RedisReplGrps.ScaleDown.md#Scaling.RedisReplGrps.ScaleDown.API)
  + [Increasing read capacity](Scaling.RedisReplGrps.ScaleOut.md)
  + [Decreasing read capacity](Scaling.RedisReplGrps.ScaleIn.md)