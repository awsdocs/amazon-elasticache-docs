# Viewing ElastiCache Events<a name="ECEvents.Viewing"></a>

ElastiCache logs events that relate to your cluster instances, security groups, and parameter groups\. This information includes the date and time of the event, the source name and source type of the event, and a description of the event\. You can easily retrieve events from the log using the ElastiCache console, the AWS CLI `describe-events` command, or the ElastiCache API action `DescribeEvents`\. 

The following procedures show you how to view all ElastiCache events for the past 24 hours \(1440 minutes\)\.

## Viewing ElastiCache Events \(Console\)<a name="ECEvents.Viewing.CON"></a>

The following procedure displays events using the ElastiCache console\.

**To view events using the ElastiCache console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. To see a list of all available events, in the navigation pane, choose **Events**\.

   On the *Events* screen each row of the list represents one event and displays the event source, the event type \(cache\-cluster, cache\-parameter\-group, cache\-security\-group, or cache\-subnet\-group\), the GMT time of the event, and a description of the event\.

   Using the **Filter** you can specify whether you want to see all events, or just events of a specific type in the event list\.

## Viewing ElastiCache Events \(AWS CLI\)<a name="ECEvents.Viewing.CLI"></a>

To generate a list of ElastiCache events using the AWS CLI, use the command `describe-events`\. You can use optional parameters to control the type of events listed, the time frame of the events listed, the maximum number of events to list, and more\.

The following code lists up to 40 cache cluster events\.

```
aws elasticache describe-events --source-type cache-cluster --max-items 40  
```

The following code lists all events for the past 24 hours \(1440 minutes\)\.

```
aws elasticache describe-events --duration 1440  
```

The output from the `describe-events` command looks something like this\.

```
{
    "Events": [
        {
            "Date": "2017-03-29T22:17:37.781Z", 
            "Message": "Added cache node 0001 in Availability Zone us-west-2a", 
            "SourceIdentifier": "mem01", 
            "SourceType": "cache-cluster"
        }, 
        {
            "Date": "2017-03-29T22:17:37.769Z", 
            "Message": "Cache cluster created", 
            "SourceIdentifier": "mem01", 
            "SourceType": "cache-cluster"
        }
    ]
}
```

For more information, such as available parameters and permitted parameter values, see [https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-events.html](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-events.html)\.

## Viewing ElastiCache Events \(ElastiCache API\)<a name="ECEvents.Viewing.API"></a>

To generate a list of ElastiCache events using the ElastiCache API, use the `DescribeEvents` action\. You can use optional parameters to control the type of events listed, the time frame of the events listed, the maximum number of events to list, and more\.

The following code lists the 40 most recent cache\-cluster events\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DescribeEvents
   &MaxRecords=40
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &SourceType=cache-cluster
   &Timestamp=20150202T192317Z
   &Version=2015-02-02
   &X-Amz-Credential=<credential>
```

The following code lists the cache\-cluster events for the past 24 hours \(1440 minutes\)\.

```
https://elasticache.us-west-2.amazonaws.com/
   ?Action=DescribeEvents
   &Duration=1440
   &SignatureVersion=4
   &SignatureMethod=HmacSHA256
   &SourceType=cache-cluster
   &Timestamp=20150202T192317Z
   &Version=2015-02-02
   &X-Amz-Credential=<credential>
```

The above actions should produce output similar to the following\.

```
<DescribeEventsResponse xmlns="http://elasticache.amazonaws.com/doc/2015-02-02/"> 
    <DescribeEventsResult> 
        <Events> 
            <Event> 
                <Message>Cache cluster created</Message> 
                <SourceType>cache-cluster</SourceType> 
                <Date>2015-02-02T18:22:18.202Z</Date> 
                <SourceIdentifier>mem01</SourceIdentifier> 
            </Event> 
               
 (...output omitted...)
          
        </Events> 
    </DescribeEventsResult> 
    <ResponseMetadata> 
        <RequestId>e21c81b4-b9cd-11e3-8a16-7978bb24ffdf</RequestId> 
    </ResponseMetadata> 
</DescribeEventsResponse>
```

For more information, such as available parameters and permitted parameter values, see [https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEvents.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEvents.html)\.