# Viewing a cluster's details<a name="Clusters.ViewDetails"></a>

You can view detail information about one or more clusters using the ElastiCache console, AWS CLI, or ElastiCache API\.

## Viewing a Cluster's Details \(Console\)<a name="Clusters.ViewDetails.CON.Memcached"></a>

You can view the details of a Memcached cluster using the ElastiCache console, the AWS CLI for ElastiCache, or the ElastiCache API\.

The following procedure details how to view the details of a Memcached cluster using the ElastiCache console\.

**To view a Memcached cluster's details**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the list in the upper\-right corner, choose the AWS Region you are interested in\.

1. In the ElastiCache console dashboard, choose **Memcached**\. Doing this displays a list of all your clusters that are running any version of Memcached\.

1. To see details of a cluster, choose the box to the left of the cluster's name\.

1. To view node information, choose the **Nodes** tab, which displays information on node\(s\) status and endpoint\.

1. To view metrics, choose the **Metrics** tab, which displays the relevant metrics for all nodes in the cluster\. For more information, see [Monitoring use with CloudWatch metrics](CacheMetrics.md)

1. Choose the **Network and security** tab to view details on the cluster's network connectivity and subnet group configuration and the VPC security group\. For more information, see [Subnets and subnet groups](SubnetGroups.md)\.

1. Choose the **Maintenance** tab to view details on the cluster's maintenance settings\. For more information, see [Managing maintenance](maintenance-window.md)\.

1. Choose the **Tags** tab to view details on any tags applied to cluster resources\. For more information, see [Tagging your ElastiCache resources](Tagging-Resources.md)\.

## Viewing a cluster's details \(AWS CLI\)<a name="Clusters.ViewDetails.CLI"></a>

The following code lists the details for *my\-cluster*:

```
aws elasticache describe-cache-clusters --cache-cluster-id my-cluster
```

Replace *my\-cluster* with the name of your cluster in a case where the cluster is created with 1 cache node and 0 shards using the `create-cache-cluster` command\.

In a case where the cluster is created using the AWS Management Console \(cluster node enabled or disabled with 1 or more shards\), use the following command to describe the cluster's details \(replace *my\-cluster* with the name of the replication group \(name of your cluster\):

```
aws elasticache describe-replication-groups --replication-group-id my-cluster 
```

For more information, see the AWS CLI for ElastiCache topic [https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-clusters.html](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-cache-clusters.html)\.

## Viewing a cluster's details \(ElastiCache API\)<a name="Clusters.ViewDetails.API"></a>

You can view the details for a cluster using the ElastiCache API `DescribeCacheClusters` action\. If the `CacheClusterId` parameter is included, details for the specified cluster are returned\. If the `CacheClusterId` parameter is omitted, details for up to `MaxRecords` \(default 100\) clusters are returned\. The value for `MaxRecords` cannot be less than 20 or greater than 100\.

The following code lists the details for `my-cluster`\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DescribeCacheClusters
   &CacheClusterId=my-cluster
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

For more information, see the ElastiCache API reference topic [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheClusters.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeCacheClusters.html)\.