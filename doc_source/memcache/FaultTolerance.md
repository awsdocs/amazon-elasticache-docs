# Mitigating Failures<a name="FaultTolerance"></a>

When planning your Amazon ElastiCache implementation, you should plan so that failures have a minimal impact upon your application and data\. The topics in this section cover approaches you can take to protect your application and data from failures\.

**Topics**
+ [Mitigating Failures when Running Memcached](#FaultTolerance.Memcached)
+ [Recommendations](#FaultTolerance.Recommendations)

## Mitigating Failures when Running Memcached<a name="FaultTolerance.Memcached"></a>

When running the Memcached engine, you have the following options for minimizing the impact of a failure\. There are two types of failures to address in your failure mitigation plans: node failure and Availability Zone failure\.

### Mitigating Node Failures<a name="FaultTolerance.Memcached.Node"></a>

To mitigate the impact of a node failure, spread your cached data over more nodes\. Because Memcached does not support replication, a node failure will always result in some data loss from your cluster\.

When you create your Memcached cluster you can create it with 1 to 40 nodes, or more by special request\. Partitioning your data across a greater number of nodes means you'll lose less data if a node fails\. For example, if you partition your data across 10 nodes, any single node stores approximately 10% of your cached data\. In this case, a node failure loses approximately 10% of your cache which needs to be replaced when a replacement node is created and provisioned\. If the same data were cached in 3 larger nodes, the failure of a node would lose approximately 33% of your cached data\.

If you need more than 40 nodes in a Memcached cluster, or more than 300 nodes total in an AWS Region, fill out the ElastiCache Limit Increase Request form at [https://aws\.amazon\.com/contact\-us/elasticache\-node\-limit\-request/](https://aws.amazon.com/contact-us/elasticache-node-limit-request/)\.

For information on specifying the number of nodes in a Memcached cluster, see [Creating a Memcached cluster \(console\)](Clusters.Create.md#Clusters.Create.CON.Memcached)\.

### Mitigating Availability Zone Failures<a name="FaultTolerance.Memcached.AZ"></a>

To mitigate the impact of an Availability Zone failure, locate your nodes in as many Availability Zones as possible\. In the unlikely event of an AZ failure, you will lose the data cached in that AZ, not the data cached in the other AZs\.

**Why so many nodes?**  
*If my region has only 3 Availability Zones, why do I need more than 3 nodes since if an AZ fails I lose approximately one\-third of my data?*

This is an excellent question\. Remember that we’re attempting to mitigate two distinct types of failures, node and Availability Zone\. You’re right, if your data is spread across Availability Zones and one of the zones fails, you will lose only the data cached in that AZ, irrespective of the number of nodes you have\. However, if a node fails, having more nodes will reduce the proportion of data lost\.

There is no "magic formula" for determining how many nodes to have in your cluster\. You must weight the impact of data loss vs\. the likelihood of a failure vs\. cost, and come to your own conclusion\.

For information on specifying the number of nodes in a Memcached cluster, see [Creating a Memcached cluster \(console\)](Clusters.Create.md#Clusters.Create.CON.Memcached)\.

For more information on regions and Availability Zones, see [Choosing regions and availability zones](RegionsAndAZs.md)\.

## Recommendations<a name="FaultTolerance.Recommendations"></a>

There are two types of failures you need to plan for, individual node failures and broad Availability Zone failures\. The best failure mitigation plan will address both kinds of failures\.

### Minimizing the Impact of Failures<a name="FaultTolerance.Recommendations.NodeFailure"></a>

When running Memcached and partitioning your data across nodes, the more nodes you use the smaller the data loss if any one node fails\.

### Minimizing the Impact of Availability Zone Failures<a name="FaultTolerance.Recommendations.AZFailure"></a>

To minimize the impact of an Availability Zone failure, we recommend launching your nodes in as many different Availability Zones as are available\. Spreading your nodes evenly across AZs will minimize the impact in the unlikely event of an AZ failure\.