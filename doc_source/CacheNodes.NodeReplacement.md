# Actions You Can Take When a Node is Scheduled for Replacement<a name="CacheNodes.NodeReplacement"></a>

The following sections specify actions you can take when ElastiCache schedules one or more of your nodes for replacement\.

## Memcached<a name="CacheNodes.NodeReplacement.Memcached"></a>

The following list identifies actions you can take when ElastiCache schedules one of your Memcached nodes for replacement\.

+ **Do nothing** – If you do nothing, ElastiCache will replace the node as scheduled\. When ElastiCache automatically replaces the node with a new node, the new node is initially empty\.

+ **Change your maintenance window** – For scheduled maintenance events where you receive an email or a notification event from ElastiCache, if you change your maintenance window before the scheduled replacement time, your node will now be replaced at the new time\. The new maintenance time can be no earlier than the originally scheduled time, and no later than a week from the originally scheduled time\. 
**Note**  
The ability to change your replacement window by moving your maintenance window is only available when the ElastiCache notification includes a maintenance window\. If the notification does not include a maintenance window, you cannot change your replacement window\.

  For example:

  Let's say, currently it's Thursday, 11/09 at 1500 and the next maintenance window is Friday, 11/10, at 1700\. Following are 3 scenarios with their outcomes:

  + You change your maintenance window to Fridays at 1600 \(after the current datetime and before the next scheduled maintenance window\)\. The node will be replaced on Friday, 11/10, at 1600\.

  + You change your maintenance window to Saturday at 1600 \(after the current datetime and after the next scheduled maintenance window\)\. The node will be replaced on Saturday, 11/11, at 1600\.

  + You change your maintenance window to Wednesday at 1600 \(earlier in the week than the current datetime\)\. The node will be replaced next Wednesday, 11/15, at 1600\.

  For instructions, see [Maintenance Window](VersionManagement.MaintenanceWindow.md)\.

+ **Manually replace the node** – If you need to replace the node before the next maintenance window, manually replace the node\.

  If you manually replace the node, keys will be redistributed which will cause cache misses\.

**To manually replace a Memcached node**

  1. Delete the node scheduled for replacement\. For instructions, see [Removing Nodes from a Cluster](Clusters.DeleteNode.md)\. 

  1. Add a new node to the cluster\. For instructions, see [Adding Nodes to a Cluster](Clusters.AddNode.md)\. 

  1. If you are not using [Node Auto Discovery \(Memcached\)](AutoDiscovery.md) on this cluster, go to your application and replace every instance of the old node's endpoint with the new node's endpoint\.

## Redis<a name="CacheNodes.NodeReplacement.Redis"></a>

The following list identifies actions you can take when ElastiCache schedules one of your Redis nodes for replacement\. To expedite finding the information you need for your situation, choose from the following menu\.

+ [Do nothing](#DoNothing) – Let Amazon ElastiCache replace the node as scheduled\.

+ [Change your maintenance window](#ChangeWindow) – Change your maintenance window to a better time\.

+ [Replace a read-replica](#ReplaceReadReplica) – A procedure to manually replace a read\-replica in a Redis replication group\.

+ [Replace the primary node](#ReplacePrimary) – A procedure to manually replace the primary node in a Redis replication group\.

+ [Replace a standalone node](#ReplaceStandalone) – Two different procedures to replace a standalone Redis node\.

**Redis node replacement options**

+ **Do nothing** – If you do nothing, ElastiCache will replace the node as scheduled\. 

  If the node is a member of a Redis \(cluster mode disabled\) cluster, the replacement node will sync with the primary node\.

  If the node is standalone, ElastiCache will first launch a replacement node and then sync from the existing node\. The existing node will not be available for service requests during this time\. Once the sync is complete, the existing node is terminated and the new node takes its place\. ElastiCache makes a best effort to retain your data during this operation\.

+ **Change your maintenance window** – For scheduled maintenance events where you receive an email or a notification event from ElastiCache, if you change your maintenance window before the scheduled replacement time, your node will now be replaced at the new time\. The new maintenance time can be no earlier than the originally scheduled time, and no later than a week from the originally scheduled time\. 
**Note**  
The ability to change your replacement window by moving your maintenance window is only available when the ElastiCache notification includes a maintenance window\. If the notification does not include a maintenance window, you cannot change your replacement window\.

  For example:

  Let's say, currently it's Thursday, 11/09, at 1500 and the next maintenance window is Friday, 11/10, at 1700\. Following are 3 scenarios with their outcomes:

  + You change your maintenance window to Fridays at 1600 \(after the current datetime and before the next scheduled maintenance window\)\. The node will be replaced on Friday, 11/10, at 1600\.

  + You change your maintenance window to Saturday at 1600 \(after the current datetime and after the next scheduled maintenance window\)\. The node will be replaced on Saturday, 11/11, at 1600\.

  + You change your maintenance window to Wednesday at 1600 \(earlier in the week than the current datetime\)\. The node will be replaced next Wednesday, 11/15, at 1600\.

  For instructions, see [Maintenance Window](VersionManagement.MaintenanceWindow.md)\.

+ **Replace a read\-replica** – If the node is a read\-replica, replace the node\.

  If your cluster has only 2 nodes and Multi\-AZ is enabled, you must disable Multi\-AZ before you can delete the replica\. For instructions, see [Modifying a Cluster with Replicas](Replication.Modify.md)\.

**To replace a read replica**

  1. Delete the replica that is scheduled for replacement\. For instructions, see [Deleting a Cluster](Clusters.Delete.md)\.

  1. Add a new replica to replace the one that is scheduled for replacement\. If you use the same name as the replica you just deleted, you can skip step 3\. For instructions, see [Adding a Read Replica to a Redis Cluster](Replication.AddReadReplica.md)\.

  1. In your application, replace the old replica's endpoint with the new replica's endpoint\.

  1. If you disabled Multi\-AZ at the start, re\-enable it now\. For instructions, see [Enabling Multi\-AZ with Automatic Failover](AutoFailover.md#AutoFailover.Enable)\.

+ **Replace the primary node** – If the node is the primary node, promote a read\-replica to primary, and then delete the former primary node\.

  If your cluster has only two nodes and Multi\-AZ is enabled, you must disable Multi\-AZ before you can delete the replica in step 2\. For instructions, see [Modifying a Cluster with Replicas](Replication.Modify.md)\.

**To replace a primary node**

  1. Promote a read\-replica to primary\. For instructions, see [Promoting a Read\-Replica to Primary](Replication.PromoteReplica.md)\.

  1. Delete the node that is scheduled for replacement \(the old primary\)\. For instructions, see [Deleting a Cluster](Clusters.Delete.md)\.

  1. Add a new replica to replace the one scheduled for replacement\. If you use the same name as the node you just deleted, you can skip step 4\.

     For instructions, see [Adding a Read Replica to a Redis Cluster](Replication.AddReadReplica.md)\.

  1. In your application, replace the old node's endpoint with the new node's endpoint\.

  1. If you disabled Multi\-AZ at the start, re\-enable it now\. For instructions, see [Enabling Multi\-AZ with Automatic Failover](AutoFailover.md#AutoFailover.Enable)\.

+ **Replace a standalone node** – If the node does not have any read replicas, you have two options to replace it:

**Option 1: Replace the node using backup and restore**

  1. Create a snapshot of the node\. For instructions, see [Making Manual Backups](backups-manual.md)\.

  1. Create a new node seeding it from the snapshot\. For instructions, see [Restoring From a Backup with Optional Cluster Resizing](backups-restoring.md)\.

  1. Delete the node scheduled for replacement\. For instructions, see [Deleting a Cluster](Clusters.Delete.md)\.

  1. In your application, replace the old node's endpoint with the new node's endpoint\.

**Option 2: Replace the node using replication**

  1. Add replication to the cluster with the node scheduled for replacement as the primary\. Do not enable Multi\-AZ on this cluster\. For instructions, see [To add replication to a Redis cluster with no shards](Clusters.AddNode.md#AddReplication.CON)\.

  1. Add a read\-replica to the cluster\. For instructions, see [To add nodes to a Memcached or Redis \(cluster mode disabled\) cluster with one shard \(console\)](Clusters.AddNode.md#AddNode.CON)\.

  1. Promote the newly created read\-replica to primary\. For instructions, see [Promoting a Read\-Replica to Primary](Replication.PromoteReplica.md)\.

  1. Delete the node scheduled for replacement\. For instructions, see [Deleting a Cluster](Clusters.Delete.md)\.

  1. In your application, replace the old node's endpoint with the new node's endpoint\.