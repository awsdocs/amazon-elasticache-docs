# Modifying an ElastiCache Cluster<a name="Clusters.Modify"></a>

In addition to adding or removing nodes from a cluster, there can be times where you need to make other changes to an existing cluster, such as, adding a security group, changing the maintenance window or a parameter group\.

We recommend that you have your maintenance window fall at the time of lowest usage\. Thus it might need modification from time to time\.

When you change a cluster's parameters, the change is applied to the cluster either immediately or after the cluster is restarted\. This is true whether you change the cluster's parameter group itself or a parameter value within the cluster's parameter group\. To determine when a particular parameter change is applied, see the **Changes Take Effect** column in the tables for [Memcached Specific Parameters](ParameterGroups.Memcached.md) and  \. For information on rebooting a cluster, see [Rebooting a Cluster](Clusters.Rebooting.md)\.

## Using the AWS Management Console<a name="Clusters.Modify.CON"></a>

**To modify a cluster \(console\)**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the list in the upper\-right corner, choose the AWS Region where the cluster that you want to modify is located\.

1. In the navigation pane, choose the engine running on the cluster that you want to modify\.

   A list of the chosen engine's clusters appears\.

1. In the list of clusters, for the cluster that you want to modify, choose its name\. 

1. Choose **Actions** and then choose **Modify**\. 

   The **Modify Cluster** window appears\.

1. In the **Modify Cluster** window, make the modifications that you want\.
**Important**  
You can upgrade to newer engine versions\. For more information on doing so, see [Upgrading Engine Versions](VersionManagement.md)\. However, you can't downgrade to older engine versions except by deleting the existing cluster and creating it again\.

   The **Apply Immediately** box applies only to engine version modifications\. To apply changes immediately, choose the **Apply Immediately** check box\. If this box is not chosen, engine version modifications are applied during the next maintenance window\. Other modifications, such as changing the maintenance window, are applied immediately\.

1. Choose **Modify**\.

## Using the AWS CLI<a name="Clusters.Modify.CLI"></a>

You can modify an existing cluster using the AWS CLI `modify-cache-cluster` operation\. To modify a cluster's configuration value, specify the cluster's ID, the parameter to change and the parameter's new value\. The following example changes the maintenance window for a cluster named `my-cluster` and applies the change immediately\.

**Important**  
You can upgrade to newer engine versions\. For more information on doing so, see [Upgrading Engine Versions](VersionManagement.md)\. However, you can't downgrade to older engine versions except by deleting the existing cluster and creating it again\.

For Linux, macOS, or Unix:

```
aws elasticache modify-cache-cluster \
    --cache-cluster-id my-cluster \
    --preferred-maintenance-window sun:23:00-mon:02:00
```

For Windows:

```
aws elasticache modify-cache-cluster ^
    --cache-cluster-id my-cluster ^
    --preferred-maintenance-window sun:23:00-mon:02:00
```

The `--apply-immediately` parameter applies only to modifications in engine version and changing the number of nodes in a cluster\. If you want to apply any of these changes immediately, use the `--apply-immediately` parameter\. If you prefer postponing these changes to your next maintenance window, use the `--no-apply-immediately` parameter\. Other modifications, such as changing the maintenance window, are applied immediately\.

For more information, see the AWS CLI for ElastiCache topic [https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-cluster.html](https://docs.aws.amazon.com/cli/latest/reference/elasticache/modify-cache-cluster.html)\.

## Using the ElastiCache API<a name="Clusters.Modify.API"></a>

You can modify an existing cluster using the ElastiCache API `ModifyCacheCluster` operation\. To modify a cluster's configuration value, specify the cluster's ID, the parameter to change and the parameter's new value\. The following example changes the maintenance window for a cluster named `my-cluster` and applies the change immediately\.

**Important**  
You can upgrade to newer engine versions\. For more information on doing so, see [Upgrading Engine Versions](VersionManagement.md)\. However, you can't downgrade to older engine versions except by deleting the existing cluster and creating it again\.

Line breaks are added for ease of reading\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=ModifyCacheCluster
    &CacheClusterId=my-cluster
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

For more information, see the ElastiCache API reference topic [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyCacheCluster.html)\.