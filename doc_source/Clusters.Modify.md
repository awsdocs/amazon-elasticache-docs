# Modifying an ElastiCache Cluster<a name="Clusters.Modify"></a>

In addition to adding or removing nodes from a cluster, there can be times where you need to make other changes to an existing cluster, such as, adding a security group, changing the maintenance window or a parameter group\.

We recommend that you have your maintenance window fall at the time of lowest usage\. Thus it might need modification from time to time\.

When you make a change to a cluster's parameters, either by changing the cluster's parameter group or by changing a parameter value in the cluster's parameter group, the changes are applied to the cluster either immediately or after the cluster is restarted\. To determine when a particular parameter change is applied, see the **Changes Take Effect** column in the tables for [Memcached Specific Parameters](ParameterGroups.Memcached.md) and [Redis Specific Parameters](ParameterGroups.Redis.md)\. For information on rebooting a cluster, go to [Rebooting a Cluster](Clusters.Rebooting.md)\.

## Modifying a Cluster \(Console\)<a name="Clusters.Modify.CON"></a>

**To modify a cluster \(console\)**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the dropdown in the upper right corner, choose the region you are interested in\.

1. In the navigation pane, choose **Redis** or **Memcached**\.

   A list of the chosen engine's clusters appears\.

1. In the list of clusters, choose the name of the cluster you want to modify\. The modifications you can make to a Redis \(cluster mode enabled\) cluster are limited to modifications to security groups, description, parameter groups, backup options, maintenance window, and SNS notifications\.

1. Choose **Modify**\. 

   The **Modify Cluster** window appears\.

1. In the **Modify Cluster** window, make the modification\(s\) you want\.
**Important**  
You can upgrade to newer engine versions \(see [Upgrading Engine Versions](VersionManagement.md)\), but you cannot downgrade to older engine versions except by deleting the existing cluster and creating it anew\.

   Because the newer Redis versions provide a better and more stable user experience, Redis versions 2\.6\.13, 2\.8\.6, and 2\.8\.19 are deprecated when using the ElastiCache console\. We recommend against using these Redis versions\. If you need to use one of them, work with the AWS CLI or ElastiCache API\.

   For more information, see the following topics:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/Clusters.Modify.html)

   The **Apply Immediately** box applies only to modifications in node type and engine version\. If you want to apply any of these changes immediately, choose the **Apply Immediately** check box\. If this box is not chosen, engine version and node type modifications will be applied during the next maintenance window\. Other modifications, such as changing the maintenance window, are applied immediately\.

1. Choose **Modify**\.

## Modifying a Cache Cluster \(AWS CLI\)<a name="Clusters.Modify.CLI"></a>

You can modify an existing cluster using the AWS CLI `modify-cache-cluster` operation\. To modify a cluster's configuration value, specify the cluster's ID, the parameter to change and the parameter's new value\. The following example changes the maintenance window for a cluster named `myCluster` and applies the change immediately\.

**Important**  
You can upgrade to newer engine versions \(see [Upgrading Engine Versions](VersionManagement.md)\), but you cannot downgrade to older engine versions except by deleting the existing cache cluster or replication group and creating it anew\.

For Linux, macOS, or Unix:

```
aws elasticache modify-cache-cluster \
    --cache-cluster-id myCluster \
    --preferred-maintenance-window sun:23:00-mon:02:00
```

For Windows:

```
aws elasticache modify-cache-cluster ^
    --cache-cluster-id myCluster ^
    --preferred-maintenance-window sun:23:00-mon:02:00
```

The `--apply-immediately` parameter applies only to modifications in node type, engine version, and changing the number of nodes in a cluster\. If you want to apply any of these changes immediately, use the `--apply-immediately` parameter\. If you prefer postponing these changes to your next maintenance window, use the `--no-apply-immediately` parameter\. Other modifications, such as changing the maintenance window, are applied immediately\.

For more information, go to the AWS CLI for ElastiCache topic [http://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-cluster.html](http://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-cluster.html)\.

## Modifying a Cache Cluster \(ElastiCache API\)<a name="Clusters.Modify.API"></a>

You can modify an existing cluster using the ElastiCache API `ModifyCacheCluster` operation\. To modify a cluster's configuration value, specify the cluster's ID, the parameter to change and the parameter's new value\. The following example changes the maintenance window for a cluster named `myCluster` and applies the change immediately\.

**Important**  
You can upgrade to newer engine versions \(see [Upgrading Engine Versions](VersionManagement.md)\), but you cannot downgrade to older engine versions except by deleting the existing cache cluster or replication group and creating it anew\.

Line breaks are added for ease of reading\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=ModifyCacheCluster
    &CacheClusterId=myCluster
    &PreferredMaintenanceWindow=sun:23:00-mon:02:00
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20150901T220302Z
    &X-Amz-Algorithm=AWS4-HMAC-SHA256
    &X-Amz-Date=20150202T220302Z
    &X-Amz-SignedHeaders=Host
    &X-Amz-Expires=20150901T220302Z
    &X-Amz-Credential=<credential>
    &X-Amz-Signature=<signature>
```

The `ApplyImmediately` parameter applies only to modifications in node type, engine version, and changing the number of nodes in a cluster\. If you want to apply any of these changes immediately, set the `ApplyImmediately` parameter to `true`\. If you prefer postponing these changes to your next maintenance window, set the `ApplyImmediately` parameter to `false`\. Other modifications, such as changing the maintenance window, are applied immediately\.

For more information, go to the ElastiCache API reference topic [http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html)\.