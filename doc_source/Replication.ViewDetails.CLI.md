# Viewing a Replication Group's Details: \(AWS CLI\)<a name="Replication.ViewDetails.CLI"></a>

You can view the details for a replication using the AWS CLI `describe-replication-groups` command\. Use the following optional parameters to refine the listing\. Omitting the parameters returns the details for up to 100 replication groups\.

**Optional Parameters**

+ `--replication-group-id` – Use this parameter to list the details of a specific replication group\. If the specified replication group has more than one node group, results are returned grouped by node group\.

+ `--max-items` – Use this parameter to limit the number of replication groups listed\. The value of `--max-items` cannot be less than 20 or greater than 100\.

**Example**  
The following code lists the details for up to 100 replication groups\.  

```
aws elasticache describe-replication-groups
```
The following code lists the details for `my-repl-group`\.  

```
aws elasticache describe-replication-groups --replication-group-id my-repl-group
```
The following code lists the details for `new-group`\.  

```
aws elasticache describe-replication-groups --replication-group-id new-group
```
The following code list the details for up to 25 replication groups\.  

```
aws elasticache describe-replication-groups --max-items 25
```
Output from this operation should look something like this \(JSON format\)\.  

```
{
   "ReplicationGroups": [
     {
       "Status": "available", 
       "Description": "test", 
       "NodeGroups": [
         {
            "Status": "available", 
               "NodeGroupMembers": [
                  {
                     "CurrentRole": "primary", 
                     "PreferredAvailabilityZone": "us-west-2a", 
                     "CacheNodeId": "0001", 
                     "ReadEndpoint": {
                        "Port": 6379, 
                        "Address": "rg-name-001.1abc4d.0001.usw2.cache.amazonaws.com"
                     }, 
                     "CacheClusterId": "rg-name-001"
                  }, 
                  {
                     "CurrentRole": "replica", 
                     "PreferredAvailabilityZone": "us-west-2b", 
                     "CacheNodeId": "0001", 
                     "ReadEndpoint": {
                        "Port": 6379, 
                        "Address": "rg-name-002.1abc4d.0001.usw2.cache.amazonaws.com"
                     }, 
                     "CacheClusterId": "rg-name-002"
                  }, 
                  {
                     "CurrentRole": "replica", 
                     "PreferredAvailabilityZone": "us-west-2c", 
                     "CacheNodeId": "0001", 
                     "ReadEndpoint": {
                        "Port": 6379, 
                        "Address": "rg-name-003.1abc4d.0001.usw2.cache.amazonaws.com"
                     }, 
                     "CacheClusterId": "rg-name-003"
                  }
               ], 
               "NodeGroupId": "0001", 
               "PrimaryEndpoint": {
                  "Port": 6379, 
                  "Address": "rg-name.1abc4d.ng.0001.usw2.cache.amazonaws.com"
               }
            }
         ], 
         "ReplicationGroupId": "rg-name", 
         "AutomaticFailover": "enabled", 
         "SnapshottingClusterId": "rg-name-002", 
         "MemberClusters": [
            "rg-name-001", 
            "rg-name-002", 
            "rg-name-003"
         ], 
         "PendingModifiedValues": {}
      }, 
      {
      ... some output omitted for brevity
      }
   ]
}
```

For more information, see the AWS CLI for ElastiCache topic [describe\-replication\-groups](http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-replication-groups.html)\.