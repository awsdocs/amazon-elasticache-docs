# Getting info about your reserved nodes<a name="reserved-nodes-describing"></a>

You can get information about the reserved nodes you've purchased using the AWS Management Console, the AWS CLI, and the ElastiCache API\.

**Topics**
+ [Using the AWS Management Console](#reserved-nodes-describing-console)
+ [Using the AWS CLI](#reserved-nodes-describing-cli)
+ [Using the ElastiCache API](#reserved-nodes-describing-api)

### Getting info about your reserved nodes \(Console\)<a name="reserved-nodes-describing-console"></a>

The following procedure describes how to use the AWS Management Console to get information about the reserved nodes you purchased\.

**To get information about your purchased reserved nodes**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation list, choose the **Reserved Cache Nodes** link\.

    The reserved nodes for your account appear in the Reserved Cache Nodes list\. You can choose any of the reserved nodes in the list to see detailed information about the reserved node in the detail pane at the bottom of the console\. 

### Getting info about your reserved nodes \(AWS CLI\)<a name="reserved-nodes-describing-cli"></a>

To get information about reserved nodes for your AWS account, type the following command at a command prompt:

```
aws elasticache describe-reserved-cache-nodes
```

This operation produces output similar to the following \(JSON format\):

```
{
    "ReservedCacheNodeId": "myreservationid",
    "ReservedCacheNodesOfferingId": "649fd0c8-cf6d-47a0-bfa6-060f8e75e95f",
    "CacheNodeType": "cache.m1.small",
    "Duration": "31536000",
    "ProductDescription": "memcached",
    "OfferingType": "Medium Utilization",
    "MaxRecords": 0
}
```

For more information, see [describe\-\-reserved\-cache\-nodes](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-reserved-cache-nodes.html) in the AWS CLI Reference\.

### Getting info about your reserved nodes \(ElastiCache API\)<a name="reserved-nodes-describing-api"></a>

To get information about reserved nodes for your AWS account, call the `DescribeReservedCacheNodes` operation\.

**Example**  

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=DescribeReservedCacheNodes
    &Version=2014-12-01
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20141201T220302Z
    &X-Amz-Algorithm=&AWS;4-HMAC-SHA256
    &X-Amz-Date=20141201T220302Z
    &X-Amz-SignedHeaders=Host
    &X-Amz-Expires=20141201T220302Z
    &X-Amz-Credential=<credential>
    &X-Amz-Signature=<signature>
```
This call returns output similar to the following:   

```
<DescribeReservedCacheNodesResponse xmlns="http://elasticache.us-west-2.amazonaws.com/doc/2013-06-15/">
  <DescribeReservedCacheNodesResult>
    <ReservedCacheNodes>
      <ReservedCacheNode>
        <OfferingType>Medium Utilization</OfferingType>
        <CurrencyCode>USD</CurrencyCode>
        <RecurringCharges/>
        <ProductDescription>memcached</ProductDescription>
        <ReservedCacheNodesOfferingId>649fd0c8-cf6d-47a0-bfa6-060f8e75e95f</ReservedCacheNodesOfferingId>
        <State>payment-failed</State>
        <ReservedCacheNodeId>myreservationid</ReservedCacheNodeId>
        <CacheNodeCount>1</CacheNodeCount>
        <StartTime>2010-12-15T00:25:14.131Z</StartTime>
        <Duration>31536000</Duration>
        <FixedPrice>227.5</FixedPrice>
        <UsagePrice>0.046</UsagePrice>
        <CacheNodeType>cache.m1.small</CacheNodeType>
      </ReservedCacheNode>
      <ReservedCacheNode>

      (...some output omitted for brevity...)

      </ReservedCacheNode>
    </ReservedCacheNodes>
  </DescribeReservedCacheNodesResult>
  <ResponseMetadata>
    <RequestId>23400d50-2978-11e1-9e6d-771388d6ed6b</RequestId>
  </ResponseMetadata>
</DescribeReservedCacheNodesResponse>
```

For more information, see [DescribeReservedCacheNodes](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReservedCacheNodes.html) in the ElastiCache API Reference\.