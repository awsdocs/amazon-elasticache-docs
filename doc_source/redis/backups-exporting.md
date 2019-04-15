# Exporting a Backup<a name="backups-exporting"></a>

Amazon ElastiCache supports exporting your ElastiCache backup to an Amazon Simple Storage Service \(Amazon S3\) bucket, which gives you access to it from outside ElastiCache\. You can export a backup using the ElastiCache console, the AWS CLI, or the ElastiCache API\.

Exporting a backup can be helpful if you need to launch a cluster in a another region\. You can export your data in one region, copy the \.rdb file to the new region, and then use that \.rdb file to seed the new cluster instead of waiting for the new cluster to populate through use\. For information about seeding a new cluster, see [Seeding a New Cluster with an Externally Created Backup](backups-seeding-redis.md)\. Another reason you might want to export your cluster's data is to use the \.rdb file for offline processing\.

**Important**  
 The ElastiCache backup and the Amazon S3 bucket that you want to copy it to must be in the same region\.
Though backups copied to an Amazon S3 bucket are encrypted, we strongly recommend that you do not grant others access to the Amazon S3 bucket where you want to store your backups\.

Before you can export a backup to an Amazon S3 bucket you must have an Amazon S3 bucket in the same region as the backup, and then grant ElastiCache access to the bucket\. The first two steps show you how to do this\.

**Warning: Data Vulnerability**  
The following scenarios expose your data in ways you may not want\.  
**When another person has access to the Amazon S3 bucket you exported your backup to\.**  
To control access to your backups, only allow access to the Amazon Amazon S3 bucket to those who you want to access your data\. For information about managing access to an Amazon Amazon S3 bucket, see [Managing Access](https://docs.aws.amazon.com/AmazonS3/latest/dev/s3-access-control.html) in the *Amazon S3 Developer Guide*\.  
 
**When another person has permissions to use the CopySnapshot API\.**  
Users or groups that have permissions to use the *CopySnapshot* API can create their own Amazon S3 buckets and copy backups to it\. To control access to your backups, use an IAM policy to control who has the ability to use the *CopySnapshot* API\. For more information about using IAM to control the use of ElastiCache APIs, see [Authentication and Access Control for Amazon ElastiCache](IAM.md) in the *ElastiCache User Guide*\.

**Topics**
+ [Step 1: Create an Amazon S3 Bucket](#backups-exporting-create-s3-bucket)
+ [Step 2: Grant ElastiCache Access to Your Amazon S3 Bucket](#backups-exporting-grant-access)
+ [Step 3: Export an ElastiCache Backup](#backups-exporting-procedures)

## Step 1: Create an Amazon S3 Bucket<a name="backups-exporting-create-s3-bucket"></a>

The following procedure uses the Amazon S3 console to create an Amazon S3 bucket where you will export and store your ElastiCache backup\.

**To create an Amazon S3 bucket**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose **Create Bucket**\.

1. In **Create a Bucket \- Select a Bucket Name and Region**, do the following:

   1. In **Bucket Name**, type a name for your Amazon S3 bucket\.

      The of your Amazon S3 bucket must be DNS\-compliant\. Otherwise, ElastiCache cannot access your backup file\. The rules for DNS compliance are:
      + Names must be at least 3 and no more than 63 characters long\.
      + Names must be a series of one or more labels separated by a period \(\.\) where each label:
        + Starts with a lowercase letter or a number\.
        + Ends with a lowercase letter or a number\.
        + Contains only lowercase letters, numbers, and dashes\.
      + Names cannot be formatted as an IP address \(e\.g\., 192\.0\.2\.0\)\.

   1. From the **Region** list, choose a region for your Amazon S3 bucket\. This region must be the same region as the ElastiCache backup you want to export\.

   1. Choose **Create**\.

For more information about creating an Amazon S3 bucket, see [Creating a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/CreatingaBucket.html) in the *Amazon Simple Storage Service Console User Guide*\. 

## Step 2: Grant ElastiCache Access to Your Amazon S3 Bucket<a name="backups-exporting-grant-access"></a>

In order for ElastiCache to copy a snapshot to an Amazon S3 bucket, it must have access to the bucket\. The following procedure grants ElastiCache access to the Amazon S3 bucket you created in the previous step\.

**Warning**  
Even though backups copied to an Amazon S3 bucket are encrypted, your data may be accessed by anyone with access to your Amazon S3 bucket\. Therefore, we strongly recommend that you set up IAM policies to prevent unauthorized access to this Amazon S3 bucket\. For more information, see [Managing Access](https://docs.aws.amazon.com/AmazonS3/latest/dev/s3-access-control.html) in the *Amazon S3 Developer Guide*\.

**To grant ElastiCache access to an Amazon S3 bucket**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the name of the Amazon S3 bucket that you want to copy the backup to\. This should be the S3 bucket you created in [Step 1: Create an Amazon S3 Bucket](#backups-exporting-create-s3-bucket)\.

1. Make sure that the bucket's region is the same as your ElastiCache backup's region\. If it isn't, return to [Step 1: Create an Amazon S3 Bucket](#backups-exporting-create-s3-bucket) and create a new bucket in the same region as the cluster you will backup\.

1. Choose **Permissions**\.

1. Choose **Access Control List**\.

1. Under *Access for other AWS accounts*, choose **\+ Add account**\.

1. In the box, add the region's canonical id as shown in the following list:
   + China \(Beijing\) and China \(Ningxia\) Regions: 

     ```
     b14d6a125bdf69854ed8ef2e71d8a20b7c490f252229b806e514966e490b8d83
     ```

      
   + AWS GovCloud \(US\-West\) Region: 

     ```
     40fa568277ad703bd160f66ae4f83fc9dfdfd06c2f1b5060ca22442ac3ef8be6
     ```
**Important**  
The backup must be exported to an S3 bucket in AWS GovCloud \(US\)\. 
   + All other regions: 

     ```
     540804c33a284a299d2547575ce1010f2312ef3da9b3a053c8bc45bf233e4353
     ```

1. Set the permissions on the bucket by choosing **Yes** for:

   1. **List objects**

   1. **Write objects**

   1. **Read bucket permissions**

1. Choose **Save**\.

Your Amazon S3 bucket is now ready for you to export an ElastiCache backup to using the ElastiCache console, the AWS CLI, or the ElastiCache API\.

## Step 3: Export an ElastiCache Backup<a name="backups-exporting-procedures"></a>

Now that you've created your S3 bucket and granted ElastiCache permissions to access it, you can use the ElastiCache console, the AWS CLI, or the ElastiCache API to export your snapshot to it\. The following assumes that you have the following additional S3 specific IAM permissions\.

```
{
"Statement":
    {
    "Effect": "Allow",
    "Action": [
        "s3:GetBucketLocation",
        "s3:ListAllMyBuckets"
        ],
    "Resource": "arn:aws:s3:::*"
    }
    "Version": "2012-10-17"
}
```

**Topics**
+ [Exporting an ElastiCache Backup \(Console\)](#backups-exporting-CON)
+ [Exporting an ElastiCache Backup \(AWS CLI\)](#backups-exporting-CLI)
+ [Exporting an ElastiCache Backup \(ElastiCache API\)](#backups-exporting-API)

### Exporting an ElastiCache Backup \(Console\)<a name="backups-exporting-CON"></a>

The following process uses the ElastiCache console to export a backup to an Amazon S3 bucket so that you can access it from outside ElastiCache\. The Amazon S3 bucket must be in the same region as the ElastiCache backup\.

**To export an ElastiCache backup to an Amazon S3 bucket**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. To see a list of your backups, from the left navigation pane choose **Backups**\.

1. From the list of backups, choose the box to the left of the name of the backup you want to export\. 

1. Choose **Copy**\.

1. In **Create a Copy of the Backup?**, do the following: 

   1. In **New backup name** box, type a name for your new backup\.

      The name must be between 1 and 1,000 characters and able to be UTF\-8 encoded\.

      ElastiCache adds an instance identifier and `.rdb` to the value that you enter here\. For example, if you enter `my-exported-backup`, ElastiCache creates `my-exported-backup-0001.rdb`\.

   1. From the **Target S3 Location** list, choose the name of the Amazon S3 bucket that you want to copy your backup to \(the bucket that you created in [Step 1: Create an Amazon S3 Bucket](#backups-exporting-create-s3-bucket)\)\.

      The **Target S3 Location** must be an Amazon S3 bucket in the backup's region with the following permissions for the export process to succeed\.
      + Object access – **Read** and **Write**\.
      + Permissions access – **Read**\.

      For more information, see [Step 2: Grant ElastiCache Access to Your Amazon S3 Bucket](#backups-exporting-grant-access)\. 

   1. Choose **Copy**\.

**Note**  
If your S3 bucket does not have the permissions needed for ElastiCache to export a backup to it, you will receive one of the following error messages\. Return to [Step 2: Grant ElastiCache Access to Your Amazon S3 Bucket](#backups-exporting-grant-access) to add the permissions specified and retry exporting your backup\.  
ElastiCache has not been granted READ permissions %s on the S3 Bucket\.  
**Solution:** Add Read permissions on the bucket\.
ElastiCache has not been granted WRITE permissions %s on the S3 Bucket\.  
**Solution:** Add Write permissions on the bucket\.
ElastiCache has not been granted READ\_ACP permissions %s on the S3 Bucket\.  
**Solution:** Add **Read** for Permissions access on the bucket\.

If you want to copy your backup to another region, use Amazon S3 to copy it\. For more information, see [Copying an Object](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/MakingaCopyofanObject.html) in the *Amazon Simple Storage Service Console User Guide*\.

### Exporting an ElastiCache Backup \(AWS CLI\)<a name="backups-exporting-CLI"></a>

Export the backup to an Amazon S3 bucket using the `copy-snapshot` CLI operation with the following parameters:

**Parameters**
+ `--source-snapshot-name` – Name of the backup to be copied\.
+ `--target-snapshot-name` – Name of the backup's copy\.

  The name must be between 1 and 1,000 characters and able to be UTF\-8 encoded\.

  ElastiCache adds an instance identifier and `.rdb` to the value you enter here\. For example, if you enter `my-exported-backup`, ElastiCache creates `my-exported-backup-0001.rdb`\.
+ `--target-bucket` – Name of the Amazon S3 bucket where you want to export the backup\. A copy of the backup is made in the specified bucket\.

  The `--target-bucket` must be an Amazon S3 bucket in the backup's region with the following permissions for the export process to succeed\.
  + Object access – **Read** and **Write**\.
  + Permissions access – **Read**\.

  For more information, see [Step 2: Grant ElastiCache Access to Your Amazon S3 Bucket](#backups-exporting-grant-access)\.

The following operation copies a backup to my\-s3\-bucket\.

For Linux, macOS, or Unix:

```
aws elasticache copy-snapshot \
    --source-snapshot-name automatic.my-redis-primary-2016-06-27-03-15 \
    --target-snapshot-name my-exported-backup \
    --target-bucket my-s3-bucket
```

For Windows:

```
aws elasticache copy-snapshot ^
    --source-snapshot-name automatic.my-redis-primary-2016-06-27-03-15 ^
    --target-snapshot-name my-exported-backup ^
    --target-bucket my-s3-bucket
```

**Note**  
If your S3 bucket does not have the permissions needed for ElastiCache to export a backup to it, you will receive one of the following error messages\. Return to [Step 2: Grant ElastiCache Access to Your Amazon S3 Bucket](#backups-exporting-grant-access) to add the permissions specified and retry exporting your backup\.  
ElastiCache has not been granted READ permissions %s on the S3 Bucket\.  
**Solution:** Add Read permissions on the bucket\.
ElastiCache has not been granted WRITE permissions %s on the S3 Bucket\.  
**Solution:** Add Write permissions on the bucket\.
ElastiCache has not been granted READ\_ACP permissions %s on the S3 Bucket\.  
**Solution:** Add **Read** for Permissions access on the bucket\.

For more information, see [copy\-snapshot](https://docs.aws.amazon.com/cli/latest/reference/elasticache/copy-snapshot.html) in the *AWS CLI Command Reference*\.

If you want to copy your backup to another region, use Amazon S3 copy\. For more information, see [Copying an Object](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/MakingaCopyofanObject.html) in the *Amazon Simple Storage Service Console User Guide*\.

### Exporting an ElastiCache Backup \(ElastiCache API\)<a name="backups-exporting-API"></a>

Export the backup to an Amazon S3 bucket using the `CopySnapshot` API operation with these parameters\.

**Parameters**
+ `SourceSnapshotName` – Name of the backup to be copied\.
+ `TargetSnapshotName` – Name of the backup's copy\.

  The name must be between 1 and 1,000 characters and able to be UTF\-8 encoded\.

  ElastiCache will add an instance identifier and `.rdb` to the value you enter here\. For example, if you enter `my-exported-backup`, you will get `my-exported-backup-0001.rdb`\.
+ `TargetBucket` – Name of the Amazon S3 bucket where you want to export the backup\. A copy of the backup is made in the specified bucket\.

  The `TargetBucket` must be an Amazon S3 bucket in the backup's region with the following permissions for the export process to succeed\.
  + Object access – **Read** and **Write**\.
  + Permissions access – **Read**\.

  For more information, see [Step 2: Grant ElastiCache Access to Your Amazon S3 Bucket](#backups-exporting-grant-access)\.

The following example makes a copy of an automatic backup to the Amazon S3 bucket `my-s3-bucket`\.

**Example**  

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=CopySnapshot
    &SourceSnapshotName=automatic.my-redis-primary-2016-06-27-03-15
    &TargetBucket=my-s3-bucket
    &TargetSnapshotName=my-backup-copy
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20141201T220302Z
    &Version=2016-01-01
    &X-Amz-Algorithm=AWS4-HMAC-SHA256
    &X-Amz-Date=20141201T220302Z
    &X-Amz-SignedHeaders=Host
    &X-Amz-Expires=20141201T220302Z
    &X-Amz-Credential=<credential>
    &X-Amz-Signature=<signature>
```

**Note**  
If your S3 bucket does not have the permissions needed for ElastiCache to export a backup to it, you will receive one of the following error messages\. Return to [Step 2: Grant ElastiCache Access to Your Amazon S3 Bucket](#backups-exporting-grant-access) to add the permissions specified and retry exporting your backup\.  
ElastiCache has not been granted READ permissions %s on the S3 Bucket\.  
**Solution:** Add Read permissions on the bucket\.
ElastiCache has not been granted WRITE permissions %s on the S3 Bucket\.  
**Solution:** Add Write permissions on the bucket\.
ElastiCache has not been granted READ\_ACP permissions %s on the S3 Bucket\.  
**Solution:** Add **Read** for Permissions access on the bucket\.

For more information, see [CopySnapshot](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CopySnapshot.html) in the *Amazon ElastiCache API Reference*\.

If you want to copy your backup to another region, use Amazon S3 copy to copy the exported backup to the Amazon S3 bucket in another region\. For more information, see [Copying an Object](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/MakingaCopyofanObject.html) in the *Amazon Simple Storage Service Console User Guide*\.