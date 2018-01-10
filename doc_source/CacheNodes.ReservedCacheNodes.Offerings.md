# Getting Info About Reserved Node Offerings<a name="CacheNodes.ReservedCacheNodes.Offerings"></a>

Before you purchase reserved nodes, you can get information about available reserved node offerings\. 

The following exampleS show how to get pricing and information about available reserved node offerings using the AWS Management Console, AWS CLI, and ElastiCache API\. 



## Getting Info About Reserved Node Offerings \(Console\)<a name="CacheNodes.ReservedCacheNodes.Offerings.Console"></a>

To get pricing and other information about available reserved cluster offerings using the AWS Management Console, follow the steps in the follwing procedure\.

**To get information about available reserved node offerings**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation list, choose the **Reserved Cache Nodes** link\.

1. Choose the **Purchase Reserved Cache Node** button\.

1. From the **Product Description** drop down list box, choose the engine \- Memcached or Redis\.

1. To determine the available offerings, make selections from the next 3 drop down list boxes:

   + **Cache Node Type**

   + **Term**

   + **Offering Type**

   After you make these selections, the cost per node and total cost of your selections is shows in the **Purchase Reserved Cache Nodes** wizard\.

1.  Choose **Cancel** to avoid purchasing these nodes and incurring charges\. 

## Getting Info About Reserved Node Offerings \(AWS CLI\)<a name="CacheNodes.ReservedCacheNodes.Offerings.CLI"></a>

To get pricing and other information about available reserved node offerings, type the following command at a command prompt:

```
1. aws elasticache describe-reserved-cache-nodes-offerings
```

This operation produces output similar to the following \(JSON format\):

```
{
    "ReservedCacheNodesOfferings": [
        {
            "OfferingType": "Heavy Utilization",
            "FixedPrice": 4328.0,
            "ReservedCacheNodesOfferingId": "0192caa9-daf2-4159-b1e5-a79bb1916695",
            "UsagePrice": 0.0,
            "RecurringCharges": [
                {
                    "RecurringChargeAmount": 0.491,
                    "RecurringChargeFrequency": "Hourly"
                }
            ],
            "ProductDescription": "memcached",
            "Duration": 31536000,
            "CacheNodeType": "cache.r3.4xlarge"
        },

		*********** some output omitted for brevity ***********

        {
            "OfferingType": "Heavy Utilization",
            "FixedPrice": 4132.0,
            "ReservedCacheNodesOfferingId": "fb766e0a-79d7-4e8f-a780-a2a6ed5ed439",
            "UsagePrice": 0.0,
            "RecurringCharges": [
                {
                    "RecurringChargeAmount": 0.182,
                    "RecurringChargeFrequency": "Hourly"
                }
            ],
            "ProductDescription": "redis",
            "Duration": 94608000,
            "CacheNodeType": "cache.r3.2xlarge"
        }
    ]
}
```

For more information, see [describe\-reserved\-cache\-nodes\-offerings](http://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-reserved-cache-nodes-offerings.html) in the AWS CLI Reference\.

## Getting Info About Reserved Node Offerings \(ElastiCache API\)<a name="CacheNodes.ReservedCacheNodes.Offerings.API"></a>

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

 This call returns output similar to the following:

```
<DescribeReservedCacheNodesOfferingsResponse xmlns="http://elasticache.us-west-2.amazonaws.com/doc/2013-06-15/">
  <DescribeReservedCacheNodesOfferingsResult>
    <ReservedCacheNodesOfferings>
      <ReservedCacheNodesOffering>
        <Duration>31536000</Duration>
        <OfferingType>Medium Utilization</OfferingType>
        <CurrencyCode>USD</CurrencyCode>
        <RecurringCharges/>
        <FixedPrice>1820.0</FixedPrice>
        <ProductDescription>memcached</ProductDescription>
        <UsagePrice>0.368</UsagePrice>
        <ReservedCacheNodesOfferingId>438012d3-4052-4cc7-b2e3-8d3372e0e706</ReservedCacheNodesOfferingId>
        <CacheNodeType>cache.m1.large</CacheNodeType>
      </ReservedCacheNodesOffering>
      <ReservedCacheNodesOffering>

      (...some output omitted for brevity...)

      </ReservedCacheNodesOffering>
    </ReservedCacheNodesOfferings>
  </DescribeReservedCacheNodesOfferingsResult>
  <ResponseMetadata>
    <RequestId>5e4ec40b-2978-11e1-9e6d-771388d6ed6b</RequestId>
  </ResponseMetadata>
</DescribeReservedCacheNodesOfferingsResponse>
```

For more information, see [DescribeReservedCacheNodesOfferings](http://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeReservedCacheNodesOfferings.html) in the ElastiCache API Reference\.