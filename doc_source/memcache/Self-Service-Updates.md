# Self\-service updates in Amazon ElastiCache<a name="Self-Service-Updates"></a>

Amazon ElastiCache monitors your fleet of Memcached clusters and nodes to determine whether service updates are applicable as they become available\. Typically, you set up a predefined maintenance window so that ElastiCache can apply these updates\. However, in some cases you might find this approach too rigid and likely to constrain your operational flows\. 

With self\-service updates, you control when and which updates are applied\. You can also monitor the progress of these updates to your selected Memcached clusters in real time\. 

Depending on the type of service update, you can choose to stop an update to remaining nodes and clusters\. You can select a group of clusters \(including the ones that were partially updated\) to apply the service updates anytime until the service update expires\. 