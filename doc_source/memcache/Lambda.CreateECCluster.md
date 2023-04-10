# Step 1: Create an ElastiCache cluster<a name="Lambda.CreateECCluster"></a>

In the step you create an Amazon ElastiCache cluster in the default Amazon Virtual Private Cloud in the us\-east\-1 region in your account using the AWS CLI\. For information on creating ElastiCache clusters using the ElastiCache console or API, see [Creating a cluster](Clusters.Create-mc.md) in the *ElastiCache User Guide*\.

1. Run the following AWS CLI command to create a Memcached cluster in the default VPC in the us\-east\-1 region\.

   For Linux, macOS, or Unix:

   ```
   aws elasticache create-cache-cluster \
       --cache-cluster-id CacheClusterForLambda \
       --cache-node-type cache.m3.medium \
       --engine memcached \   
       --num-cache-nodes 1
   ```

   For Windows:

   ```
   aws elasticache create-cache-cluster ^
       --cache-cluster-id CacheClusterForLambda ^
       --cache-node-type cache.m3.medium ^
       --engine memcached ^  
       --num-cache-nodes 1
   ```

   You can look up the default VPC security group in the VPC console under **Security Groups**\. Your example Lambda function will add and retrieve an item from this cluster\.

1. Write down the configuration endpoint for the cache cluster that you launched\. You can get this from the ElastiCache console \(see [Finding connection endpoints](Endpoints.md)\)\. You will specify this value in your Lambda function code in the next section\.

**Next Step:**

[Step 2: Create a Lambda function](Lambda.CreateLambdaFunction.md)