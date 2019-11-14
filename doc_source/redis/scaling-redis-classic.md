# Scaling Clusters for Redis \(Cluster Mode Disabled\)<a name="scaling-redis-classic"></a>

Redis \(cluster mode disabled\) clusters can be a single\-node cluster with 0 shards or multi\-node clusters with 1 shard\. Single\-node clusters use the one node for both reads and writes\. Multi\-node clusters always have 1 node as the read/write primary node with 0 to 5 read\-only replica nodes\.

**Contents**
+ [Scaling Single\-Node Clusters for Redis \(Cluster Mode Disabled\)](Scaling.RedisStandalone.md)
  + [Scaling Up Single\-Node Clusters for Redis \(Cluster Mode Disabled\)](Scaling.RedisStandalone.ScaleUp.md)
    + [Scaling Up Single\-Node Clusters for Redis \(Cluster Mode Disabled\) \(Console\)](Scaling.RedisStandalone.ScaleUp.md#Scaling.RedisStandalone.ScaleUp.CON)
    + [Scaling Up Single\-Node Redis Cache Clusters \(AWS CLI\)](Scaling.RedisStandalone.ScaleUp.md#Scaling.RedisStandalone.ScaleUp.CLI)
    + [Scaling Up Single\-Node Redis Cache Clusters \(ElastiCache API\)](Scaling.RedisStandalone.ScaleUp.md#Scaling.RedisStandalone.ScaleUp.API)
  + [Scaling Down Single\-Node Redis Clusters](Scaling.RedisStandalone.ScaleDown.md)
    + [Scaling Down a Single\-Node Redis Cluster \(Console\)](Scaling.RedisStandalone.ScaleDown.md#Scaling.RedisStandalone.ScaleDown.CON)
    + [Scaling Down a Single\-Node Redis Cache Cluster \(AWS CLI\)](Scaling.RedisStandalone.ScaleDown.md#Scaling.RedisStandalone.ScaleDown.CLI)
    + [Scaling Down a Single\-Node Cache Cluster for Redis \(Cluster Mode Disabled\) \(ElastiCache API\)](Scaling.RedisStandalone.ScaleDown.md#Scaling.RedisStandalone.ScaleDown.API)
+ [Scaling Redis \(Cluster Mode Disabled\) Clusters with Replica Nodes](Scaling.RedisReplGrps.md)
  + [Scaling Up Redis Clusters with Replicas](Scaling.RedisReplGrps.ScaleUp.md)
    + [Scaling Up a Redis Cluster with Replicas \(Console\)](Scaling.RedisReplGrps.ScaleUp.md#Scaling.RedisReplGrps.ScaleUp.CON)
    + [Scaling Up a Redis Replication Group \(AWS CLI\)](Scaling.RedisReplGrps.ScaleUp.md#Scaling.RedisReplGrps.ScaleUp.CLI)
    + [Scaling Up a Redis Replication Group \(ElastiCache API\)](Scaling.RedisReplGrps.ScaleUp.md#Scaling.RedisReplGrps.ScaleUp.API)
  + [Scaling Down Redis Clusters with Replicas](Scaling.RedisReplGrps.ScaleDown.md)
    + [Scaling Down a Redis Cluster with Replicas \(Console\)](Scaling.RedisReplGrps.ScaleDown.md#Scaling.RedisReplGrps.ScaleDown.CON)
    + [Scaling Down a Redis Replication Group \(AWS CLI\)](Scaling.RedisReplGrps.ScaleDown.md#Scaling.RedisReplGrps.ScaleDown.CLI)
    + [Scaling Down a Redis Replication Group \(ElastiCache API\)](Scaling.RedisReplGrps.ScaleDown.md#Scaling.RedisReplGrps.ScaleDown.API)
  + [Increasing Read Capacity](Scaling.RedisReplGrps.ScaleOut.md)
  + [Decreasing Read Capacity](Scaling.RedisReplGrps.ScaleIn.md)