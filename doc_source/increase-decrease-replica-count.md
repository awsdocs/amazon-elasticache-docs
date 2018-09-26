# Changing the Number of Replicas<a name="increase-decrease-replica-count"></a>

You can dynamically increase or decrease the number of read replicas in your Redis replication group using the AWS Management Console, the AWS CLI, or the ElastiCache API\. If your replication group is a Redis \(cluster mode enabled\) replication group, you can choose which shards \(node groups\) to increase or decrease the number of replicas in\.

To dynamically change the number of replicas in your Redis replication group, choose the operation from the following table that fits your situation\.


| To Do This | For Redis \(cluster mode enabled\) | For Redis \(cluster mode disabled\) | 
| --- | --- | --- | 
|  Add replicas  |  [Increasing the Number of Replicas in a Shard](increase-replica-count.md)  |  [Increasing the Number of Replicas in a Shard](increase-replica-count.md) [Adding a Read Replica, for Redis \(cluster mode disabled\) Replication Groups](Replication.AddReadReplica.md)  | 
|  Delete replicas  |  [Decreasing the Number of Replicas in a Shard](decrease-replica-count.md)  |  [Decreasing the Number of Replicas in a Shard](decrease-replica-count.md) [Deleting a Read Replica, for Redis \(cluster mode disabled\) Replication Groups ](Replication.RemoveReadReplica.md)  | 