# Viewing a Replication Group's Details \(ElastiCache API\)<a name="Replication.ViewDetails.API"></a>

You can view the details for a replication using the AWS CLI `DescribeReplicationGroups` operation\. Use the following optional parameters to refine the listing\. Omitting the parameters returns the details for up to 100 replication groups\.

**Optional Parameters**
+ `ReplicationGroupId` – Use this parameter to list the details of a specific replication group\. If the specified replication group has more than one node group, results are returned grouped by node group\.
+ `MaxRecords` – Use this parameter to limit the number of replication groups listed\. The value of `MaxRecords` cannot be less than 20 or greater than 100\. The default is 100\.

**Example**  
The following code list the details for up to 100 replication groups\.  

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DescribeReplicationGroups
   &Version=2015-02-02
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```
The following code lists the details for `myReplGroup`\.  

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DescribeReplicationGroups
   &ReplicationGroupId=myReplGroup
   &Version=2015-02-02
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```
The following code list the details for up to 25 clusters\.  

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DescribeReplicationGroups
   &MaxRecords=25
   &Version=2015-02-02
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20150202T192317Z
   &X-Amz-Credential=<credential>
```

For more information, see the ElastiCache API reference topic [DescribeReplicationGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReplicationGroups.html)\.