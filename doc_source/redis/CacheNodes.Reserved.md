# ElastiCache reserved nodes<a name="CacheNodes.Reserved"></a>

Reserving one or more nodes might be a way for you to reduce costs\. Reserved nodes are charged an up front fee that depends upon the node type and the length of reservation— one or three years\. 

To see if reserved nodes are a cost savings for your use cases, first determine the node size and number of nodes you need\. Then estimate the usage of the node, and compare the total cost to you of using On\-Demand nodes versus reserved nodes\. You can mix and match reserved and On\-Demand node usage in your clusters\. For pricing information, see [Amazon ElastiCache Pricing](https://aws.amazon.com/elasticache/pricing/)\.

**Note**  
Reserved nodes are not flexible; they only apply to the exact instance type that you reserve\.

## Managing costs with reserved nodes<a name="reserved-nodes"></a>

Reserving one or more nodes may be a way for you to reduce costs\. Reserved nodes are charged an up front fee that depends upon the node type and the length of reservation—one or three years\. This charge is much less than the hourly usage charge that you incur with On\-Demand nodes\. 

To see if reserved nodes are a cost savings for your use cases, first determine the node size and number of nodes you need\. Then estimate the usage of the node, and compare the total cost to you using On\-Demand nodes versus reserved nodes\. You can mix and match reserved and On\-Demand node usage in your clusters\. For pricing information, see [Amazon ElastiCache Pricing](https://aws.amazon.com/elasticache/pricing/)\.

AWS Region, node type and term length must be chosen at purchase, and cannot be changed later\.

You can use the AWS Management Console, the AWS CLI, or the ElastiCache API to list and purchase available reserved node offerings\.

For more information on reserved nodes, see [Amazon ElastiCache Reserved Nodes](https://aws.amazon.com/elasticache/reserved-cache-nodes/)\.

**Topics**
+ [Standard reserved node offerings](#reserved-nodes-standard)
+ [Legacy reserved node offerings](#reserved-nodes-utilization)
+ [Getting info about reserved node offerings](#reserved-nodes-offerings)
+ [Purchasing a reserved node](#reserved-nodes-purchasing)
+ [Getting info about your reserved nodes](#reserved-nodes-describing)

### Standard reserved node offerings<a name="reserved-nodes-standard"></a>

When you purchase a standard reserved node instance \(RI\) in Amazon ElastiCache, you purchase a commitment to getting a discounted rate on a specific node instance type and AWS Region for the duration of the reserved node instance\. To use an Amazon ElastiCache reserved node instance, you create a new ElastiCache node instance, just as you would for an on\-demand instance\.

 The new node instance that you create must exactly match the specifications of the reserved node instance\. If the specifications of the new node instance match an existing reserved node instance for your account, you are billed at the discounted rate offered for the reserved node instance\. Otherwise, the node instance is billed at an on\-demand rate\. These standard RIs are available from R5 and M5 instance families onwards\. 

**Note**  
All three offering types discussed next are available in one\-year and three\-year terms\.

**Offering Types**  
**No Upfront ** RI provides access to a reserved ElastiCache instance without requiring an upfront payment\. Your *No Upfront* reserved ElastiCache instance bills a discounted hourly rate for every hour within the term, regardless of usage\. 

**Partial Upfront** RI requires a part of the reserved ElasticCache instance to be paid upfront\. The remaining hours in the term are billed at a discounted hourly rate, regardless of usage\. This option is the replacement for the legacy *Heavy Utilization* option, which is explained in the next section\.

**All Upfront** RI requires full payment to be made at the start of the RI term\. You incur no other costs for the remainder of the term, regardless of the number of hours used\. 

### Legacy reserved node offerings<a name="reserved-nodes-utilization"></a>

There are three levels of legacy node reservations—Heavy Utilization, Medium Utilization, and Light Utilization\. Nodes can be reserved at any utilization level for either one or three years\. The node type, utilization level, and reservation term affect your total costs\. Verify the savings that reserved nodes can provide your business by comparing various models before you purchase reserved nodes\.

Nodes purchased at one utilization level or term cannot be converted to a different utilization level or term\.

**Utilization Levels**  
*Heavy Utilization reserved nodes* enable workloads that have a consistent baseline of capacity or run steady\-state workloads\. Heavy Utilization reserved nodes require a high up\-front commitment, but if you plan to run more than 79 percent of the reserved node term you can earn the largest savings \(up to 70 percent off of the On\-Demand price\)\. With Heavy Utilization reserved nodes, you pay a one\-time fee\. This is then followed by a lower hourly fee for the duration of the term regardless of whether your node is running\.

*Medium Utilization reserved nodes* are the best option if you plan to use your reserved nodes a large amount of the time and you want either a lower one\-time fee or to stop paying for your node when you shut it off\. Medium Utilization reserved nodes are a more cost\-effective option when you plan to run more than 40 percent of the reserved nodes term\. This option can save you up to 64 percent off of the On\-Demand price\. With Medium Utilization reserved nodes, you pay a slightly higher one\-time fee than with Light Utilization reserved nodes, and you receive lower hourly usage rates when you run a node\.

*Light Utilization reserved nodes* are ideal for periodic workloads that run only a couple of hours a day or a few days per week\. Using Light Utilization reserved nodes, you pay a one\-time fee followed by a discounted hourly usage fee when your node is running\. You can start saving when your node is running more than 17 percent of the reserved node term\. You can save up to 56 percent off of the On\-Demand rates over the entire term of your reserved node\.


**Legacy reserved node offerings**  

| Offering | Up\-front cost | Usage fee | Advantage | 
| --- | --- | --- | --- | 
|  Heavy Utilization |  Highest |  Lowest hourly fee\. Applied to the whole term whether or not you're using the reserved node\. |  Lowest overall cost if you plan to run your reserved nodes more than 79 percent of a three\-year term\. | 
|  Medium Utilization |  Medium |  Hourly usage fee charged for each hour the node is running\. No hourly charge when the node is not running\. |  Suitable for elastic workloads or when you expect moderate usage, more than 40 percent of a three\-year term\. | 
|  Light Utilization |  Lowest |  Hourly usage fee charged for each hour the node is running\. No hourly charge when the node is not running\. Highest hourly fees of all the offering types, but fees apply only when the reserved node is running\. |  Highest overall cost if you plan to run all of the time\. However, this is the lowest overall cost if you plan to use your reserved node infrequently, more than about 15 percent of a three\-year term\. | 
|  On\-Demand Use \(No reserved nodes\) |  None |  Highest hourly fee\. Applied whenever the node is running\. |  Highest hourly cost\. | 

For more information, see [Amazon ElastiCache Pricing](https://aws.amazon.com/elasticache/pricing/)\.

### Getting info about reserved node offerings<a name="reserved-nodes-offerings"></a>

Before you purchase reserved nodes, you can get information about available reserved node offerings\. 

The following examples show how to get pricing and information about available reserved node offerings using the AWS Management Console, AWS CLI, and ElastiCache API\. 

**Topics**
+ [Using the AWS Management Console](#reserved-nodes-offerings-console)
+ [Using the AWS CLI](#reserved-nodes-offerings-cli)
+ [Using the ElastiCache API](#reserved-nodes-offerings-api)

#### Getting info about reserved node offerings \(Console\)<a name="reserved-nodes-offerings-console"></a>

To get pricing and other information about available reserved cluster offerings using the AWS Management Console, use the following procedure\.

**To get information about available reserved node offerings**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Reserved Nodes**\.

1. Choose **Purchase Reserved Node**\.

1. For **Engine**, choose Redis\.

1. To determine the available offerings, make selections for the following options:
   + **Node Type**
   + **Term**
   + **Offering Type**

   After you make these selections, the cost per node and total cost of your selections is shown under **Reservation details**\.

1. Choose **Cancel** to avoid purchasing these nodes and incurring charges\. 

#### Getting info about reserved node offerings \(AWS CLI\)<a name="reserved-nodes-offerings-cli"></a>

To get pricing and other information about available reserved node offerings, type the following command at a command prompt:

```
1. aws elasticache describe-reserved-cache-nodes-offerings
```

This operation produces output similar to the following \(JSON format\):

```
 {
            "ReservedCacheNodesOfferingId": "0xxxxxxxx-xxeb-44ex-xx3c-xxxxxxxx072",
            "CacheNodeType": "cache.m6g.large",
            "Duration": 94608000,
            "FixedPrice": 1758.0,
            "UsagePrice": 0.0,
            "ProductDescription": "redis",
            "OfferingType": "All Upfront",
            "RecurringCharges": [
                {
                    "RecurringChargeAmount": 0.0,
                    "RecurringChargeFrequency": "Hourly"
                }
            ]
  },
  {
            "ReservedCacheNodesOfferingId": "0xxxxxxxx-xxeb-44ex-xx3c-xxxxxxxx072",
            "CacheNodeType": "cache.m6g.xlarge",
            "Duration": 94608000,
            "FixedPrice": 1870.0,
            "UsagePrice": 0.0,
            "ProductDescription": "redis",
            "OfferingType": "Partial Upfront",
            "RecurringCharges": [
                {
                    "RecurringChargeAmount": 0.072,
                    "RecurringChargeFrequency": "Hourly"
                }
            ]
  },
  {
            "ReservedCacheNodesOfferingId": "0xxxxxxxx-xxeb-44ex-xx3c-xxxxxxxx072",
            "CacheNodeType": "cache.m6g.large",
            "Duration": 31536000,
            "FixedPrice": 0.0,
            "UsagePrice": 0.0,
            "ProductDescription": "redis",
            "OfferingType": "No Upfront",
            "RecurringCharges": [
                {
                    "RecurringChargeAmount": 0.102,
                    "RecurringChargeFrequency": "Hourly"
                }
            ]
}
```

For more information, see [describe\-reserved\-cache\-nodes\-offerings](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-reserved-cache-nodes-offerings.html) in the AWS CLI Reference\.

#### Getting info about reserved node offerings \(ElastiCache API\)<a name="reserved-nodes-offerings-api"></a>

To get pricing and information about available reserved node offerings, call the `DescribeReservedCacheNodesOfferings` action\.

**Example**  

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=DescribeReservedCacheNodesOfferings
    &Version=2014-12-01
    &SignatureVersion=4
    &SignatureMethod=HmacSHA256
    &Timestamp=20141201T220302Z
    &X-Amz-Algorithm
    &X-Amz-SignedHeaders=Host
    &X-Amz-Expires=20141201T220302Z
    &X-Amz-Credential=<credential>
    &X-Amz-Signature=<signature>
```

For more information, see [DescribeReservedCacheNodesOfferings](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReservedCacheNodesOfferings.html) in the ElastiCache API Reference\.

### Purchasing a reserved node<a name="reserved-nodes-purchasing"></a>

The following examples show how to purchase a reserved node offering using the AWS Management Console, the AWS CLI, and the ElastiCache API\. 

**Important**  
 Following the examples in this section incurs charges on your AWS account that you can't reverse\. 

**Topics**
+ [Using the AWS Management Console](#reserved-nodes-purchasing-console)
+ [Using the AWS CLI](#reserved-nodes-purchasing-cli)
+ [Using the ElastiCache API](#reserved-nodes-purchasing-api)

#### Purchasing a reserved node \(Console\)<a name="reserved-nodes-purchasing-console"></a>

 This example shows purchasing a specific reserved node offering, *649fd0c8\-cf6d\-47a0\-bfa6\-060f8e75e95f*, with a reserved node ID of *myreservationID*\. 

The following procedure uses the AWS Management Console to purchase the reserved node offering by offering id\.

**To purchase reserved nodes**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation list, choose the **Reserved Nodes** link\.

1. Choose the **Purchase reserved nodes** button\.

1. For **Engine**, choose Redis\.

1. To determine the available offerings, make selections for the following options:
   + **Node Type**
   + **Term**
   + **Offering Type**
   + An optional **Reserved node ID**

   After you make these selections, the cost per node and total cost of your selections is shown under **Reservation details**\.

1. Choose **Purchase**\. 

#### Purchasing a reserved node \(AWS CLI\)<a name="reserved-nodes-purchasing-cli"></a>

 The following example shows purchasing the specific reserved cluster offering, *649fd0c8\-cf6d\-47a0\-bfa6\-060f8e75e95f*, with a reserved node ID of *myreservationID*\. 

 Type the following command at a command prompt: 

For Linux, macOS, or Unix:

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

#### Purchasing a reserved node \(ElastiCache API\)<a name="reserved-nodes-purchasing-api"></a>

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

For more information, see [PurchaseReservedCacheNodesOffering](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_PurchaseReservedCacheNodesOffering.html) in the ElastiCache API Reference\.

### Getting info about your reserved nodes<a name="reserved-nodes-describing"></a>

You can get information about the reserved nodes you've purchased using the AWS Management Console, the AWS CLI, and the ElastiCache API\.

**Topics**
+ [Using the AWS Management Console](#reserved-nodes-describing-console)
+ [Using the AWS CLI](#reserved-nodes-describing-cli)
+ [Using the ElastiCache API](#reserved-nodes-describing-api)

#### Getting info about your reserved nodes \(Console\)<a name="reserved-nodes-describing-console"></a>

The following procedure describes how to use the AWS Management Console to get information about the reserved nodes you purchased\.

**To get information about your purchased reserved nodes**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation list, choose the **Reserved nodes** link\.

    The reserved nodes for your account appear in the Reserved nodes list\. You can choose any of the reserved nodes in the list to see detailed information about the reserved node in the detail pane at the bottom of the console\. 

#### Getting info about your reserved nodes \(AWS CLI\)<a name="reserved-nodes-describing-cli"></a>

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
     "DataTiering": "disabled"
    "Duration": "31536000",
    "ProductDescription": "memcached",
    "OfferingType": "Medium Utilization",
    "MaxRecords": 0
}
```

For more information, see [describe\-\-reserved\-cache\-nodes](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-reserved-cache-nodes.html) in the AWS CLI Reference\.

#### Getting info about your reserved nodes \(ElastiCache API\)<a name="reserved-nodes-describing-api"></a>

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

For more information, see [DescribeReservedCacheNodes](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReservedCacheNodes.html) in the ElastiCache API Reference\.