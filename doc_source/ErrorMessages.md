# Amazon ElastiCache Error Messages<a name="ErrorMessages"></a>

The following error messages are returned by Amazon ElastiCache\. You may receive other error messages that are returned by ElastiCache, other AWS services, or by Memcached or Redis\. For descriptions of error messages from sources other than ElastiCache, see the documentation from the source that is generating the error message\.

+ [Cluster node quota exceeded](#ErrorMessages.ClusterNodeQuota)

+ [Customer's node quota exceeded](#ErrorMessages.CACHE_CLUSTER_CUSTOMER_QUOTA_EXCEEDED)

+ [Manual snapshot quota exceeded](#ErrorMessages.MANUAL_SNAPSHOT_WITHIN_24_HOURS_QUOTA_EXCEEDED)

Error Message: **Cluster node quota exceeded\. Each cluster can have at most *%n* nodes in this region\.**  
**Cause:** You attempted to create or modify a cluster with the result that the cluster would have more than *%n* nodes\.   
**Solution:** Change your request so that the cluster does not have more than *%n* nodes\. Or, if you need more than *%n* nodes, make your request using the [Amazon ElastiCache Node request form](https://aws.amazon.com/contact-us/elasticache-node-limit-request/)\.  
For more information, see [Amazon ElastiCache Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_elasticache) in *Amazon Web Services General Reference*\.

Error Messages: **Customer node quota exceeded\. You can have at most *%n* nodes in this region** Or, **You have already reached your quota of %s nodes in this region\.**  
**Cause:** You attempted to create or modify a cluster with the result that your account would have more than *%n* nodes across all clusters in this region\.  
**Solution:** Change your request so that the total nodes in the region across all clusters for this account does not exceed *%n*\. Or, if you need more than *%n* nodes, make your request using the [Amazon ElastiCache Node request form](https://aws.amazon.com/contact-us/elasticache-node-limit-request/)\.  
For more information, see [Amazon ElastiCache Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_elasticache) in *Amazon Web Services General Reference*\.

 Error Messages: **The maximum number of manual snapshots for this cluster taken within 24 hours has been reached** or **The maximum number of manual snapshots for this node taken within 24 hours has been reached its quota of *%n***  
**Cause:** You attempted to take a manual snapshot of a cluster when you have already taken the maximum number of manual snapshots allowed in a 24\-hour period\.  
**Solution:** Wait 24 hours to attempt another manual snapshot of the cluster\. Or, if you need to take a manual snapshot now, take the snapshot of another node that has the same data, such as a different node in a cluster\.