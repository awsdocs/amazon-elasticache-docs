# Getting Started with Amazon ElastiCache for Redis<a name="GettingStarted"></a>

Following, you can find topics that lead you through creating, granting access to, connecting to, and finally deleting a Redis \(cluster mode disabled\) cluster using the ElastiCache Management Console\. As part of this, the section starts by helping you determine the requirements for your cluster and create your own AWS account\.

Amazon ElastiCache supports high availability through the use of Redis replication groups\. For information about Redis replication groups and how to create them, see [High Availability Using Replication Groups](Replication.md)\.

Beginning with Redis version 3\.2, ElastiCache Redis supports partitioning your data across multiple node groups, with each node group implementing a replication group\. This exercise creates a standalone Redis cluster\.

**Topics**
+ [Determine Requirements](getting-started-determine-requirements.md)
+ [Setting Up](set-up.md)
+ [Step 1: Launch a Cluster](GettingStarted.CreateCluster.md)
+ [Step 2: Authorize Access](GettingStarted.AuthorizeAccess.md)
+ [Step 3: Connect to a Cluster's Node](GettingStarted.ConnectToCacheNode.md)
+ [Step 4: Delete Your Cluster \(Avoid Unnecessary Charges\)](GettingStarted.DeleteCacheCluster.md)
+ [Where Do I Go From Here?](GettingStarted.WhereGoFromHere.md)