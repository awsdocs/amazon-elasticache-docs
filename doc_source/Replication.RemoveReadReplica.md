# Deleting a Read Replica<a name="Replication.RemoveReadReplica"></a>

**Important**  
Currently, ElastiCache does not support deleting a read replica from a Redis \(cluster mode enabled\) replication group\. If you need to reduce the number of read replicas, create the cluster anew with the desired number of read replicas\.

As read traffic on your replication group changes you might want to add or remove read replicas\. Removing a node from a replication group is the same as just deleting a cluster, though there are some restrictions\.

**Restriction on removing nodes from a replication group**

+ You cannot remove the primary from a replication group\. If you want to delete the primary, you must do the following:

  1. Promote a read replica to primary\. For more information on promoting a read replica to primary, see [Promoting a Read\-Replica to Primary](Replication.PromoteReplica.md)\.

  1. Delete the old primary\. See the next point for a restriction on this method\.

+ If Multi\-AZ is enabled on a replication group, you cannot remove the last read replica from the replication group\. In this case you must:

  1. Modify the replication group by disabling Multi\-AZ\. For more information, see [Modifying a Cluster with Replicas](Replication.Modify.md)\.

  1. Delete the read\-replica\.

You can remove a read replica from a replication group using the ElastiCache console, the AWS CLI for ElastiCache, or the ElastiCache API\.

**For directions on deleting a cluster see:**

+ [Deleting a Cluster \(Console\)](Clusters.Delete.md#Clusters.Delete.CON)

+ [Deleting a Cache Cluster \(AWS CLI\)](Clusters.Delete.md#Clusters.Delete.CLI)

+ [Deleting a Cache Cluster \(ElastiCache API\)](Clusters.Delete.md#Clusters.Delete.API)