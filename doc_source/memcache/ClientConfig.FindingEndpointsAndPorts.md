# Finding node endpoints and port numbers<a name="ClientConfig.FindingEndpointsAndPorts"></a>

To connect to a cache node, your application needs to know the endpoint and port number for that node\.

## Finding node endpoints and port numbers \(Console\)<a name="ClientConfig.FindingEndpointsAndPorts.CON"></a>

 **To determine node endpoints and port numbers** 

1. Sign in to the [Amazon ElastiCache management console](http://aws.amazon.com/elasticache) and choose the engine running on your cluster\.

   A list of all clusters running the chosen engine appears\.

1. Continue below for the engine and configuration you're running\.

1. Choose the name of the cluster of interest\.

1. Locate the **Port** and **Endpoint** columns for the node you're interested in\.

## Finding cache node endpoints and port numbers \(AWS CLI\)<a name="ClientConfig.FindingEndpointsAndPorts.CLI"></a>

To determine cache node endpoints and port numbers, use the command `describe-cache-clusters` with the `--show-cache-node-info` parameter\.

```
aws elasticache describe-cache-clusters --show-cache-node-info 
```

The fully qualified DNS names and port numbers are in the Endpoint section of the output\.

## Finding cache node endpoints and port numbers \(ElastiCache API\)<a name="ClientConfig.FindingEndpointsAndPorts.API"></a>

To determine cache node endpoints and port numbers, use the action `DescribeCacheClusters` with the `ShowCacheNodeInfo=true` parameter\.

**Example**  

```
 1. https://elasticache.us-west-2.amazonaws.com /
 2.     ?Action=DescribeCacheClusters
 3.     &ShowCacheNodeInfo=true
 4.     &SignatureVersion=4
 5.     &SignatureMethod=HmacSHA256
 6.     &Timestamp=20140421T220302Z
 7.     &Version=2014-09-30   
 8.     &X-Amz-Algorithm=&AWS;4-HMAC-SHA256
 9.     &X-Amz-Credential=<credential>
10.     &X-Amz-Date=20140421T220302Z
11.     &X-Amz-Expires=20140421T220302Z
12.     &X-Amz-Signature=<signature>
13.     &X-Amz-SignedHeaders=Host
```