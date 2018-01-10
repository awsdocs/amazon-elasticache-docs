# Viewing a Cluster's Details \(ElastiCache API\)<a name="Clusters.ViewDetails.API"></a>

You can view the details for a cluster using the ElastiCache API `DescribeCacheClusters` action\. If the `CacheClusterId` parameter is included, details for the specified cluster are returned\. If the `CacheClusterId` parameter is omitted, details for up to `MaxRecords` \(default 100\) clusters are returned\. The value for `MaxRecords` cannot be less than 20 or greater than 100\.

The following code lists the details for `myCluster`\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DescribeCacheClusters
   &CacheClusterId=myCluster
   &Version=2015-02-02
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```

The following code list the details for up to 25 clusters\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DescribeCacheClusters
   &MaxRecords=25
   &Version=2015-02-02
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```

For more information, go to the ElastiCache API reference topic [http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheClusters.html](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheClusters.html)\.