# ElastiCache in\-transit encryption \(TLS\)<a name="in-transit-encryption"></a>

To help keep your data secure, Amazon ElastiCache and Amazon EC2 provide mechanisms to guard against unauthorized access of your data on the server\. By providing in\-transit encryption capability, ElastiCache gives you a tool you can use to help protect your data when it is moving from one location to another\. 

 You enable in\-transit encryption on a replication group by setting the parameter `TransitEncryptionEnabled` to `true` \(CLI: `--transit-encryption-enabled`\) when you create the replication group\. You can do this whether you are creating the replication group using the AWS Management Console, the AWS CLI, or the ElastiCache API\. If you enable in\-transit encryption, you must also provide a value for `CacheSubnetGroup`\.

**Topics**
+ [In\-transit encryption overview](#in-transit-encryption-overview)
+ [In\-transit encryption conditions](#in-transit-encryption-constraints)
+ [In\-transit encryption best practices](#in-transit-encryption-best-practices)
+ [See also](#in-transit-encryption-see-also)
+ [Enabling in\-transit encryption](in-transit-encryption-enable.md)
+ [Enabling in\-transit encryption on a Redis Cluster using Python](in-transit-encryption-enable-python.md)

## In\-transit encryption overview<a name="in-transit-encryption-overview"></a>

Amazon ElastiCache in\-transit encryption is an optional feature that allows you to increase the security of your data at its most vulnerable points—when it is in transit from one location to another\. Because there is some processing needed to encrypt and decrypt the data at the endpoints, enabling in\-transit encryption can have some performance impact\. You should benchmark your data with and without in\-transit encryption to determine the performance impact for your use cases\.

ElastiCache in\-transit encryption implements the following features:
+ **Encrypted connections**—both the server and client connections are TLS encrypted\.
+ **Encrypted replication—**data moving between a primary node and replica nodes is encrypted\.
+ **Server authentication**—clients can authenticate that they are connecting to the right server\.
+ **Client authentication**—using the Redis AUTH feature, the server can authenticate the clients\.

## In\-transit encryption conditions<a name="in-transit-encryption-constraints"></a>

The following constraints on Amazon ElastiCache in\-transit encryption should be kept in mind when you plan your implementation:
+ In\-transit encryption is supported on replication groups running Redis versions 3\.2\.6, 4\.0\.10 and later\.
+ Modifying the in\-transit encryption setting, for an existing cluster, is supported on replication groups running Redis version 7 and later\.
+ In\-transit encryption is supported only for replication groups running in an Amazon VPC\.
+ In\-transit encryption is only supported for replication groups running the following node types\.
  + R6g, R5, R4, R3
  + M6g, M5, M4, M3
  + T4g, T3, T2

  For more information, see [Supported node types](CacheNodes.SupportedTypes.md)\.
+ In\-transit encryption is enabled by explicitly setting the parameter `TransitEncryptionEnabled` to `true`\.
+ To connect to an in\-transit encryption enabled replication group, a database must be enabled for Transport Layer Security \(TLS\)\. To connect to a replication group that is not in\-transit encryption enabled, the database cannot be TLS\-enabled\.

## In\-transit encryption best practices<a name="in-transit-encryption-best-practices"></a>
+ Because of the processing required to encrypt and decrypt the data at the endpoints, implementing in\-transit encryption can reduce performance\. Benchmark in\-transit encryption compared to no encryption on your own data to determine its impact on performance for your implementation\.
+ Because creating new connections can be expensive, you can reduce the performance impact of in\-transit encryption by persisting your TLS connections\.

## See also<a name="in-transit-encryption-see-also"></a>
+ [At\-Rest Encryption in ElastiCache for Redis](at-rest-encryption.md)
+ [Authenticating with the Redis AUTH command](auth.md)
+ [Authenticating Users with Role\-Based Access Control \(RBAC\)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Clusters.RBAC.html)
+ [Amazon VPCs and ElastiCache security](VPCs.md)
+ [Identity and Access Management for Amazon ElastiCache](IAM.md)