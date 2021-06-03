# Internetwork traffic privacy<a name="Security"></a>

Amazon ElastiCache for Memcached does not support encryption\. Therefore, we recommend using client\-side encryption\. If you are using instances in the r6g and m6g families, you get in\-memory encryption for free\. 

Amazon ElastiCache uses the following techniques to secure your cache data and protect it from unauthorized access:
+ **[Amazon VPCs and ElastiCache security](VPCs.md)** explains the type of security group you need for your installation\.
+ **[Identity and access management in Amazon ElastiCache](IAM.md)** for granting and limiting actions of users, groups, and roles\.