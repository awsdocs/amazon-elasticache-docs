# Replacing Nodes<a name="CacheNodes.NodeReplacement"></a>

Amazon ElastiCache for Redis frequently upgrades its fleet with patches and upgrades being applied to instances seamlessly\. However, from time to time we need to relaunch your ElastiCache for Redis nodes to apply mandatory OS updates to the underlying host\. These replacements are required to apply upgrades that strengthen security, reliability, and operational performance\.

You have the option to manage these replacements yourself at any time before the scheduled node replacement window\. When you manage a replacement yourself, your instance will receive the OS update when you relaunch the node and your scheduled node replacement will be cancelled\. You may continue to receive alerts indicating that the node replacement will take place\. If you've already manually mitigated the need for the maintenance, you can ignore these alerts\.

The following list identifies actions you can take when ElastiCache schedules one of your Redis nodes for replacement\. To expedite finding the information you need for your situation, choose from the following menu\.
+ [Do nothing](#DoNothing) – Let Amazon ElastiCache replace the node as scheduled\.
+ [Change your maintenance window](#ChangeWindow) – Change your maintenance window to a better time\.
+ Redis \(cluster mode enabled\) Configurations
  + [Replace the only node in any Redis cluster](#ReplaceStandalone) – A procedure to replace a node in a Redis cluster using backup and restore\.
  + [Replace a replica node in any Redis cluster](#ReplaceReplica) – A procedure to replace a read\-replica in any Redis cluster by increasing and decreasing the replica count with no cluster downtime\.
  + [Replace any node in a Redis (cluster mode enabled) shard](#ReplaceShardNode) – A dynamic procedure with no cluster downtime to replace a node in a Redis \(cluster mode enabled\) cluster by scaling out and scaling in\.
+ Redis \(cluster mode disabled\) Configurations
  + [Replace the only node in any Redis cluster](#ReplaceStandalone) – Procedure to replace any node in a Redis cluster using backup and restore\.
  + [Replace a replica node in any Redis cluster](#ReplaceReplica) – A procedure to replace a read\-replica in any Redis cluster by increasing and decreasing the replica count with no cluster downtime\.
  + [Replace a node in a Redis (cluster mode disabled) cluster](#ReplaceStandaloneClassic) – Procedure to replace a node in a Redis \(cluster mode disabled\) cluster using replication\.
  + [Replace a Redis (cluster mode disabled) read-replica](#ReplaceReadReplica) – A procedure to manually replace a read\-replica in a Redis \(cluster mode disabled\) replication group\.
  + [Replace a Redis (cluster mode disabled) primary node](#ReplacePrimary) – A procedure to manually replace the primary node in a Redis \(cluster mode disabled\) replication group\.

**Redis node replacement options**
+ **Do nothing** – If you do nothing, ElastiCache will replace the node as scheduled\. 

  If the node is a member of an auto failover enabled cluster, ElastiCache for Redis provides improved availability during patching, updates and other maintenance\-related node replacements\.

   For ElastiCache for Redis Cluster configurations that are set up to use ElastiCache for Redis Cluster clients, will now complete while the cluster serves incoming write requests\. 

  For non\-ElastiCache for Redis Cluster configurations, you may notice a brief write interruption, of up to a few seconds, associated with DNS update\.

  If the node is standalone, Amazon ElastiCache will first launch a replacement node and then sync from the existing node\. The existing node will not be available for service requests during this time\. Once the sync is complete, the existing node is terminated and the new node takes its place\. ElastiCache makes a best effort to retain your data during this operation\. 

   
+ **Change your maintenance window** – For scheduled maintenance events where you receive an email or a notification event from ElastiCache, if you change your maintenance window before the scheduled replacement time, your node will now be replaced at the new time\. For more information, see:
  + [Modifying an ElastiCache Cluster](Clusters.Modify.md)
  + [Modifying a Replication Group](Replication.Modify.md)
**Note**  
The ability to change your replacement window by moving your maintenance window is only available when the ElastiCache notification includes a maintenance window\. If the notification does not include a maintenance window, you cannot change your replacement window\.

  For example:

  Let's say, currently it's Thursday, 11/09, at 1500 and the next maintenance window is Friday, 11/10, at 1700\. Following are 3 scenarios with their outcomes:
  + You change your maintenance window to Fridays at 1600 \(after the current datetime and before the next scheduled maintenance window\)\. The node will be replaced on Friday, 11/10, at 1600\.
  + You change your maintenance window to Saturday at 1600 \(after the current datetime and after the next scheduled maintenance window\)\. The node will be replaced on Saturday, 11/11, at 1600\.
  + You change your maintenance window to Wednesday at 1600 \(earlier in the week than the current datetime\)\. The node will be replaced next Wednesday, 11/15, at 1600\.

  For instructions, see [Managing Maintenance](maintenance-window.md)\.

   
+ **Replace the only node in any Redis cluster** – If the cluster does not have any read replicas, you can use the following procedure to replace the node\.

**To replace the only node using backup and restore**

  1. Create a snapshot of the node's cluster\. For instructions, see [Making Manual Backups](backups-manual.md)\.

  1. Create a new cluster seeding it from the snapshot\. For instructions, see [Restoring From a Backup with Optional Cluster Resizing](backups-restoring.md)\.

  1. Delete the cluster with the node scheduled for replacement\. For instructions, see [Deleting a Cluster](Clusters.Delete.md)\.

  1. In your application, replace the old node's endpoint with the new node's endpoint\.

   
+ **Replace a replica node in any Redis cluster** – To replace a replica cluster, increase your replica count by adding a replica then decrease the replica count by removing the replica you want to replace\. This process is dynamic and does not have any cluster downtime\.
**Note**  
If your shard or replication group already has 5 replicase, you need to reverse steps 1 and 2\.

**To replace a replica in any Redis cluster**

  1. Increase the replica count by adding a replica to the shard or replication group\. For more information, see [Increasing the Number of Replicas in a Shard](increase-replica-count.md)\.

  1. Delete the replica you want to replace\. For more information, see [Decreasing the Number of Replicas in a Shard](decrease-replica-count.md)\.

  1. Update the endpoints in your application\.

   
+ **Replace any node in a Redis \(cluster mode enabled\) shard** – If you want to replace the node in a cluster with no downtime, use online resharding to first add a shard by scaling out and then delete the shard with the node to be replaced by scaling in\.

**To replace any node in a Redis \(cluster mode enabled\) cluster**

  1. Scale out: Add an additional shard with the same configuration as the existing shard with the node to be replaced\. For more information, see [Adding Shards with Online Resharding](redis-cluster-resharding-online.md#redis-cluster-resharding-online-add)\. 

  1. Scale in: Delete the shard with the node to be replaced\. For more information, see [Removing Shards with Online Resharding](redis-cluster-resharding-online.md#redis-cluster-resharding-online-remove)\.

  1. Update the endpoints in your application\.

   
+ **Replace a node in a Redis \(cluster mode disabled\) cluster** – If the cluster is a Redis \(cluster mode disabled\) cluster without any read replicas, you can use the following procedure to replace the node\.

**Replace the node using replication \(cluster mode disabled only\)**

  1. Add replication to the cluster with the node scheduled for replacement as the primary\. Do not enable Multi\-AZ on this cluster\. For instructions, see [To add replication to a Redis cluster with no shards](Clusters.AddNode.md#AddReplication.CON)\.

  1. Add a read\-replica to the cluster\. For instructions, see [To add nodes to a cluster \(console\)](Clusters.AddNode.md#AddNode.CON)\.

  1. Promote the newly created read\-replica to primary\. For instructions, see [Promoting a Read Replica to Primary, for Redis \(cluster mode disabled\) Replication Groups](Replication.PromoteReplica.md)\.

  1. Delete the node scheduled for replacement\. For instructions, see [Removing Nodes from a Cluster](Clusters.DeleteNode.md)\.

  1. In your application, replace the old node's endpoint with the new node's endpoint\.

   
+ **Replace a Redis \(cluster mode disabled\) read\-replica** – If the node is a read\-replica, replace the node\.

  If your cluster has only 1 replica node and Multi\-AZ is enabled, you must disable Multi\-AZ before you can delete the replica\. For instructions, see [Modifying a Replication Group](Replication.Modify.md)\.

**To replace a Redis \(cluster mode disabled\) read replica**

  1. Delete the replica that is scheduled for replacement\. For instructions, see:
     + [Decreasing the Number of Replicas in a Shard](decrease-replica-count.md)
     + [Removing Nodes from a Cluster](Clusters.DeleteNode.md)

  1. Add a new replica to replace the one that is scheduled for replacement\. If you use the same name as the replica you just deleted, you can skip step 3\. For instructions, see:
     + [Increasing the Number of Replicas in a Shard](increase-replica-count.md)
     + [Adding a Read Replica, for Redis \(cluster mode disabled\) Replication Groups](Replication.AddReadReplica.md)

  1. In your application, replace the old replica's endpoint with the new replica's endpoint\.

  1. If you disabled Multi\-AZ at the start, re\-enable it now\. For instructions, see [Enabling Multi\-AZ with Automatic Failover](AutoFailover.md#AutoFailover.Enable)\.

   
+ **Replace a Redis \(cluster mode disabled\) primary node** – If the node is the primary node, promote a read\-replica to primary, and then delete the replica that used to be the primary node\.

  If your cluster has only 1 replica and Multi\-AZ is enabled, you must disable Multi\-AZ before you can delete the replica in step 2\. For instructions, see [Modifying a Replication Group](Replication.Modify.md)\.

**To replace a Redis \(cluster mode disabled\) primary node**

  1. Promote a read\-replica to primary\. For instructions, see [Promoting a Read Replica to Primary, for Redis \(cluster mode disabled\) Replication Groups](Replication.PromoteReplica.md)\.

  1. Delete the node that is scheduled for replacement \(the old primary\)\. For instructions, see [Removing Nodes from a Cluster](Clusters.DeleteNode.md)\.

  1. Add a new replica to replace the one scheduled for replacement\. If you use the same name as the node you just deleted, you can skip changing endpoints in your application\.

     For instructions, see [Adding a Read Replica, for Redis \(cluster mode disabled\) Replication Groups](Replication.AddReadReplica.md)\.

  1. In your application, replace the old node's endpoint with the new node's endpoint\.

  1. If you disabled Multi\-AZ at the start, re\-enable it now\. For instructions, see [Enabling Multi\-AZ with Automatic Failover](AutoFailover.md#AutoFailover.Enable)\.

   