# Use Cases and How ElastiCache Can Help<a name="elasticache-use-cases"></a>

Whether serving the latest news, a Top\-10 leaderboard, a product catalog, or selling tickets to an event, speed is the name of the game\. The success of your website and business is significantly affected by the speed at which you deliver content\. According to the New York Times in 2012, in "[For Impatient Web Users, an Eye Blink Is Just Too Long to Wait](http://www.nytimes.com/2012/03/01/technology/impatient-web-users-flee-slow-loading-sites.html?pagewanted=all&_r=0)," users can register a 250\-millisecond \(1/4 second\) difference between competing sites\. They tend to opt out of the slower site in favor of the faster site\. Tests done at Amazon in 2007, cited in [How Webpage Load Time Is Related to Visitor Loss](http://pearanalytics.com/blog/2009/how-webpage-load-time-related-to-visitor-loss/), revealed that for every 100\-ms \(1/10 second\) increase in load time, sales decrease 1 percent\. If someone wants data, whether for a webpage or a report that drives business decisions, you can deliver that data much faster if it's cached\. Can your business afford to not cache your webpages so as to deliver them with the shortest latency possible?

It might be intuitively obvious that you want to cache your most heavily requested items\. But why not cache your less frequently requested items? Even the most optimized database query or remote API call is going to be noticeably slower than retrieving a flat key from an in\-memory cache\. *Noticeably slower* tends to send customers elsewhere\.

The following examples illustrate some of the ways using ElastiCache can improve overall performance of your application\.

**Topics**
+ [In\-Memory Data Store](#elasticache-use-cases-data-store)
+ [Gaming Leaderboards \(Redis Sorted Sets\)](#elasticache-for-redis-use-cases-gaming)
+ [Messaging \(Redis Pub/Sub\)](#elasticache-for-redis-use-cases-messaging)
+ [Recommendation Data \(Redis Hashes\)](#elasticache-for-redis-use-cases-recommendations)
+ [Other Redis Uses](#elasticache-for-redis-use-cases-other)
+ [ElastiCache Customer Testimonials](#elasticache-use-cases-testimonials)

## In\-Memory Data Store<a name="elasticache-use-cases-data-store"></a>

The primary purpose of an in\-memory key\-value store is to provide ultrafast \(submillisecond\) latency and inexpensive access to copies of data\. Most data stores have areas of data that are frequently accessed but seldom updated\. Additionally, querying a database is always slower and more expensive than locating a key in a key\-value pair cache\. Some database queries are especially expensive to perform, for example, queries that involve joins across multiple tables or queries with intensive calculations\. By caching such query results, you pay the price of the query once and then are able to quickly retrieve the data multiple times without having to re\-execute the query\.

The following image shows ElastiCache caching\.

![\[Image: ElastiCache Claching\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/./images/ElastiCache-Caching.png)

### What Should I Cache?<a name="elasticache-use-cases-data-store-what-to-cache"></a>

When deciding what data to cache, consider these factors:

**Speed and Expense** – It's always slower and more expensive to acquire data from a database than from a cache\. Some database queries are inherently slower and more expensive than others\. For example, queries that perform joins on multiple tables are significantly slower and more expensive than simple, single table queries\. If the interesting data requires a slow and expensive query to acquire, it's a candidate for caching\. If acquiring the data requires a relatively quick and simple query, it might still be a candidate for caching, depending on other factors\.

**Data and Access Pattern** – Determining what to cache also involves understanding the data itself and its access patterns\. For example, it doesn't make sense to cache data that is rapidly changing or is seldom accessed\. For caching to provide a meaningful benefit, the data should be relatively static and frequently accessed, such as a personal profile on a social media site\. Conversely, you don't want to cache data if caching it provides no speed or cost advantage\. For example, it doesn't make sense to cache webpages that return the results of a search because such queries and results are almost always unique\.

**Staleness** – By definition, cached data is stale data—even if in certain circumstances it isn't stale, it should always be considered and treated as stale\. In determining whether your data is a candidate for caching, you need to determine your application's tolerance for stale data\. Your application might be able to tolerate stale data in one context, but not another\. For example, when serving a publicly traded stock price on a website, staleness might be acceptable, with a disclaimer that prices might be up to *n* minutes delayed\. But when serving up the price for the same stock to a broker making a sale or purchase, you want real\-time data\.

In summary, consider caching your data if the following is true:
+ It is slow or expensive to acquire when compared to cache retrieval\.
+ It is accessed with sufficient frequency\.
+ It is relatively static, or if rapidly changing, staleness is not a significant issue\.

For more information, see the following:
+ [https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Strategies.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Strategies.html) in the ElastiCache for Redis User Guide

## Gaming Leaderboards \(Redis Sorted Sets\)<a name="elasticache-for-redis-use-cases-gaming"></a>

Redis sorted sets move the computational complexity associated with leaderboards from your application to your Redis cluster\.

Leaderboards, such as the Top 10 scores for a game, are computationally complex, especially with a large number of concurrent players and continually changing scores\. Redis sorted sets guarantee both uniqueness and element ordering\. Using Redis sorted sets, each time a new element is added to the sorted set it's reranked in real time\. It's then added to the set in its appropriate numeric position\. 

In the following diagram, you can see how an ElastiCache for Redis gaming leaderboard works\.

![\[\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/./images/ElastiCache-Redis-Gaming.png)

**Example \- Redis Leaderboard**  
In this example, four gamers and their scores are entered into a sorted list using `ZADD`\. The command `ZREVRANGEBYSCORE` lists the players by their score, high to low\. Next, `ZADD` is used to update June's score by overwriting the existing entry\. Finally, `ZREVRANGEBYSCORE` list the players by their score, high to low, showing that June has moved up in the rankings\.  

```
ZADD leaderboard 132 Robert
ZADD leaderboard 231 Sandra
ZADD leaderboard 32 June
ZADD leaderboard 381 Adam
			
ZREVRANGEBYSCORE leaderboard +inf -inf
1) Adam
2) Sandra
3) Robert
4) June

ZADD leaderboard 232 June

ZREVRANGEBYSCORE leaderboard +inf -inf
1) Adam
2) June
3) Sandra
4) Robert
```
The following command lets June know where she ranks among all the players\. Because ranking is zero\-based, *ZREVRANK* returns a 1 for June, who is in second position\.  

```
ZREVRANK leaderboard June 
1
```

For more information, see the [Redis Documentation](http://redis.io/commands#sorted_set) on sorted sets\.

## Messaging \(Redis Pub/Sub\)<a name="elasticache-for-redis-use-cases-messaging"></a>

When you send an email message, you send it to one or more specified recipients\. In the pub/sub paradigm, you send a message to a specific channel not knowing who, if anyone, receives it\. Recipients of the message are those who are subscribed to the channel\. For example, suppose that you subscribe to the *news\.sports\.golf* channel\. You and all others subscribed to the *news\.sports\.golf* channel receive any messages published to *news\.sports\.golf*\.

Redis pub/sub functionality has no relation to any key space\. Therefore, it doesn't interfere on any level\. In the following diagram, you can find an illustration of ElastiCache for Redis messaging\.

![\[\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/./images/ElastiCache-Redis-PubSub.png)

### Subscribing<a name="elasticache-use-cases-messaging-subscribing"></a>

To receive messages on a channel, you must subscribe to the channel\. You can subscribe to a single channel, multiple specified channels, or all channels that match a pattern\. To cancel a subscription, you unsubscribe from the channel specified when you subscribed to it or the same pattern you used if you subscribed using pattern matching\.

**Example \- Subscription to a Single Channel**  
To subscribe to a single channel, use the SUBSCRIBE command specifying the channel you want to subscribe to\. In the following example, a client subscribes to the *news\.sports\.golf* channel\.  

```
SUBSCRIBE news.sports.golf
```
After a while, the client cancels their subscription to the channel using the UNSUBSCRIBE command specifying the channel to unsubscribe from\.  

```
UNSUBSCRIBE news.sports.golf
```

**Example \- Subscriptions to Multiple Specified Channels**  
To subscribe to multiple specific channels, list the channels with the SUBSCRIBE command\. In the following example, a client subscribes to both the *news\.sports\.golf*, *news\.sports\.soccer* and *news\.sports\.skiing* channels\.  

```
SUBSCRIBE news.sports.golf news.sports.soccer news.sports.skiing
```
To cancel a subscription to a specific channel, use the UNSUBSCRIBE command specifying the channel to unsubscribe from\.  

```
UNSUBSCRIBE news.sports.golf
```
To cancel subscriptions to multiple channels, use the UNSUBSCRIBE command specifying the channels to unsubscribe from\.  

```
UNSUBSCRIBE news.sports.golf news.sports.soccer
```
To cancel all subscriptions, use `UNSUBSCRIBE` and specify each channel or UNSUBSCRIBE without specifying any channel\.  

```
UNSUBSCRIBE news.sports.golf news.sports.soccer news.sports.skiing
```
or  

```
UNSUBSCRIBE
```

**Example \- Subscriptions Using Pattern Matching**  
Clients can subscribe to all channels that match a pattern by using the PSUBSCRIBE command\.  
In the following example, a client subscribes to all sports channels\. Rather than listing all the sports channels individually, as you do using `SUBSCRIBE`, pattern matching is used with the `PSUBSCRIBE` command\.  

```
PSUBSCRIBE news.sports.*
```

**Example Canceling Subscriptions**  
To cancel subscriptions to these channels, use the `PUNSUBSCRIBE` command\.  

```
PUNSUBSCRIBE news.sports.*
```
The channel string sent to a \[P\]SUBSCRIBE command and to the \[P\]UNSUBSCRIBE command must match\. You cannot `PSUBSCRIBE` to *news\.\** and `PUNSUBSCRIBE` from *news\.sports\.\** or `UNSUBSCRIBE` from *news\.sports\.golf*\.

### Publishing<a name="elasticache-for-redis-use-cases-messaging-publishing"></a>

To send a message to all subscribers to a channel, use the PUBLISH command, specifying the channel and the message\. The following example publishes the message, “It’s Saturday and sunny\. I’m headed to the links\.” to the *news\.sports\.golf* channel\.

```
PUBLISH news.sports.golf "It's Saturday and sunny. I'm headed to the links."
```

A client cannot publish to a channel to which it is subscribed\.

For more information, see [Pub/Sub](http://redis.io/topics/pubsub) in the Redis documentation\.

## Recommendation Data \(Redis Hashes\)<a name="elasticache-for-redis-use-cases-recommendations"></a>

Using INCR or DECR in Redis makes compiling recommendations simple\. Each time a user "likes" a product, you increment an *item:productID:like* counter\. Each time a user "dislikes" a product, you increment an *item:productID:dislike* counter\. Using Redis hashes, you can also maintain a list of everyone who has liked or disliked a product\. The following diagram illustrates an ElastiCache for Redis real\-time analytics store\.

![\[\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/./images/ElastiCache-Redis-Analytics.png)

**Example \- Likes and Dislikes**  

```
INCR item:38923:likes
HSET item:38923:ratings Susan 1
INCR item:38923:dislikes
HSET item:38923:ratings Tommy -1
```

## Other Redis Uses<a name="elasticache-for-redis-use-cases-other"></a>

The blog post [How to take advantage of Redis just adding it to your stack](http://oldblog.antirez.com/post/take-advantage-of-redis-adding-it-to-your-stack.html) by Salvatore Sanfilippo discusses a number of common database uses and how they can be easily solved using Redis\. This approach removes load from your database and improves performance\.

## ElastiCache Customer Testimonials<a name="elasticache-use-cases-testimonials"></a>

To learn about how businesses like Airbnb, PBS, Esri, and others are using Amazon ElastiCache to grow their businesses with improved customer experience, see [How Others Use Amazon ElastiCache](https://aws.amazon.com/elasticache/testimonials/)\.

You can also watch the [ElastiCache Videos](Tutorials.md#tutorial-videos) for additional ElastiCache customer use cases\.
