# Amazon ElastiCache for Redis Data Encryption<a name="encryption"></a>

To help keep your data secure, Amazon ElastiCache and Amazon EC2 provide mechanisms to guard against unauthorized access of your data on the server\.

Amazon ElastiCache for Redis also provides optional encryption features for data on clusters running Redis version 3\.2\.6 and later:

+ In\-transit encryption encrypts your data whenever it is moving from one place to another, such as between nodes in your cluster or between your cluster and your application\.

+ At\-rest encryption encrypts your on\-disk data during sync and backup operations\.


+ [Amazon ElastiCache for Redis In\-Transit Encryption](in-transit-encryption.md)
+ [Amazon ElastiCache for Redis At\-Rest Encryption](at-rest-encryption.md)