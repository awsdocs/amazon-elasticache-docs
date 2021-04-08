# Step 4: Connect to the cluster's node<a name="GettingStarted.ConnectToCacheNode"></a>

Before you continue, be sure you have completed [Step 3: Authorize access to the cluster](GettingStarted.AuthorizeAccess.md)\.

This section assumes that you've created an Amazon EC2 instance and can connect to it\. For instructions on how to do this, see the [Amazon EC2 Getting Started Guide](https://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)\. 

An Amazon EC2 instance can connect to a cluster node only if you have authorized it to do so\. For more information, see [Step 3: Authorize access to the cluster](GettingStarted.AuthorizeAccess.md)\.

## Find your node endpoints<a name="GettingStarted.FindEndpoints"></a>

When your cluster is in the *available* state and you've authorized access to it \([Step 3: Authorize access to the cluster](GettingStarted.AuthorizeAccess.md)\), you can log in to an Amazon EC2 instance and connect to the cluster\. To do so, you must first determine the endpoint\.

To find your endpoints, see the relevant topic from the following list\. When you find the endpoint you need, copy it to your clipboard for use in the next section\.
+ [Finding connection endpoints](Endpoints.md)
+ [Finding a Cluster's Endpoints \(Console\)](Endpoints.md#Endpoints.Find.Memcached)—You need the cluster's Configuration endpoint\.
+ [Finding Endpoints \(AWS CLI\)](Endpoints.md#Endpoints.Find.CLI)
+ [Finding Endpoints \(ElastiCache API\)](Endpoints.md#Endpoints.Find.API)

## Connect to a Memcached cluster<a name="GettingStarted.ConnectToCacheNode.Memcached"></a>

Once your cluster is in the *available* state and you've authorized access to it \([Step 3: Authorize access to the cluster](GettingStarted.AuthorizeAccess.md), you can log in to an Amazon EC2 instance and connect to the cluster\.

For instructions on connecting to a Memcached cluster, see [Connecting to nodes](nodes-connecting.md)\.