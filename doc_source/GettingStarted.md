# Getting Started with Amazon ElastiCache<a name="GettingStarted"></a>

Beginning with determining the requirements for your cluster and creating your own AWS account, the topics in this section walk you through the process of creating, granting access to, connecting to, and finally deleting a Redis \(cluster mode disabled\) cluster using the ElastiCache console\.

Amazon ElastiCache supports high availability through the use of Redis replication groups\. For information about Redis replication groups and how to create them, see [ElastiCache Replication \(Redis\)](Replication.md)\.

Beginning with Redis version 3\.2, ElastiCache Redis supports partitioning your data across multiple node groups, with each node group implementing a replication group\. This exercise creates a standalone Redis cluster\.


+ [Determine Your Requirements \[Every time\]](getting-started-determine-requirements.md)
+ [Step 1: Create an AWS Account and Set Up Permissions \[One time\]](GettingStarted.BeforeYouBegin.md)
+ [Step 2: Launch a Cluster](GettingStarted.CreateCluster.md)
+ [Step 3: \(Optional\) View Cluster Details](GettingStarted.ViewClusterDetails.md)
+ [Step 4: Authorize Access](GettingStarted.AuthorizeAccess.md)
+ [Step 5: Connect to a Cluster's Node](GettingStarted.ConnectToCacheNode.md)
+ [Step 6: Delete Your Cluster \[Avoid Unnecessary Charges\]](GettingStarted.DeleteCacheCluster.md)
+ [Where Do I Go From Here?](GettingStarted.WhereGoFromHere.md)