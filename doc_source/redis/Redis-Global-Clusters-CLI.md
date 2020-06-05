# Using Global Datastores \(CLI\)<a name="Redis-Global-Clusters-CLI"></a>

You can use the AWS Command Line Interface \(AWS CLI\) to control multiple AWS services from the command line and automate them through scripts\. You can use the AWS CLI for ad hoc \(one\-time\) operations\. 

## Downloading and Configuring the AWS CLI<a name="Redis-Global-Clusters-Downloading-CLI"></a>

The AWS CLI runs on Windows, macOS, or Linux\. Use the following procedure to download and configure it\.

**To download, install, and configure the CLI**

1. Download the AWS CLI on the [AWS Command Line Interface](http://aws.amazon.com/cli) webpage\.

1. Follow the instructions for Installing the AWS CLI and Configuring the AWS CLI in the *AWS Command Line Interface User Guide*\.

## Using the AWS CLI with Global Datastores<a name="Redis-Global-Clusters-Using-CLI"></a>

Use the following CLI operations to work with global datastores: 
+ [create\-global\-replication\-group](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateGlobalReplicationGroup.html)

  ```
  aws elasticache create-global-replication-group \
     --global-replication-group-id-suffix my global datastore \
     --primary-replication-group-id sample-repl-group \
     --global-replication-group-description an optional description of the global datastore
  ```
+  [create\-replication\-group](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateReplicationGroup.html) – Use this operation to create secondary clusters for a Global Datastore by supplying the name of the Global Datastore to the `--global-replication-group-id` parameter\.

  ```
  aws elasticache create-replication-group \
    --replication-group-id secondary replication group name \
    --replication-group-description “Replication group description" \
    --global-replication-group-id global datastore name \
  ```
+ [describe\-global\-replication\-groups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeGlobalReplicationGroups.html)

  ```
  aws elasticache describe-global-replication-groups \
     --global-replication-group-id my global datastore \
     --show-member-info an optional parameter that returns a list of the primary and secondary clusters that make up the global datastore
  ```
+ [modify\-global\-replication\-group](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyGlobalReplicationGroup.html)

  ```
  aws elasticache modify-global-replication-group \
     --global-replication-group-id my global datastore \
     --automatic-failover-enabled yes/no 
     --cache-node-type node type              
     --engine-version engine version
     -—apply-immediately
     --global-replication-group-description description
  ```
+ [delete\-global\-replication\-group](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DeleteGlobalReplicationGroup.html)

  ```
  aws elasticache delete-global-replication-group \
     --global-replication-group-id my global datastore \
     --retain-primary-replication-group defaults to true
  ```
+ [disassociate\-global\-replication\-group](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DisassociateGlobalReplicationGroup.html)

  ```
  aws elasticache disassociate-global-replication-group \
     --global-replication-group-id my Global Datastore \
     --replication-group-id my secondary cluster \  
     --replication-group-region the AWS Region in which the secondary cluster resides
  ```
+ [failover\-global\-replication\-group](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_FailoverGlobalReplicationGroup.html)

  ```
  aws elasticache failover-replication-group \
     --global-replication-group-id my global datastore \
     --primary-region The AWS Region of the primary cluster \  
     --primary-replication-group-id  The name of the global datastore, including the suffix.
  ```
+ [increase\-node\-groups\-in\-global\-replication\-group](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_IncreaseNodeGroupsInGlobalReplicationGroup.html)

  ```
  aws elasticache increase-node-groups-in-global-replication-group \
     --apply-immediately yes\
     --global-replication-group-id global-replication-group-name \
     --node-group-count  3
  ```
+ [decrease\-node\-groups\-in\-global\-replication\-group](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DecreaseNodeGroupsInGlobalReplicationGroup.html)

  ```
  aws elasticache decrease-node-groups-in-global-replication-group \
     --apply-immediately yes\
     --global-replication-group-id global-replication-group-name \
     --node-group-count  3
  ```
+ [rebalance\-shards\-in\-global\-replication\-group](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_RebalanceSlotsInGlobalReplicationGroup.html)

  ```
  aws elasticache rebalance-shards-in-global-replication-group \
     --apply-immediately yes\
     --global-replication-group-id global-replication-group-name \
  ```

Use help to list all available commands ElastiCache for Redis\.

```
aws elasticache help
```

You can also use help to describe a specific command and learn more about its usage: 

```
aws elasticache create-global-replication-group help
```