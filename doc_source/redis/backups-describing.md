# Describing Backups<a name="backups-describing"></a>

The following procedures show you how to display a list of your backups\. If you desire, you can also view the details of a particular backup\.

## Describing Backups \(Console\)<a name="backups-describing-CON"></a>

**To display backups using the AWS Management Console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the navigation pane, choose **Backups**\.

1. Use the **Filter** list to display **manual**, **automatic**, or **all** backups\.

1. To see the details of a particular backup, choose the box to the left of the backup's name\.

## Describing Backups \(AWS CLI\)<a name="backups-describing-CLI"></a>

To display a list of backups and optionally details about a specific backup, use the `describe-snapshots` CLI operation\. 

**Examples**

The following operation uses the parameter `--max-records` to list up to 20 backups associated with your account\. Omitting the parameter `--max-records` lists up to 50 backups\.

```
aws elasticache describe-snapshots --max-records 20
```

The following operation uses the parameter `--cache-cluster-id` to list only the backups associated with the cluster `my-cluster`\.

```
aws elasticache describe-snapshots --cache-cluster-id my-cluster
```

The following operation uses the parameter `--snapshot-name` to display the details of the backup `my-backup`\.

```
aws elasticache describe-snapshots --snapshot-name my-backup
```

For more information, see [describe\-snapshots](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-snapshots.html) in the AWS CLI Command Reference\.

## Describing Backups \(ElastiCache API\)<a name="backups-describing-API"></a>

To display a list of backups, use the `DescribeSnapshots` operation\.

**Examples**

The following operation uses the parameter `MaxRecords` to list up to 20 backups associated with your account\. Omitting the parameter `MaxRecords` will list up to 50 backups\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=DescribeSnapshots
    &MaxRecords=20
    &SignatureMethod=HmacSHA256
    &SignatureVersion=4
    &Timestamp=20141201T220302Z
    &Version=2014-12-01
    &X-Amz-Algorithm=AWS4-HMAC-SHA256
    &X-Amz-Date=20141201T220302Z
    &X-Amz-SignedHeaders=Host
    &X-Amz-Expires=20141201T220302Z
    &X-Amz-Credential=<credential>
    &X-Amz-Signature=<signature>
```

The following operation uses the parameter `CacheClusterId` to list all backups associated with the cluster `MyCluster`\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=DescribeSnapshots
    &CacheClusterId=MyCluster
    &SignatureMethod=HmacSHA256
    &SignatureVersion=4
    &Timestamp=20141201T220302Z
    &Version=2014-12-01
    &X-Amz-Algorithm=AWS4-HMAC-SHA256
    &X-Amz-Date=20141201T220302Z
    &X-Amz-SignedHeaders=Host
    &X-Amz-Expires=20141201T220302Z
    &X-Amz-Credential=<credential>
    &X-Amz-Signature=<signature>
```

The following operation uses the parameter `SnapshotName` to display the details for the backup `MyBackup`\.

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=DescribeSnapshots
    &SignatureMethod=HmacSHA256
    &SignatureVersion=4
    &SnapshotName=MyBackup
    &Timestamp=20141201T220302Z
    &Version=2014-12-01
    &X-Amz-Algorithm=AWS4-HMAC-SHA256
    &X-Amz-Date=20141201T220302Z
    &X-Amz-SignedHeaders=Host
    &X-Amz-Expires=20141201T220302Z
    &X-Amz-Credential=<credential>
    &X-Amz-Signature=<signature>
```

For more information, see [DescribeSnapshots](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeSnapshots.html)\.