# ElastiCache Notifications<a name="elasticache-notifications"></a>

This topic covers ElastiCache notifications that you might be interested in\. A notification is a situation or event that, in most cases, is temporary, lasting only until a solution is found and implemented\. Notifications generally have a start date and a resolution date, after which the notification is no longer relevant\. Any one notification might or might not be relevant to you\. We recommend an implementation guideline that, if followed, improves the performance of your cluster\. 

Notifications do not announce new or improved ElastiCache features or functionality\.

## Alert: Memcached LRU Crawler Causing Segmentation Faults<a name="notification-lru-crawler"></a>

****  
Alert Date: February 28, 2017  
In some circumstances, your cluster might display instability with a segmentation fault in the Memcached LRU Crawler\. This is an issue within the Memcached engine that has existed for some time\. The issue became apparent in Memcached 1\.4\.33 when the LRU Crawler was enabled by default\.  
If you are experiencing this issue, we recommend that you disable the LRU Crawler until there is a fix\. To do so, use `lru_crawler disable` at the command line or modify the `lru_crawler` parameter value \(preferred\)\.  
Resolved Date:  
Resolution:  
 