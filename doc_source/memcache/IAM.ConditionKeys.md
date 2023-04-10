# Using condition keys<a name="IAM.ConditionKeys"></a>

You can specify conditions that determine how an IAM policy takes effect\. In ElastiCache, you can use the `Condition` element of a JSON policy to compare keys in the request context with key values that you specify in your policy\. For more information, see [IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html)\. For a list of global condition keys, see [AWS global condition context keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html)\.

## Specifying Conditions: Using Condition Keys<a name="IAM.SpecifyingConditions"></a>

To implement fine\-grained control, you write an IAM permissions policy that specifies conditions to control a set of individual parameters on certain requests\. You then apply the policy to IAM users, groups, or roles that you create using the IAM console\. 

To apply a condition, you add the condition information to the IAM policy statement\. In the following example, you specify the condition that any cache cluster created will be of node type `cache.r5.large`\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "elasticache:CreateCacheCluster"
            ],
            "Resource": [
                "arn:aws:elasticache:*:*:parametergroup:*",
                "arn:aws:elasticache:*:*:subnetgroup:*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "elasticache:CreateCacheCluster"
            ],
            "Resource": [
                "arn:aws:elasticache:*:*:cluster:*"
            ],
            "Condition": {
                "StringEquals": {
                    "elasticache:CacheNodeType": [
                        "cache.r5.large"
                    ]
                }
            }
        }
    ]
}
```

The following table shows the service\-specific condition keys that apply to ElastiCache and the actions that use them\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/IAM.ConditionKeys.html)

For more information, see [Tag\-Based access control policy examples](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/Tagging-Resources.html)\. 

For more information on using policy condition operators, see [ElastiCache API permissions: Actions, resources, and conditions reference](IAM.APIReference.md)\.

## Example Policies: Using Conditions for Fine\-Grained Parameter Control<a name="IAM.ExamplePolicies"></a>

This section shows example policies for implementing fine\-grained access control on the previously listed ElastiCache parameters\.

1. **elasticache:CacheNodeType**:   Specify which NodeType\(s\) a user can create\. Using the provided conditions, the customer can specify a single or a range value for a node type\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
            {
               "Effect": "Allow",
               "Action": [
                   "elasticache:CreateCacheCluster"
               ],
               "Resource": [
                   "arn:aws:elasticache:*:*:parametergroup:*",
                   "arn:aws:elasticache:*:*:subnetgroup:*"
               ]
           },
   
           {
               "Effect": "Allow",
               "Action": [
                   "elasticache:CreateCacheCluster"
                   "elasticache:CreateReplicationGroup"
               ],
               "Resource": [
                   "arn:aws:elasticache:*:*:cluster:*"
                   "arn:aws:elasticache:*:*:replicationgroup:*"
               ],
               "Condition": {
                   "StringEquals": {
                       "elasticache:CacheNodeType": [
                           "cache.t2.micro",
                           "cache.t2.medium"
                       ]
                   }
               }
           }
       ]
   }
   ```

1. **elasticache:EngineVersion**:   Specify usage of engine version 1\.6\.6

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
        {
               "Effect": "Allow",
               "Action": [
                   "elasticache:CreateCacheCluster"
               ],
               "Resource": [
                   "arn:aws:elasticache:*:*:parametergroup:*",
                   "arn:aws:elasticache:*:*:subnetgroup:*"
               ]
           },
   
           {
               "Effect": "Allow",
               "Action": [
                   "elasticache:CreateCacheCluster"
                   "elasticache:CreateReplicationGroup"
               ],
               "Resource": [
                   "arn:aws:elasticache:*:*:cluster:*"
                   "arn:aws:elasticache:*:*:replicationgroup:*"
               ],
               "Condition": {
                   "StringEquals": {
   
                       "elasticache:EngineVersion": "1.6.6"
                   }
               }
           }
       ]
   }
   ```

1. **elasticache:CacheParameterGroupName**:   Specify a non default parameter group with specific parameters from an organization on your clusters\. You could also specify a naming pattern for your parameter groups or block delete on a specific parameter group name\. Following is an example constraining usage of only "my\-org\-param\-group"\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
   
            {
               "Effect": "Allow",
               "Action": [
                   "elasticache:CreateCacheCluster"
               ],
               "Resource": [
                   "arn:aws:elasticache:*:*:parametergroup:*",
                   "arn:aws:elasticache:*:*:subnetgroup:*"
               ]
           },
   
           {
               "Effect": "Allow",
               "Action": [
                   "elasticache:CreateReplicationGroup"
               ],
               "Resource": [
                   "arn:aws:elasticache:*:*:cluster:*"
               ],
               "Condition": {
                   "StringEquals": {
                       "elasticache:CacheParameterGroupName": "my-org-param-group"
                   }
               }
           }
       ]
   }
   ```

1. **elasticache:CreateCacheCluster**: Denying `CreateCacheCluster` action if the request tag `Project` is missing or is not equal to `Dev`, `QA` or `Prod`\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
             {
               "Effect": "Allow",
               "Action": [
                   "elasticache:CreateCacheCluster"
               ],
               "Resource": [
                   "arn:aws:elasticache:*:*:parametergroup:*",
                   "arn:aws:elasticache:*:*:subnetgroup:*",
                   "arn:aws:elasticache:*:*:securitygroup:*",
                   "arn:aws:elasticache:*:*:replicationgroup:*"
               ]
           },
           {
               "Effect": "Deny",
               "Action": [
                   "elasticache:CreateCacheCluster"
               ],
               "Resource": [
                   "arn:aws:elasticache:*:*:cluster:*"
               ],
               "Condition": {
                   "Null": {
                       "aws:RequestTag/Project": "true"
                   }
               }
           },
           {
               "Effect": "Allow",
               "Action": [
                   "elasticache:CreateCacheCluster",
                   "elasticache:AddTagsToResource"
               ],
               "Resource": "arn:aws:elasticache:*:*:cluster:*",
               "Condition": {
                   "StringEquals": {
                       "aws:RequestTag/Project": [
                           "Dev",
                           "Prod",
                           "QA"
                       ]
                   }
               }
           }
       ]
   }
   ```

1. **elasticache:createcachecluster**:   Allowing `CreateCacheCluster` with `cacheNodeType` cache\.r5\.large or cache\.r6g\.4xlarge and tag `Project=XYZ`\. 

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
         {
         "Effect": "Allow",
         "Action": [
           "elasticache:CreateCacheCluster"
         ],
         "Resource": [
           "arn:aws:elasticache:*:*:parametergroup:*",
           "arn:aws:elasticache:*:*:subnetgroup:*"
         ]
       },
       {
         "Effect": "Allow",
         "Action": [
           "elasticache:CreateCacheCluster"
         ],
         "Resource": [
           "arn:aws:elasticache:*:*:cluster:*"
         ],
         "Condition": {
           "StringEqualsIfExists": {
             "elasticache:CacheNodeType": [
               "cache.r5.large",
               "cache.r6g.4xlarge"
             ]
           },
           "StringEquals": {
             "aws:RequestTag/Project": "XYZ"
           }
         }
       }
     ]
   }
   ```

**Note**  
When creating polices to enforce tags and other condition keys together, the conditional `IfExists` may be required on condition key elements due to the extra `elasticache:AddTagsToResource` policy requirements for creation requests with the `--tags` parameter\.