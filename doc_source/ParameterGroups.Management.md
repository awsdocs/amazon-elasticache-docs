# Parameter Management<a name="ParameterGroups.Management"></a>

Parameters are grouped together into named parameter groups for easier parameter management\. A parameter group represents a combination of specific values for the parameters that are passed to the engine software during startup\. These values determine how the engine processes on each node will behave at runtime\. The parameter values on a specific parameter group apply to all nodes that are associated with the group, regardless of which cluster they belong to\.

To fine tune your cluster's performance, you can modify some parameter values or change the cluster's parameter group\.

**Constraints**

+ You cannot modify or delete the default parameter groups\. If you need custom parameter values, you must create a custom parameter group\.

+ The parameter group family and the cluster you're assigning it to must be compatible\. For example, if your cluster is running Redis version 2\.8\.6, you can only use parameter groups, default or custom, from the Redis 2\.8 family, not the Redis 2\.6 parameter group family\.

+ If you change a cluster's parameter group, the values for any conditionally modifiable parameter must be the same in both the current and new parameter groups\.

+ When running Redis 3\.2\.4 or later you have the option of running it as Redis \(cluster mode disabled\) or Redis \(cluster mode enabled\)\.

  + To run Redis \(cluster mode disabled\) use the parameter group `default.redis3.2` or one derived from it\.

  + To run Redis \(cluster mode enabled\) use the parameter group `default.redis3.2.cluster.on` or one derived from it\.

+ When you make a change to a cluster's parameters, either by changing the cluster's parameter group or by changing a parameter value in the cluster's parameter group, the changes are applied to the cluster either immediately or after the cluster is restarted\. To determine when a particular parameter change is applied, see the **Changes Take Effect** column in the tables for [Memcached Specific Parameters](ParameterGroups.Memcached.md) and [Redis Specific Parameters](ParameterGroups.Redis.md)\. For information on rebooting a cluster, go to [Rebooting a Cluster](Clusters.Rebooting.md)\.