# Purchasing a reserved node<a name="reserved-nodes-purchasing"></a>

The following examples show how to purchase a reserved node offering using the AWS Management Console, the AWS CLI, and the ElastiCache API\. 

**Important**  
 Following the examples in this section incurs charges on your AWS account that you can't reverse\. 

**Topics**
+ [Using the AWS Management Console](#reserved-nodes-purchasing-console)
+ [Using the AWS CLI](#reserved-nodes-purchasing-cli)
+ [Using the ElastiCache API](#reserved-nodes-purchasing-api)

### Purchasing a reserved node \(Console\)<a name="reserved-nodes-purchasing-console"></a>

 This example shows purchasing a specific reserved node offering, *649fd0c8\-cf6d\-47a0\-bfa6\-060f8e75e95f*, with a reserved node ID of *myreservationID*\. 

The following procedure uses the AWS Management Console to purchase the reserved node offering by offering id\.

**To purchase reserved nodes**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation list, choose the **Reserved Cache Nodes** link\.

1. Choose the **Purchase Reserved Cache Node** button\.

1. Choose the node type from the **Product Description** drop\-down list box\.

1. Choose the node class from the **Cache Node Class** drop\-down list box\.

1. Choose length of time you want to reserve the node for from the **Term** drop\-down list box\.

1. Do either one of the following:
   +  Choose the offering type from the **Offering Type** drop\-down list box\.
   + Enter a reserved node ID in the **Reserved Cache Node ID** text box\. 
**Note**  
The Reserved Cache Node ID is a unique customer\-specified identifier to track this reservation\. If this box is left blank, ElastiCache automatically generates an identifier for the reservation\.

1.  Choose the **Next** button\. 

    The **Purchase Reserved Cache Node** dialog box shows a summary of the reserved node attributes that you've chosen and the payment due\. 

1.  Choose the **Yes, Purchase** button to proceed and purchase the reserved node\. 
**Important**  
When you choose **Yes, Purchase** you incur the charges for the reserved nodes you selected\. To avoid incurring these charges, choose **Cancel**\.

### Purchasing a reserved node \(AWS CLI\)<a name="reserved-nodes-purchasing-cli"></a>

 The following example shows purchasing the specific reserved cluster offering, *649fd0c8\-cf6d\-47a0\-bfa6\-060f8e75e95f*, with a reserved node ID of *myreservationID*\. 

 Type the following command at a command prompt: 

For Linux, OS X, or Unix:

```
1. aws elasticache purchase-reserved-cache-nodes-offering \
2.     --reserved-cache-nodes-offering-id 649fd0c8-cf6d-47a0-bfa6-060f8e75e95f \
3.     --reserved-cache-node-id myreservationID
```

For Windows:

```
1. aws elasticache purchase-reserved-cache-nodes-offering ^
2.     --reserved-cache-nodes-offering-id 649fd0c8-cf6d-47a0-bfa6-060f8e75e95f ^
3.     --reserved-cache-node-id myreservationID
```

 The command returns output similar to the following: 

```
1. RESERVATION  ReservationId      Class           Start Time                Duration  Fixed Price  Usage Price  Count  State            Description      Offering Type
2. RESERVATION  myreservationid    cache.m1.small  2013-12-19T00:30:23.247Z  1y        455.00 USD   0.092 USD    1      payment-pending  memcached        Medium Utilization
```

For more information, see [purchase\-reserved\-cache\-nodes\-offering](https://docs.aws.amazon.com/cli/latest/reference/elasticache/purchase-reserved-cache-nodes-offering.html) in the AWS CLI Reference\.

### Purchasing a reserved node \(ElastiCache API\)<a name="reserved-nodes-purchasing-api"></a>

The following example shows purchasing the specific reserved node offering, *649fd0c8\-cf6d\-47a0\-bfa6\-060f8e75e95f*, with a reserved cluster ID of *myreservationID*\. 

Call the `PurchaseReservedCacheNodesOffering` operation with the following parameters:
+ `ReservedCacheNodesOfferingId` = `649fd0c8-cf6d-47a0-bfa6-060f8e75e95f`
+ `ReservedCacheNodeID` = `myreservationID`
+ `CacheNodeCount` = `1`

**Example**  

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=PurchaseReservedCacheNodesOffering
    &ReservedCacheNodesOfferingId=649fd0c8-cf6d-47a0-bfa6-060f8e75e95f
    &ReservedCacheNodeID=myreservationID
    &CacheNodeCount=1
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
<PurchaseReservedCacheNodesOfferingResponse xmlns="http://elasticache.us-west-2.amazonaws.com/doc/2013-06-15/">
  <PurchaseReservedCacheNodesOfferingResult>
    <ReservedCacheNode>
      <OfferingType>Medium Utilization</OfferingType>
      <CurrencyCode>USD</CurrencyCode>
      <RecurringCharges/>
      <ProductDescription>memcached</ProductDescription>
      <ReservedCacheNodesOfferingId>649fd0c8-cf6d-47a0-bfa6-060f8e75e95f</ReservedCacheNodesOfferingId>
      <State>payment-pending</State>
      <ReservedCacheNodeId>myreservationID</ReservedCacheNodeId>
      <CacheNodeCount>10</CacheNodeCount>
      <StartTime>2013-07-18T23:24:56.577Z</StartTime>
      <Duration>31536000</Duration>
      <FixedPrice>123.0</FixedPrice>
      <UsagePrice>0.123</UsagePrice>
      <CacheNodeType>cache.m1.small</CacheNodeType>
    </ReservedCacheNode>
  </PurchaseReservedCacheNodesOfferingResult>
  <ResponseMetadata>
    <RequestId>7f099901-29cf-11e1-bd06-6fe008f046c3</RequestId>
  </ResponseMetadata>
</PurchaseReservedCacheNodesOfferingResponse>
```

For more information, see [PurchaseReservedCacheNodesOffering](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_PurchaseReservedCacheNodesOffering.html) in the ElastiCache API Reference\.