# Deleting a Backup<a name="backups-deleting"></a>

An automatic backup is automatically deleted when its retention limit expires\. If you delete a cluster, all of its automatic backups are also deleted\. If you delete a replication group, all of the automatic backups from the clusters in that group are also deleted\.

ElastiCache provides a deletion API that lets you delete a backup at any time, regardless of whether the backup was created automatically or manually\. \(Since manual backups do not have a retention limit, manual deletion is the only way to remove them\.\) 

You can delete a backup using the ElastiCache console, the AWS CLI, or the ElastiCache API\.

## Deleting a Backup \(Console\)<a name="backups-deleting-CON"></a>

The following procedure deletes a backup using the ElastiCache console\.

**To delete a backup**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Backups**\.

   The Backups screen appears with a list of your backups\.

1. Choose the box to the left of the name of the backup you want to delete\.

1. Choose **Delete**\.

1. If you want to delete this backup, choose **Delete** on the **Delete Backup** confirmation screen\. The status changes to *deleting*\.

## Deleting a Backup \(AWS CLI\)<a name="backups-deleting-CLI"></a>

Use the delete\-snapshot AWS CLI operation with the following parameter to delete a backup\.

+ `--snapshot-name` – Name of the backup to be deleted\.

The following code deletes the backup `myBackup`\.

```
aws elasticache delete-snapshot --snapshot-name myBackup
```

For more information, see [delete\-snapshot](http://docs.aws.amazon.com/cli/latest/reference/elasticache/delete-snapshot.html) in the *AWS Command Line Interface Reference*\.

## Deleting a Backup \(ElastiCache API\)<a name="backups-deleting-API"></a>

Use the `DeleteSnapshot` API operation with the following parameter to delete a backup\.

+ `SnapshotName` – Name of the backup to be deleted\.

The following code deletes the backup `myBackup`\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DeleteSnapshot
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &SnapshotId=myBackup
   &Timestamp=20150202T192317Z
   &Version=2015-02-02
   &X-Amz-Credential=<credential>
```

For more information, see [DeleteSnapshot](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteSnapshot.html) in the *Amazon ElastiCache API Reference*\.