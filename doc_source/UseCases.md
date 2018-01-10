# ElastiCache Use Cases<a name="UseCases"></a>

Whether serving up the latest news, a Top\-10 leaderboard, a product catalog, or selling tickets to an event, speed is the name of the game\. The success of your website and business is significantly impacted by the speed at which you deliver content\. According to research reported by the NY Times in 2012, "[For Impatient Web Users, an Eye Blink Is Just Too Long to Wait](http://www.nytimes.com/2012/03/01/technology/impatient-web-users-flee-slow-loading-sites.html?pagewanted=all&_r=0)," users can register a 250 millisecond \(1/4 second\) difference between competing sites and will opt out of the slower site in favor of the faster site\. Tests done at Amazon in 2007, cited in [How Webpage Load Time Is Related to Visitor Loss](http://pearanalytics.com/blog/2009/how-webpage-load-time-related-to-visitor-loss/), revealed that for every 100ms \(1/10 second\) increase in load time, sales would decrease 1%\. If someone wants data, whether for a webpage or a report that drives business decisions, you can deliver that data faster if it is cached, much faster\. Can your business afford to not cache your web pages so as to deliver them with the shortest latency possible?

It may be intuitively obvious that you want to cache your most heavily requested items\. But why would you not want to cache your less frequently requested items? Even the most optimized database query or remote API call is going to be noticeably slower than retrieving a flat key from an in\-memory cache\. Remember, *noticeably slower* is what sends customers elsewhere\.

The following examples illustrate some of the ways using ElastiCache can improve overall performance of your application\.

## In\-Memory Data Cache<a name="UseCases.DataStore"></a>

The primary purpose of an in\-memory key\-value store is to provide ultra\-fast \(sub\-millisecond latency\) and inexpensive access to copies of data\. Most data stores have areas of data that are frequently accessed but seldom updated\. Additionally, querying a database will always be slower and more expensive than locating a key in a key\-value pair cache\. Some database queries are especially expensive to perform, for example, queries that involve joins across multiple tables or queries with intensive calculations\. By caching such query results, you pay the price of the query once and then are able to quickly retrieve the data multiple times without having to re\-execute the query\. 

### What Should I Cache?<a name="UseCases.DataStore.WhatToCache"></a>

When deciding what data to cache you should consider these factors:

**Speed and Expense** – It is always slower and more expensive to acquire data from a database than from a cache\. Some database queries are inherently slower and more expensive than others\. For example, queries that perform joins on multiple tables are significantly slower and more expensive than simple, single table queries\. If the interesting data requires a slow and expensive query to acquire, it is a candidate for caching\. If acquiring the data requires a relatively quick and simple query, it may still be a candidate for caching, depending on other factors\.

**Data and Access Pattern** – Determining what to cache also involves understanding the data itself and its access patterns\. For example, it doesn't make sense to cache data that is rapidly changing or is seldom accessed\. For caching to provide a meaningful benefit, the data should be relatively static and frequently accessed, such as a personal profile on a social media site\. Conversely, you don't want to cache data if caching it provides no speed or cost advantage\. For example, it wouldn't make sense to cache web pages that return the results of a search since such queries and results are almost always unique\.

**Staleness** – By definition, cached data is stale data—even if in certain circumstances it isn't stale, it should always be considered and treated as stale\. In determining whether your data is a candidate for caching, you need to determine your application's tolerance for stale data\. Your application may be able to tolerate stale data in one context, but not another\. For example, when serving up a publicly traded stock price on a web site, staleness might be quite acceptable, along with a disclaimer that prices may be up to *n* minutes delayed\. But, when serving up the price for the same stock to a broker making a sale or purchase you want real\-time data\.

In summary, consider caching your data if:

+ It is slow or expensive to acquire when compared to cache retrieval\.

+ It is accessed with sufficient frequency\.

+ It is relatively static, or if rapidly changing, staleness is not a significant issue\.

For more information, see [Caching Strategies](Strategies.md)\.

## Gaming Leaderboards \(Redis Sorted Lists\)<a name="UseCases.Gaming"></a>

Redis sorted sets move the computational complexity associated with leaderboards from your application to your Redis cluster\.

Leaderboards, such as the Top 10 scores for a game, are computationally complex, especially with a large number of concurrent players and continually changing scores\. Redis sorted sets guarantee both uniqueness and element ordering\. Using Redis sorted sets, each time a new element is added to the sorted set it is re\-ranked in real time and added to the set in its appropriate numeric position\.

**Example \- Redis Leaderboard**  
In this example four gamers and their scores are entered into a sorted list using `ZADD`\. The command `ZREVRANGEBYSCORE` lists the players by their score, high to low\. Next, `ZADD` is used to update June's score by overwriting the existing entry\. Finally `ZREVRANGEBYSCORE` list the players by their score, high to low, showing that June has moved up in the rankings\.  

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
The following command lets June know where she ranks among all the players\. Since ranking is zero\-based, *ZREVRANK* returns a 1 for June who is in second position\.  

```
ZREVRANK leaderboard June 
1
```

For more information, see the [Redis Documentation](http://redis.io/commands#sorted_set) on sorted sets\.

## Messaging \(Redis pub/sub\)<a name="UseCases.Messaging"></a>

When you send an email message, you send it to one or more specified recipients\. In the pub/sub paradigm, you send a message to a specific channel not knowing who, if anyone, will receive it\. Recipients of the message are those who are subscribed to the channel\. For example, suppose you subscribe to the *news\.sports\.golf* channel\. You and all others subscribed to the *news\.sports\.golf* channel will receive any messages published to *news\.sports\.golf*\.

Redis pub/sub functionality has no relation to any key space\. Therefore, it will not interfere on any level\.

### Subscribing<a name="UseCases.Messaging.Subscribing"></a>

To receive messages on a channel you must subscribe to the channel\. You may subscribe to a single channel, multiple specified channels, or all channels that match a pattern\. To cancel a subscription you unsubscribe from the channel specified when you subscribed to it or the same pattern you used if you subscribed using pattern matching\.

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
To cancel all subscriptions, use UNSUBSCRIBE and specify each channel or UNSUBSCRIBE without specifying any channel\.  

```
UNSUBSCRIBE news.sports.golf news.sports.soccer news.sports.skiing
```

```
UNSUBSCRIBE
```

**Example \- Subscriptions Using Pattern Matching**  
Clients can subscribe to all channels that match a pattern by using the PSUBSCRIBE command\.  
In the following example, a client subscribes to all sports channels\. Rather than listing all the sports channels individually, as would be done using SUBSCRIBE, pattern matching is used with the `PSUBSCRIBE` command\.  

```
PSUBSCRIBE news.sports.*
```
To cancel subscriptions to these channels, use the PUNSUBSCRIBE command\.  

```
PUNSUBSCRIBE news.sports.*
```

**Important**  
The channel string sent to a \[P\]SUBSCRIBE command and to the \[P\]UNSUBSCRIBE command must match\. You cannot PSUBSCRIBE to *news\.\** and PUNSUBSCRIBE from *news\.sports\.\** or UNSUBSCRIBE from *news\.sports\.golf*\.

### Publishing<a name="UseCases.Messaging.Publishing"></a>

To send a message to all subscribers to a channel, use the PUBLISH command, specifying the channel and the message\. The following example publishes the message, “It’s Saturday and sunny\. I’m headed to the links\.” to the *news\.sports\.golf* channel\.

```
PUBLISH news.sports.golf "It's Saturday and sunny. I'm headed to the links."
```

A client cannot publish to a channel to which it is subscribed\.

For more information, see [Pub/Sub](http://redis.io/topics/pubsub) in the Redis documentation\.

## Recommendation Data \(Redis Counters & Hashes\)<a name="UseCases.Recommendations"></a>

Redis counters and hashes make compiling recommendations simple\. Each time a user "likes" a product, you increment an *item:productID:like* counter\. Each time a user "dislikes" a product, you increment an *item:productID:dislike* counter\. Using Redis hashes, you can also maintain a list of everyone who has liked or disliked a product\.

**Example \- Likes & Dislikes**  

```
INCR item:38923:likes
HSET item:38923:ratings Susan 1
INCR item:38923:dislikes
HSET item:38923:ratings Tommy -1
```

## Other Redis Uses<a name="UseCases.OtherRedis"></a>

An article by Salvatore Sanfilippo \([How to take advantage of Redis just adding it to your stack](http://oldblog.antirez.com/post/take-advantage-of-redis-adding-it-to-your-stack.html)\) discusses a number of common database uses and how they can be easily solved using Redis, thus removing load from your database and improving performance\.

## Testimonials<a name="UseCases.Testimonials"></a>

Go to [Testimonials](https://aws.amazon.com/elasticache/testimonials/) to read about how businesses like Airbnb, PBS, Esri, and others are leveraging Amazon ElastiCache to grow their businesses with improved customer experience\.