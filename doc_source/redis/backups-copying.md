# Copying a backup<a name="backups-copying"></a>

You can make a copy of any backup, whether it was created automatically or manually\. You can also export your backup so you can access it from outside ElastiCache\. For guidance on exporting your backup, see [Exporting a backup](backups-exporting.md)\.

The following procedures show you how to copy a backup\.

## Copying a backup \(Console\)<a name="backups-copying-CON"></a>

**To copy a backup \(console\)**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. To see a list of your backups, from the left navigation pane choose **Backups**\.

1. From the list of backups, choose the box to the left of the name of the backup you want to copy\.

1. Choose **Copy**\.

1. In the **Create Copy of the Backup?** dialog box, do the following:

   1. In the **New backup name** box, type a name for your new backup\.

   1. Leave the optional **Target S3 Bucket** box blank\. This field should only be used to export your backup and requires special S3 permissions\. For information on exporting a backup, see [Exporting a backup](backups-exporting.md)\.

   1. Choose **Copy**\.

## Copying a backup \(AWS CLI\)<a name="backups-copying-CLI"></a>

To copy a backup, use the `copy-snapshot` operation\.

**Parameters**
+ `--source-snapshot-name` – Name of the backup to be copied\.
+ `--target-snapshot-name` – Name of the backup's copy\.
+ `--target-bucket` – Reserved for exporting a backup\. Do not use this parameter when making a copy of a backup\. For more information, see [Exporting a backup](backups-exporting.md)\.

The following example makes a copy of an automatic backup\.

For Linux, macOS, or Unix:

```
aws elasticache copy-snapshot \
    --source-snapshot-name automatic.my-redis-primary-2014-03-27-03-15 \
    --target-snapshot-name my-backup-copy
```

For Windows:

```
aws elasticache copy-snapshot ^
    --source-snapshot-name automatic.my-redis-primary-2014-03-27-03-15 ^
    --target-snapshot-name my-backup-copy
```

For more information, see [https://docs.aws.amazon.com/cli/latest/reference/elasticache/copy-snapshot.html](https://docs.aws.amazon.com/cli/latest/reference/elasticache/copy-snapshot.html) in the *AWS CLI*\.

## Copying a backup \(ElastiCache API\)<a name="backups-copying-API"></a>

To copy a backup, use the [CopySnapshot](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CopySnapshot.html) operation with the following parameters:

**Parameters**
+ `SourceSnapshotName` – Name of the backup to be copied\.
+ `TargetSnapshotName` – Name of the backup's copy\.
+ `TargetBucket` – Reserved for exporting a backup\. Do not use this parameter when making a copy of a backup\. For more information, see [Exporting a backup](backups-exporting.md)\.

The following example makes a copy of an automatic backup\.

**Example**  

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=CopySnapshot
    &SourceSnapshotName=automatic.my-redis-primary-2014-03-27-03-15
    &TargetSnapshotName=my-backup-copy
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20141201T220302Z
    &Version=2014-12-01
    &X-Amz-Algorithm=&AWS;4-HMAC-SHA256
    &X-Amz-Date=20141201T220302Z
    &X-Amz-SignedHeaders=Host
    &X-Amz-Expires=20141201T220302Z
    &X-Amz-Credential=<credential>
    &X-Amz-Signature=<signature>
```

For more information, see [CopySnapshot](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CopySnapshot.html) in the *Amazon ElastiCache API Reference*\.