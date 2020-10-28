# Parameter Management<a name="ParameterGroups.Management"></a>

Parameters are grouped together into named parameter groups for easier parameter management\. A parameter group represents a combination of specific values for the parameters that are passed to the engine software during startup\. These values determine how the engine processes on each node behave at runtime\. The parameter values on a specific parameter group apply to all nodes that are associated with the group, regardless of which cluster they belong to\.

To fine\-tune your cluster's performance, you can modify some parameter values or change the cluster's parameter group\.
+ You cannot modify or delete the default parameter groups\. If you need custom parameter values, you must create a custom parameter group\.
+ The parameter group family and the cluster you're assigning it to must be compatible\. For example, if your cluster is running Redis version 3\.2\.10, you can only use parameter groups, default or custom, from the Redis3\.2 family\.
+ If you change a cluster's parameter group, the values for any conditionally modifiable parameter must be the same in both the current and new parameter groups\.
+ When you change a cluster's parameters, the change is applied to the cluster either immediately or after the cluster is restarted\. This is true whether you change the cluster's parameter group itself or a parameter value within the cluster's parameter group\. To determine when a particular parameter change is applied, see the **Changes Take Effect** column in the tables for [Redis\-specific parameters](ParameterGroups.Redis.md)\. For information on rebooting a cluster, see [Rebooting a Cluster](Clusters.Rebooting.md)\.
+ You can associate parameter groups with Redis global datastores\. *Global datastores *are a collection of one or more clusters that span AWS Regions\. In this case, the parameter group is shared by all clusters that make up the global datastore\. Any modifications to the parameter group of the primary cluster are replicated to all remaining clusters in the global datastore\. For more information, see [Replication Across AWS Regions Using Global Datastores](Redis-Global-Datastore.md)\.

  You can check if a parameter group is part of a global datastore by looking in these locations:
  + On the ElastiCache console on the **Parameter Groups** page, the yes/no **Global** attribute 
  + The yes/no `IsGlobal` property of the [CacheParameterGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CacheParameterGroup.html) API operation