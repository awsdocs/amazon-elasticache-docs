# Assigning a Subnet Group to a Cluster or Replication Group<a name="SubnetGroups.Assigning"></a>

After you have created a subnet group, you can launch a cluster or replication group in an Amazon VPC\. For more information, see one of the following topics\.

+ **Memcached cluster** – To launch a Memcached cluster, see [Creating a Cluster \(Console\): Memcached](Clusters.Create.CON.Memcached.md)\. In step 5\.a \(**Advanced Memcached Settings**\), choose a VPC subnet group\.

+ **Standalone Redis cluster** – To launch a single\-node Redis cluster, see [Creating a Redis \(cluster mode disabled\) Cluster \(Console\)](Clusters.Create.CON.Redis.md)\. In step 5\.a \(**Advanced Redis Settings**\), choose a VPC subnet group\.

+ **Redis \(cluster mode disabled\) replication group** – To launch a Redis \(cluster mode disabled\) replication group in a VPC, see [Creating a Redis \(cluster mode disabled\) Cluster with Replicas from Scratch \(Console\)](Replication.CreatingReplGroup.NoExistingCluster.Classic.md#Replication.CreatingReplGroup.NoExistingCluster.Classic.CON)\. In step 5\.b \(**Advanced Redis Settings**\), choose a VPC subnet group\.

+ **Redis \(cluster mode enabled\) replication group** – [Creating a Redis \(cluster mode enabled\) Cluster \(Console\)](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md#Replication.CreatingReplGroup.NoExistingCluster.Cluster.CON)\. In step 5\.a \(**Advanced Redis Settings**\), choose a VPC subnet group\.