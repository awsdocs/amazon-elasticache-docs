# Getting started with Amazon ElastiCache for Redis<a name="GettingStarted"></a>

Following, you can find topics that lead you through creating, granting access to, connecting to, and finally deleting a Redis \(cluster mode disabled\) cluster using the ElastiCache Management Console\. As part of this, the section starts by helping you determine the requirements for your cluster and create your own AWS account\.

Amazon ElastiCache supports high availability through the use of Redis replication groups\. For information about Redis replication groups and how to create them, see [High availability using replication groups](Replication.md)\.

Beginning with Redis version 3\.2, ElastiCache Redis supports partitioning your data across multiple node groups, with each node group implementing a replication group\. This exercise creates a standalone Redis cluster\.

**Topics**
+ [Setting up](set-up.md)
+ [Step 1: Create a subnet group](SubnetGroups.Creating-gs.md)
+ [Step 2: Create a cluster](GettingStarted.CreateCluster.md)
+ [Step 3: Authorize access to the cluster](GettingStarted.AuthorizeAccess.md)
+ [Step 4: Connect to the cluster's node](GettingStarted.ConnectToCacheNode.md)
+ [Step 5: Deleting a cluster](Clusters.Delete-gs.md)
+ [ElastiCache tutorials and videos](Tutorials.md)
+ [Where do I go from here?](GettingStarted.WhereGoFromHere.md)