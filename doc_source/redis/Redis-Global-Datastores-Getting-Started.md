# Prerequisites and limitations<a name="Redis-Global-Datastores-Getting-Started"></a>

Before getting started with global datastores, be aware of the following:
+ Global datastores are supported in the following AWS Regions: Asia Pacific \(Seoul, Tokyo, Singapore, Sydney, Mumbai, and Osaka\), Europe \(Frankfurt, Paris, London, Ireland, and Stockholm\), US East \(N\. Virginia and Ohio\), US West \(N\. California and Oregon\), South America \(São Paulo\), AWS GovCloud \(US\-West and US\-East\), Canada \(Central\) Region, China \(Beijing and Ningxia\) 
+ To use global datastores, use Redis engine version 5\.0\.6 or higher and R5, R6g, R6gd, M5 or M6g node types\.
+  All clusters—primary and secondary—in your global datastore should have the same number of primary nodes, node type, engine version, and number of shards \(in case of cluster\-mode enabled\)\. Each cluster in your global datastore can have a different number of read replicas to accommodate the read traffic local to that cluster\. 

  Replication must be enabled if you plan to use an existing single\-node cluster\.
+ You can set up replication for a primary cluster from one AWS Region to a secondary cluster in up to two other AWS Regions\. 
**Note**  
The exception to this are China \(Beijing\) Region and China \(Ningxia\) regions, where replication can only occur between the two regions\. 
+ You can work with global datastores only in VPC clusters\. For more information, see [Access Patterns for Accessing an ElastiCache Cluster in an Amazon VPC](elasticache-vpc-accessing.md)\. Global datastores aren't supported when you use EC2\-Classic\. For more information, see [EC2\-Classic](https://docs.aws.amazon.com//AWSEC2/latest/UserGuide/ec2-classic-platform.html) in the *Amazon EC2 User Guide for Linux Instances\.*
**Note**  
At this time, you can't use global datastores in [Using local zones with ElastiCache ](Local_zones.md)\.
+ ElastiCache doesn't support autofailover from one AWS Region to another\. When needed, you can promote a secondary cluster manually\. For an example, see [Promoting the secondary cluster to primary](Redis-Global-Datastores-Console.md#Redis-Global-Datastores-Console-Promote-Secondary)\. 
+ To bootstrap from existing data, use an existing cluster as primary to create a global datastore\. We don't support adding an existing cluster as secondary\. The process of adding the cluster as secondary wipes data, which may result in data loss\. 
+ Parameter updates are applied to all clusters when you modify a local parameter group of a cluster belonging to a global datastore\. 
+ You can scale regional clusters both vertically \(scaling up and down\) and horizontally \(scaling in and out\)\. You can scale the clusters by modifying the global datastore\. All the regional clusters in the global datastore are then scaled without interruption\. For more information, see [Scaling ElastiCache for Redis clusters](Scaling.md)\.
+ Global datastores support [encryption at rest](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/at-rest-encryption.html), [encryption in transit](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/in-transit-encryption.html), and [Redis AUTH](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/auth.html)\. 
+  Global datastores support AWS KMS keys\. For more information, see [AWS key management service concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys) in the *AWS Key Management Service Developer Guide\.* 

**Note**  
Global datastores support [pub/sub messaging](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/elasticache-use-cases.html#elasticache-for-redis-use-cases-messaging) with the following stipulations:  
For cluster\-mode disabled, pub/sub is fully supported\. Events published on the primary cluster of the primary AWS Region are propagated to secondary AWS Regions\.
For cluster mode enabled, the following applies:  
For published events that aren't in a keyspace, only subscribers in the same AWS Region receive the events\.
For published keyspace events, subscribers in all AWS Regions receive the events\.