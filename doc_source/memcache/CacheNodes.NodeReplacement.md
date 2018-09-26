# Replacing Nodes<a name="CacheNodes.NodeReplacement"></a>

Amazon ElastiCache for Memcached frequently upgrades its fleet with patches and upgrades being applied to instances seamlessly\. However, from time to time we need to relaunch your ElastiCache for Memcached nodes to apply mandatory OS updates to the underlying host\. These replacements are required to apply upgrades that strengthen security, reliability, and operational performance\.

You have the option to manage these replacements yourself at any time prior to the scheduled node replacement window\. When you manage a replacement yourself, your instance will receive the OS update when you relaunch the node and your scheduled node replacement will be cancelled\. You may continue to receive alerts indicating that the node replacement will take place\. If you've already manually mitigated the need for the maintenance, you can ignore these alerts\.

The following list identifies actions you can take when ElastiCache schedules one of your Memcached nodes for replacement\.
+ **Do nothing** – If you do nothing, ElastiCache will replace the node as scheduled\. When ElastiCache automatically replaces the node with a new node, the new node is initially empty\.
+ **Change your maintenance window** – For scheduled maintenance events where you receive an email or a notification event from ElastiCache, if you change your maintenance window before the scheduled replacement time, your node will now be replaced at the new time\. For more information, see [Modifying an ElastiCache Cluster](Clusters.Modify.md)\.
**Note**  
The ability to change your replacement window by moving your maintenance window is only available when the ElastiCache notification includes a maintenance window\. If the notification does not include a maintenance window, you cannot change your replacement window\.

  For example:

  Let's say, currently it's Thursday, 11/09 at 1500 and the next maintenance window is Friday, 11/10, at 1700\. Following are 3 scenarios with their outcomes:
  + You change your maintenance window to Fridays at 1600 \(after the current datetime and before the next scheduled maintenance window\)\. The node will be replaced on Friday, 11/10, at 1600\.
  + You change your maintenance window to Saturday at 1600 \(after the current datetime and after the next scheduled maintenance window\)\. The node will be replaced on Saturday, 11/11, at 1600\.
  + You change your maintenance window to Wednesday at 1600 \(earlier in the week than the current datetime\)\. The node will be replaced next Wednesday, 11/15, at 1600\.

  For instructions, see [Managing Maintenance](maintenance-window.md)\.
+ **Manually replace the node** – If you need to replace the node before the next maintenance window, manually replace the node\.

  If you manually replace the node, keys will be redistributed which will cause cache misses\.

**To manually replace a Memcached node**

  1. Delete the node scheduled for replacement\. For instructions, see [Removing Nodes from a Cluster](Clusters.DeleteNode.md)\. 

  1. Add a new node to the cluster\. For instructions, see [Adding Nodes to a Cluster](Clusters.AddNode.md)\. 

  1. If you are not using [Automatically Identify Nodes in your Memcached Cluster](AutoDiscovery.md) on this cluster, see your application and replace every instance of the old node's endpoint with the new node's endpoint\.