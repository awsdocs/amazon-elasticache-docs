# Finding replication group endpoints<a name="Replication.Endpoints"></a>

An application can connect to any node in a replication group, provided that it has the DNS endpoint and port number for that node\. Depending upon whether you are running a Redis \(cluster mode disabled\) or a Redis \(cluster mode enabled\) replication group, you will be interested in different endpoints\.

**Redis \(Cluster Mode Disabled\)**  
Redis \(cluster mode disabled\) clusters with replicas have three types of endpoints; the *primary endpoint*, the *reader endpoint* and the *node endpoints*\. The primary endpoint is a DNS name that always resolves to the primary node in the cluster\. The primary endpoint is immune to changes to your cluster, such as promoting a read replica to the primary role\. For write activity, we recommend that your applications connect to the primary endpoint, not the primary node\.

A reader endpoint will evenly split incoming connections to the endpoint between all read replicas in an ElastiCache for Redis cluster\. Additional factors such as when the application creates the connections or how the application \(re\)\-uses the connections will determine the traffic distribution\. Reader endpoints keep up with cluster changes in real\-time as replicas are added or removed\. You can place your ElastiCache for Redis cluster’s multiple read replicas in different AWS Availability Zones \(AZ\) to ensure high availability of reader endpoints\. 

**Note**  
A reader endpoint is not a load balancer\. It is a DNS record that will resolve to an IP address of one of the replica nodes in a round robin fashion\.

For read activity, applications can also connect to any node in the cluster\. Unlike the primary endpoint, node endpoints resolve to specific endpoints\. If you make a change in your cluster, such as adding or deleting a replica, you must update the node endpoints in your application\.

**Redis \(Cluster Mode Enabled\)**  
Redis \(cluster mode enabled\) clusters with replicas, because they have multiple shards \(API/CLI: node groups\), which mean they also have multiple primary nodes, have a different endpoint structure than Redis \(cluster mode disabled\) clusters\. Redis \(cluster mode enabled\) has a *configuration endpoint* which "knows" all the primary and node endpoints in the cluster\. Your application connects to the configuration endpoint\. Whenever your application writes to or reads from the cluster's configuration endpoint, Redis, behind the scenes, determines which shard the key belongs to and which endpoint in that shard to use\. It is all quite transparent to your application\.

You can find the endpoints for a cluster using the ElastiCache console, the AWS CLI, or the ElastiCache API\.

**Finding Replication Group Endpoints**

To find the endpoints for your replication group, see one of the following topics:
+ [Finding a Redis \(Cluster Mode Disabled\) Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Redis)
+ [Finding Endpoints for a Redis \(Cluster Mode Enabled\) Cluster \(Console\)](Endpoints.md#Endpoints.Find.RedisCluster)
+ [Finding the Endpoints for Replication Groups \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI.ReplGroups)
+ [Finding Endpoints for Replication Groups \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API.ReplGroups)