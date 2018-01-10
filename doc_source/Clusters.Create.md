# Creating a Cluster<a name="Clusters.Create"></a>

When you launch an Amazon ElastiCache cluster, you can choose to use the Memcached or Redis engine\. The Redis engine has two flavors, Redis \(cluster mode disabled\) and Redis \(cluster mode enabled\) To determine which engine will best suit your needs, see [Engines and Versions](SelectEngine.md) in this guide\.

In this section you will find instructions on creating a standalone cluster using the ElastiCache console, AWS CLI, or ElastiCache API\.

Knowing the answers to these questions before you begin will expedite creating your cluster\.

+ Which engine you will use?

  For a comparison of engines and engine versions, see [Engines and Versions](SelectEngine.md)\.

+ Which node instance type do you need?

  For guidance on choosing an instance node type, see [Choosing Your Node Size](CacheNodes.SelectSize.md)\.

+ Will you launch your cluster in a VPC or an Amazon VPC? 
**Important**  
If you're going to launch your cluster in an Amazon VPC, you need to create a subnet group in the same VPC before you start creating a cluster\. For more information, see [Subnets and Subnet Groups](SubnetGroups.md)\.  
An advantage of launching in a Amazon VPC is that, though ElastiCache is designed to be accessed from within AWS using Amazon EC2, if your cluster is in an Amazon VPC you can provide access from outside AWS\. For more information, see [Accessing ElastiCache Resources from Outside AWS](Access.Outside.md)\.

+ Do you need to customize any parameter values?

  If you do, you need to create a custom Parameter Group\. For more information, see [Creating a Parameter Group](ParameterGroups.Creating.md)\.

   If you're running Redis you may want to consider at least setting `reserved-memory` or `reserved-memory-percent`\. For more information, see [Managing Reserved Memory \(Redis\)](redis-memory-management.md)\.

+ Do you need to create your own *Security Group* or *VPC Security Group*? 

  For more information, see [Security Groups \[EC2\-Classic\]](SecurityGroups.md) and [Security in Your VPC](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Security.html)\.

+ How do you intend to implement fault tolerance?

  For more information, see [Mitigating Failures](FaultTolerance.md)\.


+ [Creating a Cluster \(Console\): Memcached](Clusters.Create.CON.Memcached.md)
+ [Creating a Redis \(cluster mode disabled\) Cluster \(Console\)](Clusters.Create.CON.Redis.md)
+ [Creating a Redis \(cluster mode enabled\) Cluster \(Console\)](Clusters.Create.CON.RedisCluster.md)
+ [Creating a Cache Cluster \(AWS CLI\)](Clusters.Create.CLI.md)
+ [Creating a Cache Cluster \(ElastiCache API\)](Clusters.Create.API.md)