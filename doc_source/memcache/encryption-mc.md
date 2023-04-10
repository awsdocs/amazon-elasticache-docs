# Data security in Amazon ElastiCache<a name="encryption-mc"></a>

To help keep your data secure, Amazon ElastiCache and Amazon EC2 provide mechanisms to guard against unauthorized access of your data on the server\.

Amazon ElastiCache for Memcached also provides optional encryption features for data on clusters running Memcached versions 1\.6\.12 or later\. In\-transit encryption encrypts your data whenever it is moving from one place to another, such as between nodes in your cluster or between your cluster and your application\.

If you want to enable in\-transit, you must meet the following conditions\.
+ Your cluster must be running Memcached 1\.6\.12 or later\.
+ Your cluster must be created in a VPC based on Amazon VPC\.

**Topics**
+ [ElastiCache in\-transit encryption \(TLS\)](in-transit-encryption-mc.md)