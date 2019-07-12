# Self\-Service Updates in Amazon ElastiCache<a name="Self-Service-Updates"></a>

Amazon ElastiCache automatically monitors your fleet of Redis clusters and nodes to apply service updates as they become available\. This typically means that you must set up a pre\-defined maintenance window so that ElastiCache can apply these updates, which you might find too rigid and might constrain your business flows\. With self\-service updates, you control when and which updates are applied\. You can also monitor the progress of these updates to your selected Redis clusters in real\-time\. Depending on your business requirements, you can choose to stop the update to remaining nodes and clusters\. You can select a new set of clusters \(including the ones that were partially updated\) to apply the service updates anytime until the service update expires\. 

ElastiCache currently supports self\-service updates for the following engine versions and replication groups:
+ **Redis Engine Version:** 3\.2\.6 or 4\.0\.10 and later
+ **Redis Replication Group:** Redis \(cluster mode enabled\) multi\-node cluster or Redis \(cluster mode disabled\) cluster

**Important**  
Redis clusters created using the `cacheCluster` operation don't support this feature\. A Redis single\-node cluster created using the ElastiCache console is also not supported\.
Service updates in an expired state can't be applied\.
Amazon ElastiCache continues to apply other mandatory updates in your weekly maintenance window\.
For more information about compliance, see [Self\-Service Security Updates for Compliance](elasticache-compliance.md#elasticache-compliance-self-service)\.

**Topics**
+ [Managing the Service Updates](managing-updates.md)