# Deleting a Replication Group<a name="Replication.DeletingRepGroup"></a>

If you no longer need one of your clusters with replicas \(called *replication groups* in the API/CLI\), you can delete it\. When you delete a replication group, ElastiCache deletes all of the nodes in that group\.

After you have begun this operation, it cannot be interrupted or canceled\.

**Warning**  
When you delete an ElastiCache for Redis cluster, your manual snapshots are retained\. You will also have an option to create a final snapshot before the cluster is deleted\. Automatic cache snapshots are not retained\.

## Deleting a Replication Group \(Console\)<a name="Replication.DeletingRepGroup.CON"></a>

To delete a cluster that has replicas, see [Deleting a Cluster](Clusters.Delete.md)\.

## Deleting a Replication Group \(AWS CLI\)<a name="Replication.DeletingRepGroup.CLI"></a>

Use the command [delete\-replication\-group](https://docs.aws.amazon.com/AmazonElastiCache/latest/CommandLineReference/CLIReference-cmd-DeleteReplicationGroup.html) to delete a replication group\.

```
aws elasticache delete-replication-group --replication-group-id my-repgroup 
```

A prompt asks you to confirm your decision\. Enter *y* \(yes\) to start the operation immediately\. After the process starts, it is irreversible\.

```
						
   After you begin deleting this replication group, all of its nodes will be deleted as well.
   Are you sure you want to delete this replication group? [Ny]y

REPLICATIONGROUP  my-repgroup  My replication group  deleting
```

## Deleting a Replication Group \(ElastiCache API\)<a name="Replication.DeletingRepGroup.API"></a>

Call [DeleteReplicationGroup](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteReplicationGroup.html) with the `ReplicationGroup` parameter\. 

**Example**  

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DeleteReplicationGroup
   &ReplicationGroupId=my-repgroup
   &Version=2014-12-01
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &Timestamp=20141201T220302Z
   &X-Amz-Algorithm=AWS4-HMAC-SHA256
   &X-Amz-Date=20141201T220302Z
   &X-Amz-SignedHeaders=Host
   &X-Amz-Expires=20141201T220302Z
   &X-Amz-Credential=<credential>
   &X-Amz-Signature=<signature>
```

**Note**  
If you set the `RetainPrimaryCluster` parameter to `true`, all of the read replicas will be deleted, but the primary cluster will be retained\.