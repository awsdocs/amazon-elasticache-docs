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
  + [Scaling down Redis clusters with replicas](Scaling.RedisReplGrps.ScaleDown.md)
  + [Increasing read capacity](Scaling.RedisReplGrps.ScaleOut.md)
  + [Decreasing read capacity](Scaling.RedisReplGrps.ScaleIn.md)