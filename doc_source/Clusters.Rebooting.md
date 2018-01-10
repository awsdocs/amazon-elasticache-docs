# Rebooting a Cluster<a name="Clusters.Rebooting"></a>

Some changes require that the cluster be rebooted for the changes to be applied\. For example, for some parameters, changing the parameter value in a parameter group is only applied after a reboot\.

When you reboot a cluster, the cluster flushes all its data and restarts its engine\. During this process you cannot access the cluster\. Because the cluster flushed all its data, when the cluster is available again, you are starting with an empty cluster\.

Rebooting a cluster is currently supported on Memcached and Redis \(cluster mode disabled\) clusters\. Rebooting is not supported on Redis \(cluster mode enabled\) clusters\.

You are able to reboot a cluster using the ElastiCache console, the AWS CLI, or the ElastiCache API\. Whether you use the ElastiCache console, the AWS CLI or the ElastiCache API, you can only initiate rebooting a single cluster\. To reboot multiple clusters you must iterate on the process or operation\.

**Redis \(cluster mode enabled\) and Reboots**  
If you make changes to parameters that require a Redis \(cluster mode enabled\) cluster reboot for the changes to be applied, follow these steps\.  
Create a manual backup of your cluster\. See [Making Manual Backups](backups-manual.md)\.
Delete the Redis \(cluster mode enabled\) cluster\. See [Deleting a Cluster](Clusters.Delete.md)\.
Restore the cluster using the altered parameter group and backup to seed the new cluster\. See [Restoring From a Backup with Optional Cluster Resizing](backups-restoring.md)\.

## Rebooting a Cluster \(Console\)<a name="Clusters.Rebooting.CON"></a>

You can reboot a cluster using the ElastiCache console\.

**To reboot a cluster \(console\)**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the dropdown in the upper right corner, choose the region you are interested in\.

1. In the navigation pane, choose **Memcached** or **Redis**\.

   A list of clusters running the chosen engine will appear\.

1. Choose the cluster to reboot by choosing on the box to the left of the cluster's name\.

   The **Reboot** button will become active\.

   If you choose more than one cluster, the **Reboot** button becomes disabled\.

1. Choose **Reboot**\.

   The reboot cluster confirmation screen appears\.

1. To reboot the cluster, choose **Reboot**\. The status of the cluster will change to *rebooting cluster nodes*\.

   To not reboot the cluster, choose **Cancel**\.

To reboot multiple clusters, repeat steps 2 through 5 for each cluster you want to reboot\.

## Rebooting a Cache Cluster \(AWS CLI\)<a name="Clusters.Rebooting.CLI"></a>

To reboot a cluster \(AWS CLI\), use the `reboot-cache-cluster` CLI operation\.

To reboot specific nodes in the cluster, use the `--cache-node-ids-to-reboot` to list the specific clusters to reboot\. The following command reboots the nodes 0001, 0002, and 0004 of *myCluster*\.

For Linux, macOS, or Unix:

```
aws elasticache reboot-cache-cluster \
    --cache-cluster-id myCluster \
    --cache-node-ids-to-reboot 0001 0002 0004
```

For Windows:

```
aws elasticache reboot-cache-cluster ^
    --cache-cluster-id myCluster ^
    --cache-node-ids-to-reboot 0001 0002 0004
```

To reboot all the nodes in the cluster, use the `--cache-node-ids-to-reboot` parameter and list all the cluster's node ids\. For more information, see [reboot\-cache\-cluster](http://docs.aws.amazon.com/cli/latest/reference/elasticache/reboot-cache-cluster.html)\.

## Rebooting a Cache Cluster \(ElastiCache API\)<a name="Clusters.Rebooting.API"></a>

To reboot a cluster using the ElastiCache API, use the `RebootCacheCluster` action\.

To reboot specific nodes in the cluster, use the `CacheNodeIdsToReboot` to list the specific clusters to reboot\. The following command reboots the nodes 0001, 0002, and 0004 of *myCluster*\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=RebootCacheCluster
   &CacheClusterId=myCluster
   &CacheNodeIdsToReboot.member.1=0001
   &CacheNodeIdsToReboot.member.2=0002
   &CacheNodeIdsToReboot.member.3=0004
   &Version=2015-02-02
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```

To reboot all the nodes in the cluster, use the `CacheNodeIdsToReboot` parameter and list all the cluster's node ids\. For more information, see [RebootCacheCluster](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RebootCacheCluster.html)\.