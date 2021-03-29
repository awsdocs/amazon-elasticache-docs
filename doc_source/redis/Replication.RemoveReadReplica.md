# Deleting a read replica, for Redis \(Cluster Mode Disabled\) replication groups<a name="Replication.RemoveReadReplica"></a>

Information in the following topic applies to Redis \(cluster mode disabled\) replication groups only\.

As read traffic on your Redis replication group changes, you might want to add or remove read replicas\. Removing a node from a Redis \(cluster mode disabled\) replication group is the same as just deleting a cluster, though there are restrictions:
+ You cannot remove the primary from a replication group\. If you want to delete the primary, do the following:

  1. Promote a read replica to primary\. For more information on promoting a read replica to primary, see [Promoting a read replica to primary, for Redis \(cluster mode disabled\) replication groups](Replication.PromoteReplica.md)\.

  1. Delete the old primary\. For a restriction on this method, see the next point\.
+ If Multi\-AZ is enabled on a replication group, you can't remove the last read replica from the replication group\. In this case, do the following:

  1. Modify the replication group by disabling Multi\-AZ\. For more information, see [Modifying a replication group](Replication.Modify.md)\.

  1. Delete the read replica\.

You can remove a read replica from a Redis \(cluster mode disabled\) replication group using the ElastiCache console, the AWS CLI for ElastiCache, or the ElastiCache API\.

For directions on deleting a cluster from a Redis replication group, see the following:
+ [Using the AWS Management Console](Clusters.Delete.md#Clusters.Delete.CON)
+ [Using the AWS CLI](Clusters.Delete.md#Clusters.Delete.CLI)
+ [Using the ElastiCache API](Clusters.Delete.md#Clusters.Delete.API)
+ [Scaling clusters in Redis \(Cluster Mode Enabled\)](scaling-redis-cluster-mode-enabled.md)
+ [Decreasing the number of replicas in a shard](decrease-replica-count.md)