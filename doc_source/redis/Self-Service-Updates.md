# Self\-service updates in Amazon ElastiCache<a name="Self-Service-Updates"></a>

Amazon ElastiCache automatically monitors your fleet of Redis clusters and nodes to apply service updates as they become available\. Typically, you set up a predefined maintenance window so that ElastiCache can apply these updates\. However, in some cases you might find this approach too rigid and likely to constrain your business flows\. 

With self\-service updates, you control when and which updates are applied\. You can also monitor the progress of these updates to your selected Redis clusters in real time\. 

Depending on your business requirements, you can choose to stop an update to remaining nodes and clusters\. You can select a new set of clusters \(including the ones that were partially updated\) to apply the service updates anytime until the service update expires\. 